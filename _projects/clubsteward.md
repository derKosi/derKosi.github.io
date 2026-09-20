---
title: ClubSteward – Overnight Club-Secretary Agent
order: 6
icon: fas fa-trophy
tags: [Python, Strands Agents SDK, GLM, LiteLLM, Policy-as-Data, Human-in-the-Loop, Local AI]
summary: "Vereinssekretär-Agent, der den Inbox overnight abarbeitet und nur für echte Entscheidungen einen Menschen weckt. Strands Agents SDK, GLM via LiteLLM, Policy-as-Data-HITL, vollständig lokal."
category: hackathons
---

# ClubSteward – Overnight Club-Secretary Agent

> **Einreichung:** [Agents for Humans Hackathon](https://agentsforhumans.devpost.com/) (AWS × Devpost, 2026) · Track: *Good Neighbor Agents*
> **Quellcode:** [github.com/derKosi/clubsteward](https://github.com/derKosi/clubsteward) (public)

## Inspiration

Vereine laufen auf ausgebrannten Ehrenamtlichen. Die Vereinssekretärin jedes Sportvereins, jeder Elternvertretung, jeder Pfadfindergruppe verbringt 5–10 Stunden pro Woche mit Mitglieder-Mail: Anmeldungen, Adressänderungen, Spielplan-Fragen — und einmal im Monat ein Brief, der ein menschliches Herz braucht, wie alleinerziehende Eltern, die einen Beitragserlass bitten.

Wir wollten einen Agenten, der die repetitiven 80 % von diesem Teller nimmt — **ohne die 20 % jemals anzufassen, die menschliches Urteil verdienen.** Nicht „KI macht alles", sondern ein Agent, der genau weiß, wo er anhalten muss.

## Was er tut

ClubSteward bearbeitet den Vereins-Inbox über Nacht, unbeaufsichtigt. Jede Mail wird triagiert (strukturierte Extraktion: Intent, Fakten, Konfidenz), das Mitgliederverzeichnis aktualisiert, herzliche markenkonforme Antworten entworfen — und **nur echte Entscheidungsfragen** landen beim Menschen: Hardship-Waiver, Saisonabbrüche mitten drin, Beschwerden. Jede davon wird zu einer Decision card: die Original-Mail, was der Agent verstanden hat, was er vorschlägt — und die exakte Policy-Zeile, die eskaliert hat (*„Warum du?"* — in der Sprache des Vereins). Spam wird still verworfen.

Es wird auf **Bedingungen eskaliert, nicht nur auf Intents**: eine einfache Anmeldung läuft automatisch — dieselbe Anmeldung mit Erwähnung eines Asthma-Sprays wird als `medical` geflaggt und von der `ask_if`-Regel des Vereins gestoppt. Sechs Demo-Vereine in drei Sprachen (Deutsch, Englisch, Spanisch) — Antworten kommen immer in der Sprache des Mitglieds, signiert vom Verein.

Die **Web-Konsole** ist der Morgen der Sekretärin: Mails live durchsteppen (Mail links, Agent-Analyse rechts), mit einer Anweisung genehmigen, jeden Entwurf side-by-side mit der beantworteten Mail prüfen. **Es wird nie automatisch gesendet.** Eine ganze Nacht kostet etwa einen Cent.

## Wie er gebaut ist

- **Strands Agents SDK** (Python): zwei spezialisierte Agenten — ein Triage-Agent mit `structured_output` (Pydantic) und ein Act-Agent mit fünf Custom-Tools — laufen innerhalb der SDK-**HumanInTheLoop-Intervention** mit einem eigenen, policy-getriebenen Approval-Klassifizierer. Read-Tools laufen frei, Writes brauchen Policy oder Menschen, unbekannte Tools failen closed.
- **Policy as Data**: die gesamte Vereins-Governance ist eine 30-zeilige YAML, die Routing (auto/ask/reject), den Laufzeit-Approval-Klassifizierer und den Antwort-Ton steuert. Ehrenamtliche editieren YAML, nicht Code.
- **Web-Konsole**: FastAPI + Vanilla-JS ohne Build-Step — dieselbe Pipeline, die ein Juror live fahren kann: *Reset data → Process next mail → Approve + instruct → Run night/Stop*.
- **GLM (Z.ai)** über LiteLLMs OpenAI-kompatiblen Provider. Keine Cloud, keine Accounts — läuft auf einem Laptop.
- **Eval-Harness**: ein gelabeltes 10-Mail-Korpus regressionstestet die Triage-Genauigkeit bei jeder Änderung.
- **Replay-Modus**: Juroren ohne API-Key spielen eine aufgezeichnete reale Session ab, klar als Aufzeichnung gelabelt.

## Herausforderungen

- „Nur einen Menschen fragen, wenn es darauf ankommt" zu einer **engineered property** machen, nicht einem Vibe. Gelöst mit drei inspizierbaren Schichten — Policy-Route (auto/ask/reject), Tool-Kategorie-Gating, per-Case-Kontext. Dieselbe YAML treibt alle drei.
- **Das Modell war in beide Richtungen overconfident-falsch.** Eine höfliche Nachfrage mit Fee-Relief-Anspielung triagierte als reine „Frage" (unser Eval-Harness fing es — der Fix ist regression-getestet). Eine andere Mail saß bei 90 % Konfidenz auf einer partiellen Adressänderung: Konfidenz allein reichte nicht, also kamen Condition-Flags (`ask_if: billing_address_unclear`) und `min_confidence` dazu — unter der Schwelle fragt der Agent nach, statt zu raten.
- **Governance muss die Sprache des Vereins sprechen**: Decision-Begründungen werden in der Locale des Vereins generiert — ein deutscher Vorstand liest *„Warum du?"*. Policy-Transparenz, die ein Ehrenamtlicher wirklich liest.

## Stolz darauf

- Der Agent **erfindet nichts**: er flaggte eine „Bruder ist schon im Verein"-Aussage, die er nicht verifizieren konnte, statt den Geschwister-Rabatt zu gewähren.
- Eskalation ist **für Nicht-Programmierer erklärbar**: jede Decision-Card zeigt die exakte Policy-Zeile, die den Agenten gestoppt hat.
- Eine komplette, ehrliche Produkt-Schleife — overnight run → morning decision cards → aktualisiertes Register und Outbox — für etwa **einen Cent pro Nacht**, vollständig lokal bis auf den einen LLM-Call.

## Was wir gelernt haben

Autonomie ist ein Spektrum, das man **daten-getrieben** gestalten kann. Der schwerste Teil war nicht, den Agenten fähig zu machen — sondern die Stellen zu engineeren, an denen er *anhalten* muss. Für Ehrenamits-Organisationen ist die Policy-Datei das eigentliche Produkt: Governance, die ein Nicht-Programmierer lesen, editieren und vertrauen kann.

## Ausblick

- IMAP/SMTP-Adapter für echte Postfächer (dieselbe Pipeline, Ordner-Grenzen bleiben)
- Multi-Club-Hosting mit per-Club-Policy-Dateien
- Optionaler Managed Runtime für Vereine, die nicht über Nacht ein Laptop laufen lassen wollen
