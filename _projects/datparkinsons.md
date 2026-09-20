---
title: DaT Parkinson's – SPECT-Klassifikation
order: 7
icon: fas fa-trophy
tags: [Python, PyTorch, Medical ML, DaT-SPECT, DrivenData, Log Loss, Imaging]
summary: "Binärklassifikation von DaT-SPECT-Hirnscans (normal/abnormal) für die DrivenData-Challenge 2026. 3D-CNN auf SPECT-Volumen, Metrik: Log Loss."
category: hackathons
---

# DaT Parkinson's – SPECT-Klassifikation

> **Wettbewerb:** [DaT Parkinson's Challenge](https://www.drivendata.org/competitions/311/dat-parkinsons-challenge/) (DrivenData, 2026)
> **Metrik:** Log Loss · **Preise:** €12.500 / €7.500 / €5.000

## Aufgabe

Binärklassifikation von DaT-SPECT-Hirnscans: normal oder abnormal? DaT-SPECT (Dopamin-Transporter-SPECT) macht dopaminerge Deﬁzite sichtbar – die Bildgebung ist etabliert in der Parkinson-Diagnostik, die visuelle Befundung ist aber untersucherabhängig. Ein robuster Klassifikator unterstützt Standardisierung.

## Lösung

- **3D-CNN-Pipeline** auf SPECT-Volumen (kein 2D-Slicing – die räumliche Information des Striatum ist das Signal)
- Striatum-fokussierte Cropping-Strategie statt Ganzhirn-Input
- Sorgfältiges Kalibrieren der Wahrscheinlichkeiten – bei Log Loss zahlt man für overconfidence
- Reproduzierbares Setup: fixe Seeds, Version-Pinning, Submission-Pipeline

## Warum dieses Projekt

Medical ML ist der härteste Test für ML-Disziplin: kleine Datensätze, klassen-imbalance, kalibrierte Unsicherheit statt nackter Accuracy. Genauso die Fähigkeit, unter Deadline eine belastbare Pipeline zu stehen.

## Status

Submission abgegeben (Deadline 16.09.2026). Auswertung läuft.
