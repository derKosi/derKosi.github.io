---
title: DaT Parkinson's – SPECT-Klassifikation (EN)
order: 10
icon: fas fa-trophy
tags: [Python, PyTorch, LightGBM, Medical ML, DaT-SPECT, DrivenData, Log Loss, Domain Adaptation]
summary: "Binary classification of DaT-SPECT brain scans for the 2026 DrivenData challenge. GBM+SBR features + MIP-CNN against site shift, LCO validation. Final LB score 0.3963 (log loss), AUROC 0.9004."
category: hackathons
permalink: /projects/datparkinsons-en/
---

[Deutsch](/projects/datparkinsons/) · **English** · [Plain words (DE)](/projects/datparkinsons/#in-einfachen-worten)

## In plain words

Doctors use a special brain image (DaT-SPECT) to see whether the dopamine system is diseased — a hint at Parkinson's. Computers can read such images, but this competition made it tricky: the scans came from many hospitals with very different scanners. It's like judging photos taken with **52 different cameras** — each one exposes differently.

Our program learned to *normalize* the images (bring them all to the same standard) and then to answer carefully: not "sick!" or "healthy!", but a probability — and when unsure, it says "fairly unsure." The metric (log loss) punishes confident wrong answers hard. Caution was the goal, not a weakness.

## The problem

Binary classification of DaT-SPECT volumes (NIfTI): `is_pathologic` 0/1. The difficulty wasn't the CNN — it was the dataset:

- **1,362 scans, 615 normal / 747 pathologic** — small and multi-center
- **66 different voxel shapes, 52 different spacings** (1.37–4.42 mm anisotropic)
- Null-voxel median 67 % — radically heterogeneous acquisition
- ~30 % label-image conflicts (SWEDD-like)

## Validation first

The core decision: never trust the public leaderboard as a ruler. Instead:

- **Grouped CV / Leave-Cluster-Out (LCO):** k-means clusters on intensity/FOV proxies as scanner proxies; selection by LCO-mean **and** LCO-worst
- **Two-worlds banding:** strat→LB gap +0.042, LCO floor −0.006, deployment-near +0.014 — the real score landed inside the predicted band (0.38–0.45; final: 0.3963)
- The method's payoff: **LCO-floor AUC 0.8995 predicted the LB AUROC 0.9004 to within 0.001**

## Solution

Two branches that complement each other:

**GBM on domain features.** Isotropic resampling to 2 mm, per-volume normalization (p1/p99 clip on foreground), central 96³ crop; striatal binding ratio features (caudate/putamen VOIs against occipital and TwoBox reference) and asymmetry features — deliberately **without** left-right flip, because asymmetry *is* the clinical signal here. Final: 89 ratio features (raw intensity scale removed — LCO diagnostics showed site overfit).

**MIP-CNN.** Maximum-intensity projections in SBR units (not z-score — z-score/instance norm erases the intensity signal) through EfficientNet-B0.

**Ensemble + calibration.** Logit blend (GBM-heavy) + joint Platt scaling + clip [0.025, 0.975]. An explicit calibration bake-off discarded isotonic: better OOF numbers but 85 extreme probabilities — a single 1e-4 miss costs +0.026 LB. Robustness beat the OOF optimum (deliberately 0.008 worse).

## What didn't work

Honest negatives with numbers — the most valuable part of the writeup:

| Idea | Result |
|------|--------|
| 2.5D CNN (ImageNet backbone on slices) | OOF fine, LB transfer weak (−0.009 vs −0.031) — learns scanner texture |
| Template-ROI features | LCO-worst worsened — discarded |
| Self-supervised pretraining | learned: scanner ID. Cut. |
| Aux-pretraining (A1) | tested negative |
| Isotonic calibration | OOF gain, fatal extreme probs — replaced by Platt+clip |

## The agent meta-experiment

Three LLM-driven workstreams — **original** (experience), **fresh** (blind start without plan context), plus a fourth round as a fresh-eyes control: a second model (Fable/Claude) received the same evidence but deliberately *not* the existing plan. Result: top priorities converged 1:1 — plus three slot-neutral new ideas. Independent double-consultation as a verification pattern, fully archived (prompts + responses). AI assistance transparently documented; only aggregates ever left the machine — never patient data.

## Progression

| Submission | LB log loss | Note |
|---|---|---|
| GBM v1 (isotonic) | 0.5924 | clearly beats prior |
| Ensemble v1b | 0.5838 | CNN transfer weak |
| GBM-ratios (Sub 3) | 0.5110 | site-scale fix |
| **stack_v3 (-fresh)** | **0.3963** | final — band 0.38–0.45 hit |

## Compliance

No PPMI (DUA forbids competition use — organizer-confirmed), pretrained weights only license-checked (MIT/Apache), no UDA-gated collections. Reproducible: fixed seeds, Docker offline validation, identical smoke scores in-container.

## Why this project

Medical ML under real conditions means: small n, distribution shift between clinics, noisy labels — and a metric that punishes overconfidence. The real story isn't the model, it's the validation discipline: site-shift diagnostics, label-noise handling, and calibration under uncertainty.
