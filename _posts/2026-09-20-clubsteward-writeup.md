---
layout: post
title: "ClubSteward: Ein Agent, der weiß, wo er anhalten muss"
date: 2026-09-20 13:00:00 +0200
author: derKosi
lang: de
categories: [AI Agents, Wettbewerbe]
tags: [Strands Agents SDK, Human-in-the-Loop, Policy as Data, GLM, Hackathon, AWS]
---

> *Einreichung für den [Agents for Humans Hackathon](https://agentsforhumans.devpost.com/) (AWS × Devpost) — Track „Good Neighbor Agents". [Quellcode](https://github.com/derKosi/clubsteward) public, [Projektseite](/projects/clubsteward/) mit allen Details.*

## Inspiration

Vereine laufen auf ausgebrannten Ehrenamtlichen. Jede Vereinssekretärin verbringt 5–10 Stunden pro Woche mit Mitglieder-Mail — und einmal im Monat kommt ein Brief, der ein menschliches Herz braucht: ein Elternteil, das um Beitragserlass bittet.

Die Frage war nicht „kann ein Agent Mails beantworten?" — das kann jeder Draft-Agent. Die Frage war: **Wo genau muss er anhalten?**

## Was gebaut wurde

![Konsole mit Decision-Cards](/assets/img/projects/clubsteward/04-decisions.png)

Ein Agent, der den Vereins-Inbox über Nacht abarbeitet: Triage, Register-Updates, Entwürfe. Aber nur die repetitiven 80 %. Alles, was Urteil braucht, wird morgens zu einer **Decision-Card**: die Original-Mail, was der Agent verstanden hat, sein Vorschlag — und die exakte Policy-Zeile, die ihn gestoppt hat. Auf Deutsch, in der Sprache des Vereins: *„Warum du?"*

Das Entscheidende ist die Architektur dahinter:

- **Policy as Data** — die Vereins-Governance ist eine 30-zeilige YAML (auto/ask/reject-Routing, Eskalations-Bedingungen, Antwort-Ton). Ehrenamtliche editieren YAML, nicht Code.
- **Bedingte Eskalation** — dieselbe Anmeldung läuft automatisch; erwähnt sie ein Asthma-Spray, greift `ask_if: medical` und stoppt. Bedingungen, nicht nur Intents.
- **Fail-closed Tools** — Read-Tools laufen frei, Writes brauchen Policy oder Mensch, unbekannte Tools verweigern.

## Die ehrlichen Modellfehler

Zwei Geschichten aus dem Eval-Harness, die mehr sagen als jede Featureliste:

1. Eine höfliche Nachfrage mit Fee-Relief-Anspielung triagierte als reine „Frage" — kein Eskalations-Intent. Gefangen vom 10-Mail-Regressionstest, nicht von uns.
2. Eine Adressänderung mit 90 % Konfidenz, aber partiell. Konfidenz allein reichte nicht: `min_confidence` + `ask_if: billing_address_unclear` — unter der Schwelle fragt der Agent nach, statt zu raten.

Und mein Liebling: Der Agent **erfand nichts**. Bei „Mein Bruder ist schon im Verein" (Geschwister-Rabatt) flaggte er: *nicht verifizierbar — Mensch gefragt.*

## Warum das mehr als ein Hackathon-Projekt ist

Agentic AI wird als Autonomie-Fantasie verkauft. Die interessante Frage ist das Gegenteil: **Autonomie als Spektrum, das man datengetrieben steuert.** Der schwerste Engineering-Teil war nicht, den Agenten fähig zu machen — sondern die Stellen zu bauen, an denen er *anhalten* muss.

Für Vereine ist die Policy-Datei das eigentliche Produkt: Governance, die ein Nicht-Programmierer lesen, editieren und vertrauen kann. Eine Nacht kostet etwa einen Cent. Läuft lokal auf einem Laptop, GLM via LiteLLM, keine Cloud.

Die vollständige Story — Web-Konsole, Replay-Modus für Juroren, der Ausblick auf IMAP/SMTP — steht auf der [Projektseite](/projects/clubsteward/).
