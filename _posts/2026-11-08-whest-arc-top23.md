---
layout: post
title: "Top 23 % bei ARC White-Box Estimation — solo, mit KI-Agenten, bei harter Materie"
date: 2026-11-08 11:00:00 +0100
author: derKosi
lang: de
categories: [Machine Learning, Wettbewerbe]
tags: [ARC, AIcrowd, Mechanistic Interpretability, Autograd, Cumulant Propagation, Experiment Design, AI-assisted Development]
---

> *Ergebnis der [ARC White-Box Estimation Challenge](https://www.aicrowd.com/challenges/arc-white-box-estimation-challenge-2026) (AIcrowd, 2026): **Platz 180 von 800** — als Solo-Teilnehmer, mit KI-Agenten als dev-Team, im ersten wirklich großen KI-getriebenen Entwicklungsprojekt. Arbeits-Repo bleibt privat; dieser Post ist der WRITEUP.*

## Die Aufgabe

Das [Alignment Research Center](https://alignment.org/) hat eine Frage gestellt, die man nicht googlen kann: Gegeben sind nur die *Gewichte* eines zufallsinitialisierten ReLU-Netzes (Phase 2: 1024 breit, 16 tief, He-Init). Sage für jede Position den Erwartungswert des Neurons voraus — **ohne einen einzigen Forward-Pass**, innerhalb eines FLOP-Budgets von 2⁴¹ ≈ 2,2 Billionen Operationen pro Netz. Weniger Budget-Ausnutzung erhöht den Score, jede Vorhersage wird gegen Ground-Truth bewertet.

Das ist kein Kaggle-Wettkampf mit vorgekauten Features. Das ist mathematische Materie: Momenten-Propagation durch 16 Layer, Numerik, Budget-Ökonomie.

## Die Methode

Kern ist eine **faktorisierte Cumulant-Propagation** („kprop", k_max=3) — ein Port des ARC-Referenzalgorithmus, der Mittelwert *und* Varianz-Schätzung durch die Layer trägt, plus zwei eigene Hebel:

- **Budget-Schedule** für die κ3-Historie: nicht alle Historie behalten, sondern nach Budget-Plan degradieren — „l2x10"
- **Multiplikativer Final-Shrink** gegen den systematischen Bias am Netzrand

Der Score lief in vier dokumentierten Stufen: **7,31e-7 → 1,185e-7 → 1,1327e-7 → 1,12914e-7** — die beste bewertete Submission mit **9,8 % des FLOP-Budgets**, also doppelter Bonusraum gegen die Konkurrenz.

## Der interessanteste Befund ist negativ

Der Fehler des Referenzalgorithmus kommt **nicht von Kovarianz-Rauschen**, wie die Intuition sagt. Er ist ein **Closure-Bias der Propagations-Trajektorie** — und der Beweis ist hässlich schön: wenn man *wahre* Kovarianz injiziert, wird es *schlechter*. Drei weitere belastbare Befunde aus dem Laborbuch: Budget-Schedules schlagen „mehr Historie behalten"; synthetische Netze mit denselben Randmomenten flippen das Bias-**Vorzeichen**; und eine label-freie Bias-Kalibrierung brachte allein in Phase 1 −27,6 % adjusted Score.

## Wie das working ging — und was KI dabei tat

Das Projekt lief als Laborbuch, nicht als Code-Dump: **über 20 vorregistrierte Hypothesen mit harten Adopt-Gates** (Hypothese vor der Messung aufgeschrieben, Confirmation nur auf jungfräulichen Seeds, F-Fingerprint-Anker gegen stille Fallbacks), **144 Sonden- und Verifikationsskripte**, 3.197 Zeilen Score-Log, Ground-Truth-Bakes auf eigener GPU. Negativ-Ergebnisse wurden nicht weggeworfen, sondern dokumentiert — die Verwerfungskette *ist* ein Teil des Beitrags.

Und offen gesagt: Das war das erste Projekt, in dem ich KI-Agenten nicht als Autocomplete, sondern als **dev-Team** geführt habe — die Commit-Frequenz (hunderte Commits in knapp zwei Wochen) ist agenten-getrieben, und der Preis dafür ist real: Traceability ist top, Peer-Review durch einen zweiten Menschen findet nicht statt. Die Disziplin (Pre-Registration, Gates, unverifizierte Skripte niemals als Evidenz zu zählen) ist meine Antwort darauf: Sie ersetzt nicht Review, aber sie macht jede Zahl angreifbar-nachprüfbar.

## Die Platzierung, ehrlich

**Platz 180 von 800 — Top 23 %.** Im August lag ich mit der bewerteten Submission noch auf Platz 13 von 43 bewerteten Einreichungen; das Feld ist danach schnell und groß geworden, und ich habe nach dem 29.08. nicht mehr weiteroptimiert, weil die Deadline-Realität (Hackathons, Abgaben) Vorrang hatte. Ich habe nicht gewonnen. Was der Wert ausmacht, ist anders: eine externe, von Dritten verifizierte Platzierung auf einem kompetitiven Feld mit ARC-Bezug — unter Beweisstellung, dass ich ein wissenschaftliches Experiment sauber aufziehen, KI-Agenten als Team führen und ein Ergebnis *ohne* Überzeichnung berichten kann.

Der Rest-Schritt ist dieser Post: Der WRITEUP-Schritt aus meinem Portfolio-Plan ist damit erledigt, das Repo geht in den Hygiene-Archiv-Modus.
