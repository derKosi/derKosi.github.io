---
title: "SignalFlow – Erklärbare Ampelsteuerung (Hackatron-Gewinner 🏆)"
order: 15
icon: fas fa-trophy
tags: [Python, Simulation, Explainable AI, Smart City, Traffic Flow, Max-Pressure, OpenStreetMap, Hackathon]
summary: "Gewinner der Smart-Cities-Challenge beim Hackatron München (MunichTech EXPO 2026). Adaptive, erklärbare Ampelsteuerung: −24,5 % Delay auf identischem Verkehrsstrom bewiesen, jede Umschaltung in natürlicher Sprache begründet. Zero-Dependency-Python, Live-Demo."
category: hackathons
---

> **Wettbewerb:** Hackatron München (MunichTech EXPO 2026, 19.–20.09.2026) · Challenge: *Smart Cities: Adaptive Traffic Flow*
> **Ergebnis:** 🏆 **Gewinner der Smart-Cities-Challenge**
**Deutsch**

> **Live-Demo:** [signalflow.complykit.de](https://signalflow.complykit.de/) · **Quellcode:** nicht öffentlich (PolyForm-NC-Lizenz) — diese Seite ist die öffentliche Writeup-Fassung.

## In einfachen Worten

Ampeln in Städten schalten meist nach festen Zeitplänen — egal, wie viele Autos gerade warten. SignalFlow ist ein Programm, das Ampeln stattdessen nach dem tatsächlichen Stau steuert: Wo die längste Schlange steht, bekommt sie früher Grün. Und weil man einer solchen Maschine nicht blind vertrauen soll, erklärt sie nach jeder Umschaltung in normaler Sprache, *warum* sie geschaltet hat.

Um zu zeigen, dass es wirklich besser wird, lässt SignalFlow zwei Welten gegeneinander antreten: dieselbe Kreuzung, derselbe Verkehr — einmal mit festem Zeitplan, einmal adaptiv. Der Unterschied ist dann messbar, nicht behauptet.

## Der Beweis statt der Behauptung

Die Kernmethodik: **identische Verkehrsströme im A/B-Vergleich.** Gleicher Seed, gleiche Ankunftszeiten (Poisson-Prozesse, deterministisch reproduzierbar) — eine Kreuzung fährt den Festzeitplan, dieselbe Kreuzung den Queue-Pressure-/Max-Pressure-Controller. Damit ist jeder Unterschied im KPI-Dashboard kausal dem Controller zurechenbar.

**Ergebnisse (Rush-Hour-Szenario, 30 min):**

| Metrik | Festzeitplan | Adaptive Steuerung |
|---|---|---|
| Ø Delay pro Fahrzeug | 53,9 s | **40,7 s (−24,5 %)** |
| Verschwendete Grünzeit | — | **−68,4 %** |
| Durchsatz | — | +1,0 % |
| CO₂-Proxy (Idle-Zeit) | — | −24,5 % |

**Demand-Sweep 0,8×–1,1×** (flach und Peak): mittlere Delay-Reduktion **39,9 %** — 54,8 % im ungestauten Bereich, noch 16 % im übersättigten. Ehrlich dazugesagt: Die Simulation kennt die Modellgrenzen — belegt ist der Effekt auf dem simulierten Strom, nicht auf realer Induktionsschleife.

## Was es kann

- **Erklärbare Umschaltungen:** jede Phasenentscheidung wird in natürlicher Sprache begründet (*„Nord-Zufahrt wartet longest seit 42 s — Grünphase verlängert"*), optional mit Audio-Ausgabe
- **Sensor-Feed-Pfad:** dieselbe Pipeline frisst echte Detektor-Zählwerte aus CSV (`arrival_source=csv`) inkl. simuliertem Detektor-Ausfall — der Controller reagiert auf *gemessene* Werte, nicht nur modelleigene Raten
- **District Mode:** lädt das echte OpenStreetMap-Netz ganzer München-Viertel (Messe/ICM Riem, Altstadtring) und steuert alle Knoten gleichzeitig — Netzwerk-Vergleich fixed vs. adaptive
- **Before/After-Dashboard** mit Frame-Replay und KPIs, Tastatursteuerung, `prefers-reduced-motion`

## Wie es gebaut ist

**Python 3.11+ Standardbibliothek only** — null Dependencies, kein `pip install`, kein Build-Step. Die Simulation ist eine diskrete Warteschlangen-Simulation (dt = 1 s, 4 Zufahrten × L/T/R, geschützte Linksabbieger, Rush-Hour-Rampe), deterministisch über Seeds. Das Web-Dashboard läuft über einen kleinen HTTP-Server, ein Cache-Warming-Tool macht die Demo instant. Erklär-KI (Text + Audio) ist optional per `.env` zugeschaltet — die App läuft auch komplett ohne.

## Warum das für mich mehr als ein Hackathon-Projekt ist

Explainability wird oft als Feature-Checkbox verkauft. Hier war sie die Kernentscheidung: eine Steuerung, die jede Umschaltung begründen kann, ist auditiert- und beratungsfähig — dieselbe Denkschule wie bei meinen Compliance-Projekten. Und die Methodik (gleicher Seed, sauberer A/B-Vergleich, Limit-Doku) ist der Teil, den ich in jedes Evaluationsthema mitnehme.

## Stand nach dem Sieg

Live-Demo läuft (inkl. Demo-Polish aus September), Code bleibt unter PolyForm Noncommercial 1.0.0 geschlossen. Dieser Post und die Projektkarte sind der WRITEUP-Schritt — Details zur Contest-Doku (ADR-Log, Prize-Mapping, Handoff-Kit) liegen im Submission-Kit.
