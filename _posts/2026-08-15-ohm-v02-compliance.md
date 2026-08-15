---
layout: post
title: "Ohm v0.2: Vom Aufräum-Tool zum Compliance-Instrument"
date: 2026-08-15
author: derKosi
lang: de
categories: [Projekte]
tags: [Ohm, Go, TUI, Compliance, AI-Hygiene, Privacy, Security]
---

> **TL;DR:** Ohm – der Privacy-First-Scanner für AI-Software – hat ein v0.2-Update:
> **Compliance-Mode** mit CI-Gates, Baselines und Reports, reversibler **Backup-Mode**,
> ein **Credential-Audit**, Custom Signatures und 90+ Signaturen inklusive Browser-
> Extensions und Windows-Registry. Immer noch: null Netzwerk-Code.

Ohm ist aus einer einfachen Frage entstanden: *Was ist eigentlich alles auf meinem
Rechner passiert?* Man testet AI-Tools, installiert Agents und Runtimes, lädt Modelle –
und ein Jahr später frisst `.cache/huggingface` leise 80 GB, und niemand weiß, in
welchen Config-Dateien API-Keys liegen. v0.1 hat das sichtbar gemacht. v0.2 macht es
**steuerbar**.

## Compliance-Mode: Ohm in der Pipeline

Der größte neue Block. Vier Features, die zusammen ein Audit-Setup ergeben:

**`--fail-on <risk>`** gibt Ohm Exit-Codes mit dokumentierter Semantik. Damit lässt
sich ein Scan als Gate in CI-Pipelines oder Onboarding-Checks einbauen: Neue AI-Tools
mit kritischen Findings brechen den Lauf ab. Das Instrument, das vorher nur *zeigte*,
kann jetzt *durchsetzen*.

**Baselines und Diff:** `--save-baseline` friert den Soll-Zustand ein, `--diff`
vergleicht dagegen. Das beantwortet die Compliance-Frage, die vor einem Jahr noch
niemand stellte: *Welche AI-Software ist seit der letzten Prüfung neu auf den
Rechnern gelandet?* Drift wird sichtbar.

**Reports:** `--csv` und `--markdown` erzeugen teilbare Berichte. Für IT-Abteilungen,
die Bestandsaufnahmen brauchen, ist das der Unterschied zwischen „guck mal in die TUI"
und einem anhängbaren Dokument.

**`ohm audit`** prüft Berechtigungen: Findet API-Key-Locations, die für andere lokale
Nutzer lesbar sind. Wichtig – und bewusst eng gefasst: Es werden nur Permissions
geprüft, niemals Inhalte exponiert. Ohm zeigt dir, *dass* ein Key liegt und falsch
berechtigt ist, nicht *welcher*.

## Backup-Mode: Aufräumen ohne Reue

`ohm generate --backup` schreibt jetzt Skripte, die nichts löschen: Gefundene Items
wandern nach `~/.ohm/trash/`, und ein passendes Restore-Skript legt sie bei Bedarf
zurück. reversible cleanup. Für den persönlichen Einsatz ist das Komfort – für
 Skeptische ist es der Unterschied zwischen „probier das Tool" und „ok, das Tool
 kann nichts kaputt machen".

## Detektion: breiter und tiefer

- **90+ Signaturen** über 12 Kategorien: Agents (Claude Code, Aider, Codex CLI,
  Cursor Agent, ...), Runtimes (Ollama, LM Studio, vLLM, KTransformers, MLX, ...),
  ComfyUI mit Checkpoints/LoRAs, Model-Caches, SDKs, Docker, Agent-Memory und
  Session-Histories
- **Browser-AI-Extensions:** erkennt AI-Erweiterungen in Chrome, Edge, Brave,
  Vivaldi, Opera, Arc und Firefox – inklusive deren Local Storage. Ein Bereich,
  den klassische Inventory-Tools auslassen, in dem aber API-Keys und Berechtigungen
  liegen
- **Windows-Registry:** installer-basierte AI-Apps (LM Studio, GPT4All, ...) über
  ihre Uninstall-Entries – der richtige Weg auf Windows statt Dateisystem-Ratelerei
- **Custom Signatures:** eigene Tool-Definitionen in `~/.ohm/signatures/*.yaml`.
  Der häufigste Feature-Wunsch – interne Tools und Nischen-Software ohne Fork
  abdecken zu können

## Distribution: ernst nehmen

Ein Einzelbinary (Go, amd64 + arm64), `install.sh` und `install.ps1`, Manifeste für
Winget, Scoop und Homebrew – und ein **IT-Fleet-Deployment-Guide** für Rollouts auf
Maschinenparks inklusive Compliance-JSON-Verarbeitung. Ohm ist damit nicht mehr nur
ein Entwickler-Tool für den eigenen Rechner, sondern deploybar.

## Was sich NICHT geändert hat

Die Privacy-First-Architektur. Kein HTTP-Client im Codebase, keine Telemetrie,
kein Phone-Home, 100 % offline. Ein Tool, das API-Keys und Projekt-Pfade scannt,
darf selbst nicht senden – das war das Design-Prinzip von v0.1 und bleibt es.
Wenn du Netzwerk-Code in Ohm findest, ist das ein Bug.

Mehr im [Projekt-Portfolio](/projects/ohm/).
