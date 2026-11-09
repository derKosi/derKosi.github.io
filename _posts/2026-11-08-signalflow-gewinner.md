---
description: "SignalFlow gewinnt die Smart-Cities-Challenge beim Hackatron München: −24,5 % Delay auf identischem Verkehrsstrom bewiesen."
layout: post
title: "🏆 SignalFlow gewinnt die Smart-Cities-Challenge beim Hackatron München"
date: 2026-11-08 10:00:00 +0100
author: derKosi
lang: de
categories: [Simulation, Wettbewerbe]
tags: [Explainable AI, Smart City, Traffic Simulation, Hackathon, MunichTech EXPO, Max-Pressure]
---

> *Einreichung für den Hackatron München (MunichTech EXPO 2026, 19.–20.09.) — Challenge „Smart Cities: Adaptive Traffic Flow". **Ergebnis: 🏆 gewonnen.** [Projektseite](/projects/signalflow/) · [Live-Demo](https://signalflow.complykit.de/) — Quellcode bleibt geschlossen (PolyForm-NC), deshalb hier die Story statt des Codes.*

## Die Idee in einem Satz

Ampeln, die nach dem Stau schalten statt nach dem Kalender — und jede Umschaltung *begründen* können.

## Warum das Team (ich) gewonnen haben könnte

Nicht wegen der Idee — Max-Pressure-Steuerung ist Literatur. Sondern wegen der Art, wie sie bewiesen wurde:

**Gleicher Seed, gleicher Verkehr, zwei Welten.** Dieselbe Kreuzung fährt einmal den Festzeitplan und einmal den adaptiven Controller — auf exakt demselben Verkehrsstrom (deterministische Poisson-Ankünfte, reproduzierbar per Seed). Damit ist der Unterschied kein „feelt sich schneller an", sondern kausal dem Controller zurechenbar:

| Metrik (Rush-Hour, 30 min) | Festzeitplan | Adaptiv |
|---|---|---|
| Ø Delay pro Fahrzeug | 53,9 s | **40,7 s (−24,5 %)** |
| Verschwendete Grünzeit | — | **−68,4 %** |
| CO₂-Proxy (Idle) | — | −24,5 % |

Über den Sweep 0,8×–1,1× Nachfrage (flach und Peak) hält die Verbesserung im Mittel **39,9 %** — und im Writeup steht dazugesagt, wo die Grenzen sind: simulierter Strom, keine Felddaten.

**Zweitens: Explainability als Kern, nicht als Deko.** Jede Phasenumschaltung kommt mit einer Begründung in natürlicher Sprache (*„Nord wartet am längsten — Grün verlängert"*), optional gesprochen. Jurymäßig der Punkt, der die Demo von „nette Simulation" zu „Regulierungsdiskussion-tauglich" gehoben hat.

**Drittens: null Dependencies.** Python 3.11+ Standardbibliothek, kein `pip install` — die Demo läuft überall, auch ohne die optionalen KI-Keys. Und der District Mode steuert nicht nur eine Spielzeug-Kreuzung, sondern das echte OpenStreetMap-Netz ganzer München-Viertel (Messe Riem, Altstadtring) gleichzeitig.

## Was danach passierte

20.–22.09. habe ich die Live-Demo poliert ([signalflow.complykit.de](https://signalflow.complykit.de/)), sie steht seitdem. Der Code bleibt unter **PolyForm Noncommercial 1.0.0** geschlossen — die Geschichte und die Ergebnisse sind öffentlich, der Code nicht. [Projektseite mit allen Details](/projects/signalflow/).

## Ehrliche Einordnung

Ein Hackathon-Sieg in einer Challenge-Sparte ist ein Kompetenzsignal, kein Geschäftsmodell: Verkehr ist nicht meine Branche, die NC-Lizenz sperrt Kommerz bewusst. Was bleibt, ist die Methode — identische Streams statt Behauptungen, Begründungen statt Blackbox — und die gehört in jedes meiner Evaluierungs-Themen, vom LLM-Gateway bis zum Compliance-Audit.

*→ [Alle Wettbewerbe auf einen Blick](/wettbewerbe/)*
