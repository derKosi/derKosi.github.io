---
title: DaT Parkinson's – SPECT-Klassifikation
order: 7
icon: fas fa-trophy
tags: [Python, PyTorch, LightGBM, Medical ML, DaT-SPECT, DrivenData, Log Loss, Domain Adaptation]
summary: "Binärklassifikation von DaT-SPECT-Hirnscans für die DrivenData-Challenge 2026. GBM+SBR-Features + MIP-CNN, Kalibrierung unter Site-Shift, LCO-Validierung. Finaler LB-Score 0.3963 (Log Loss), AUROC 0.9004."
category: hackathons
---

> **Wettbewerb:** [DaT Parkinson's Challenge](https://www.drivendata.org/competitions/311/dat-parkinsons-challenge/) (DrivenData, 2026)
> **Metrik:** Log Loss · **Finaler Stand:** 0.3963 · AUROC 0.9004 · Rank #197/744
> Arbeits-Repo privat; diese Seite ist die öffentliche Writeup-Fassung.

## In einfachen Worten

Ärzte machen ein spezielles Hirnbild (DaT-SPECT), um zu sehen, ob das Dopamin-System erkrankt ist — ein Hinweis auf Parkinson. Computer können solche Bilder einordnen, aber hier war es besonders knifflig: Die Bilder kamen von vielen verschiedenen Krankenhäusern mit ganz unterschiedlichen Geräten. Das ist, als müsste man Fotos auswerten, die mit 52 verschiedenen Kameras aufgenommen wurden — jede belichtet anders.

Unser Programm hat gelernt, die Bilder zu *normieren* (alle auf denselben Standard bringen) und dann vorsichtig zu antworten: Nicht „krank!" oder „gesund!", sondern eine Wahrscheinlichkeit — und wenn es unsicher ist, sagt es „ziemlich unsicher". Bei der Bewertung (Log Loss) wird man nämlich hart bestraft, wenn man selbstsicher falsch liegt. Vorsicht ist hier keine Schwäche, sondern das Ziel.

## Das Problem

Binärklassifikation von DaT-SPECT-Volumen (NIfTI): `is_pathologic` 0/1. Die Schwierigkeit war nicht das CNN — sie war der Datensatz:

- **1.362 Scans, 615 normal / 747 pathologisch** — klein und multizentrisch
- **66 verschiedene Voxel-Formen, 52 verschiedene Spacings** (1,37–4,42 mm anisotrop)
- Null-Voxel-Median 67 % — radikal heterogene Acquisition
- ~30 % Label-Bild-Konflikte (SWEDD-artig)

## Validierung zuerst

Die Kernentscheidung war, der Public-Leaderboard nie als Lineal zu benutzen. Stattdessen:

- **Grouped CV / Leave-Cluster-Out (LCO):** k-means-Cluster auf Intensitäts-/FOV-Proxies als Scanner-Proxys; Selektion nach LCO-mean **und** LCO-worst
- **Zwei-Welten-Banding:** strat→LB-Gap +0.042, LCO-Floor −0.006, deployment-nah +0.014 — der echte Score lag in der vorhergesagten Bande (0.38–0.45; final: 0.3963)
- Die Prognosekraft der Methode: **LCO-Floor-AUC 0.8995 sagte den LB-AUROC 0.9004 auf 0.001 voraus**

## Lösung

Zwei Zweige, die sich ergänzen:

**GBM auf Domain-Features.** Isotropes Resampling auf 2 mm, per-Volume-Normalisierung (p1/p99-Clip auf Foreground), zentraler 96³-Crop; dazu Striatal-Binding-Ratio-Features (Caudate/Putamen-VOIs gegen Okzipital- und TwoBox-Referenz) und Asymmetrie-Features — bewusst **ohne** Links-Rechts-Flip, weil Asymmetrie hier das klinische Signal *ist*. Final: 89 Ratio-Features (rohe Intensitäts-Scale raus — LCO-Diagnose zeigte Site-Overfit).

**MIP-CNN.** Maximum-Intensity-Projections in SBR-Einheiten (nicht Z-Score — Z-Score/InstanceNorm löschen das Intensitätssignal) durch EfficientNet-B0.

**Ensemble + Kalibrierung.** Logit-Blend (GBM-lastig) + gemeinsame Platt-Skalierung + Clip [0.025, 0.975]. Ein expliciter Kalibrierungs-Bake-off verwarf Isotonic: bessere OOF-Zahlen, aber 85 Extremwahrscheinlichkeiten — ein einziger 1e-4-Fehlschlag kostet +0.026 LB. Robustheit schlug OOF-Optimum (bewusst 0.008 schlechter).

## Was nicht funktionierte

Ehrliche Negative mit Zahlen — der wertvollste Teil des Writeups:

| Idee | Ergebnis |
|------|----------|
| CNN 2.5D (ImageNet-Backbone auf Slices) | OOF ok, LB-Transfer schwach (−0.009 statt −0.031) — lernt Scanner-Textur |
| Template-ROI-Features | LCO-worst verschlechtert — verworfen |
| Self-Supervised Pretraining | gelernt: Scanner-ID. Gestrichen. |
| Aux-Pretraining (A1) | negativ getestet |
| Isotonic-Kalibrierung | OOF-Gewinn, Extremprobs fatal — ersetzt durch Platt+Clip |

## Der Agenten-Meta-Versuch

Drei LLM-getriebene Arbeitsstränge — **original** (Erfahrung), **fresh** (Blindstart ohne Plan-Kontext), plus eine vierte Runde als Fresh-Eyes-Kontrolle: ein zweites Modell (Fable/Claude) bekam dieselbe Evidence, aber bewusst *nicht* den bestehenden Plan. Ergebnis: Konvergenz der Top-Prioritäten 1:1 — und drei slot-neutrale neue Ideen. Unabhängige Doppel-Beratung als Verifikationsmuster, komplett archiviert (Prompts + Responses). AI-Assistenz transparent dokumentiert, nur Aggregate übertragen — nie Patientendaten.

## Verlauf

| Submission | LB Log Loss | Anmerkung |
|---|---|---|
| GBM v1 (isotonic) | 0.5924 | schlägt Prior klar |
| Ensemble v1b | 0.5838 | CNN-Transfer schwach |
| GBM-ratios (Sub 3) | 0.5110 | Site-Scale-Fix |
| **stack_v3 (-fresh)** | **0.3963** | Finale — Bande 0.38–0.45 getroffen |

## Compliance

Kein PPMI (DUA verbietet Wettbewerbsnutzung — Organizer-Bestätigung), Pretrained-Weights nur nach Lizenz (MIT/Apache), keine UDA-gated Kollektionen. Reproduzierbar: fixe Seeds, Docker-Offline-Validierung, identische Smoke-Scores im Container.

## Warum dieses Projekt

Medical ML unter echten Bedingungen heißt: kleines n, Verteilungs-Shift zwischen Kliniken, verrauschte Labels — und eine Metrik, die Overconfidence bestraft. Die eigentliche Geschichte ist nicht das Modell, sondern die Validierungs-Disziplin: Site-Shift-Diagnostik, Label-Noise-Handling und Kalibrierung unter Unsicherheit.
