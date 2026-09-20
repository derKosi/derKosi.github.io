---
layout: post
title: "DaT Parkinson's Challenge: 0.59 → 0.40 Log Loss — die Geschichte hinter dem Score"
date: 2026-09-20 12:30:00 +0200
author: derKosi
lang: de
categories: [Machine Learning, Wettbewerbe]
tags: [DrivenData, Medical ML, DaT-SPECT, Kalibrierung, Domain Shift, LightGBM, Validierung]
---

> *Gastbeitrag von der eigenen Werkbank: Der komplette DrivenData-Wettkampf — Multicenter-SPECT, Site-Shift, Label-Noise — und warum der Score am Ende die kleinste der interessanten Zahlen war. [Projektseite mit technischen Details](/projects/datparkinsons/).*

## In einfachen Worten

Ärzte nutzen ein spezielles Hirnbild (DaT-SPECT), um das Dopamin-System sichtbar zu machen — ein Hinweis auf Parkinson. Computer können solche Bilder einordnen. Aber: Die Bilder dieses Wettbewerbs kamen aus vielen Kliniken mit ganz unterschiedlichen Geräten. Das ist, als müsste man Fotos auswerten, die mit **52 verschiedenen Kameras** aufgenommen wurden — jede belichtet anders.

Unser Programm lernte, alle Bilder auf einen gemeinsamen Standard zu normieren — und dann *vorsichtig* zu antworten: eine Wahrscheinlichkeit statt eines Urteils, und bei Unsicherheit ein ehrliches „ziemlich unsicher". Die Metrik (Log Loss) bestraft selbstsicheres Falschliegen brutal. **Vorsicht war hier das Ziel, nicht Schwäche.**

## Der Datensatz war das Problem

1.362 Scans, 66 verschiedene Voxel-Formen, 52 Spacings (1,37–4,42 mm), Null-Voxel-Median 67 %, rund 30 % Label-Bild-Konflikte (SWEDD-artig). Ein CNN zu bauen ist bei so etwas der leichte Teil.

## Validierung vor Modell

Die wichtigste Entscheidung: dem Public-Leaderboard nie zu vertrauen.

- **Leave-Cluster-Out (LCO):** Scanner-Cluster als Proxy für Kliniken; Selektion nach LCO-mean *und* LCO-worst
- **Zwei-Welten-Banding:** Wir bandeten den erwarteten LB-Score vor dem Upload (0.38–0.45). Final: **0.3963** — in der Bande.
- Die schönste Zahl des Wettbewerbs: **LCO-Floor-AUC 0.8995 sagte den LB-AUROC 0.9004 auf 0.001 voraus.** Die Validierungsmethode war besser als das Modell.

## Was wir lernten (die harten Lektionen)

- **Z-Score-Normalisierung löscht das Signal.** SPECT-Intensität IST die Information — Standardisierung pro Volume (p1/p99-Clip) statt Z-Score pro Scanner.
- **CNNs lernen Scanner-ID.** Das 2.5D-CNN lief gut in CV und transferierte schwach auf den LB — Diagnose: es lernte Site-Textur, nicht Pathologie. Konsequenz: Ratios statt roher Skalen, GRL gegen Site-Adversarial.
- **Isotonic-Kalibrierung ist eine Extremprobs-Falle.** Besser in OOF, aber 85 Wahrscheinlichkeiten nahe 0/1 — ein einziger 1e-4-Fehlschlag kostet +0.026 LB. Finale Wahl: Platt + Clip [0.025, 0.975], bewusst 0.008 OOF schlechter.
- **Negative Ergebnisse sind Kapital.** Template-ROIs, Aux-Pretraining, SSL-Pretraining (lernte: Scanner-ID) — alles dokumentiert und begraben, mit Zahlen.

## Der Agenten-Meta-Versuch

Nebenbei lief ein Experiment: dieselbe Evidence zwei LLM-Beratungsrunden — einmal mit vollem Plan-Kontext (Erfahrung), einmal bewusst *ohne* (Fresh Eyes, anderes Modell). Ergebnis: **Konvergenz der Top-Prioritäten 1:1** — plus drei slot-neutrale neue Ideen und zwei dokumentierte Divergenzen zur Empirie. Unabhängige Doppel-Beratung als Verifikationsmuster; Prompts und Responses vollständig archiviert.

## Der Verlauf

- Sub 1 (GBM v1): **0.5924**
- Sub 2 (Ensemble v1b): 0.5838 — CNN-Transfer schwach
- Sub 3 (GBM-ratios): 0.5110 — Site-Scale-Fix
- Final (stack_v3, -fresh-Track): **0.3963** · AUROC 0.9004

Warum das Fazit trotzdem „Validierung first" heißt — und die komplette Negative-Results-Tabelle, Compliance-Entscheidungen (PPMI-DUA, Weight-Lizenzen) und Pipeline-Details — steht auf der [Projektseite](/projects/datparkinsons/).
