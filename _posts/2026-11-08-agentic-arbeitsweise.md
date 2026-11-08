---
layout: post
title: "KI-Agenten als dev-Team: meine Arbeitsweise, mit Zahlen"
date: 2026-11-08 16:00:00 +0100
author: derKosi
lang: de
categories: [AI Agents, Arbeitsweise]
tags: [Agentic Development, Claude, GLM, Coding Agents, Pre-Registration, Review Gates, ComplyCoder]
---

> *Wohl die häufigste Interviewfrage an mich: „Wie arbeitest du konkret mit KI-Agenten?" — dieser Post ist die Antwort, mit nachprüfbaren Zahlen aus Projekten, deren Evidenz öffentlich dokumentiert ist.*

## Die falsche Frage und die richtige

„Nutzt du Copilot?" ist die falsche Frage. Autocomplete ändert die Produktivität, nicht die Architektur der Arbeit. Die richtige Frage ist: **Was übernimmt der Agent, was der Mensch, und wie wird verhindert, dass der Agent Selbstvertrauen mit Richtigkeit verwechselt?**

Meine Antwort darauf hat sich über drei Projekte 2026 herauskristallisiert — und sie ist keine Vibe-Methodik, sondern besteht aus Mechanismen, die man angreifen kann.

## Baustein 1: Agenten als dev-Team — mit Gates statt Vertrauen

Bei der [ARC White-Box Estimation Challenge]({% post_url 2026-11-08-whest-arc-top23 %}) habe ich Agenten erstmals nicht als Werkzeug, sondern als *Team* geführt: hunderte Commits in unter zwei Wochen, 144 Sonden-/Verifikationsskripte, 3.197 Zeilen Mess-Laborbuch. Der Preis dieser Frequenz ist real: **Traceability top, menschliches Peer-Review null.** Meine Antwort war die Disziplin des Labors:

- **Pre-Registration:** Hypothese wird *vor* der Messung aufgeschrieben — über 20 mal, jede mit hartem Adopt-Gate
- **Confirmation nur auf jungfräulichen Seeds** — kein Ergebnis darf auf den Daten bestätigt werden, auf denen es gefunden wurde
- **F-Fingerprint-Anker** gegen stille Fallbacks: ein Skript, das unbemerkt auf einen anderen Codepfad läuft, fällt beim Fingerabdruck auf
- **Negative Ergebnisse bleiben dokumentiert** — die Verwerfungskette ist Teil des Beitrags, nicht Müll

Das Resultat: Platz 180/800, beste Submission mit 9,8 % des FLOP-Budgets — und jede Zahl im Laborbuch ist nachprüfbar, auch für niemanden, der mir vertraut.

## Baustein 2: Agenten im unsafe Modus — Containment vor Kompetenz

Wenn Agenten *ausführen* statt nur schreiben, verschiebt sich das Problem: Die Frage ist nicht mehr „ist die Antwort richtig?", sondern „was kann der Agent anrichten?". [agent-rush]({% post_url 2026-11-08-agent-rush-eingereicht %}) ist die Konsequenz: untrusted Code kommt in einen Container, dessen Policy ausführbare Flags sind (Egress-Deny-Proxy, read-only rootfs, non-root, cap-drop ALL) — und am Ende stehen **Roh-Evidenz-Artefakte** neben dem Verdict, gegen die man die Behauptungen des Agenten loggen kann. „Evidenz statt Behauptung" gilt dabei auch für den Agenten selbst: die Doku listet offen, was die Sandbox *nicht* kann.

Dieselbe Denkschule steckt in [ClubSteward]({% post_url 2026-09-20-clubsteward-writeup %}): Policy-as-Data mit `ask_if`-Bedingungen, fail-closed Tools, und der Agent, der *nichts erfindet* — eine unverifizierbare Behauptung wird eskaliert, nicht geglaubt.

## Baustein 3: Die Kodifizierung — ComplyCoder

Die Arbeitsweise aus 1 und 2 habe ich in ein eigenes System gegossen: **ComplyCoder** — ein selbst laufendes dev-Team (Manager für Ticket-Triage mit WIP-Limit, Worker-/QA-Agenten pro Ticket in isolierten Worktrees, Verifikations-Leiter von Gates über QA mit anderem Modell bis zum Menschen), auf eigenem Forgejo, End-to-End gelaufen. Bewusst **handgerollt statt Framework**: keine CrewAI-/LangChain-Baukästen, sondern Python-Stdlib und Bash-Harness-Adapter — weil mir bei Systemen, die andere Systeme bauen, jede Abstraktionsschicht ein zusätzliches Vertrauensproblem ist. ComplyCoder ist interne Produktions-Infrastruktur (privat, keine Produktwerbung); was öffentlich nachprüfbar ist, sind die Ergebnisse, die die Arbeitsweise daraus ableitbar macht — die Projekte und Wettbewerbe auf dieser Site.

## Was die Zahlen sagen

| Projekt | Agenten-Rolle | Mensch-Rolle | Evidenz |
|---|---|---|---|
| whest (ARC) | dev-Team: Skripte, Sonden, Doku | Architektur, Gates, alle Adoptionen | 180/800, Laborbuch öffentlich dokumentiert |
| agent-rush | Pipeline, Sandbox-Doku, Tests | Threat-Model, Containment-Invarianten | 109 Tests, E2E-Run mit Evidenz-Artefakten |
| ClubSteward | Implementierung, Eval-Harness | Policy-Design, Human-in-the-Loop-Konzept | 10-Mail-Regression, Replay-Modus für Juroren |

## Die Transferthese

Alles drei ist dieselbe Kernkompetenz in verschiedenen Risikozonen: **Autonomie ist ein Spektrum, das man engineeren muss** — mit Gates, Containment und Evidenz statt Vertrauen. Genau das ist auch die Compliance-Frage des EU AI Act in klein: Wo genau muss ein System anhalten, und wie weist man nach, dass es angehalten hat? Ich habe die Antwort nicht in der Theorie gelernt, sondern in Projekten gebaut, die man klicken kann: [Projekte](/projects/), [Wettbewerbe](/wettbewerbe/), [Zertifikate](/zertifikate/).
