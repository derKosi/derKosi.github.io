---
title: Ohm – Privacy-First AI-Scanner
order: 3
icon: fas fa-bolt
tags: [Go, Bubble Tea, TUI, Privacy, Offline, AI-Hygiene, Security]
summary: "Cross-platform TUI-Scanner für AI-Software: erkennt installierte Agents, Modelle, Config-Dateien und Credentials – 100 % offline, kein HTTP-Client im Codebase."
---

# Ohm – Privacy-First AI-Scanner

> **Tagline:** *Resistance against AGI bloat.*
> Ohm misst, was da ist – und hilft dir, zu entfernen, was du nicht brauchst.

## Problem

Du testest AI-Tools. Du installierst Agents, Harnesses, Runtimes, lädst Modelle herunter, richtest SDKs ein. Monate später ist deine Platte voll mit 14 GB Modell-Dateien von diesem einen Experiment, drei AI-Editoren, die du vergessen hast, und einem `.cache/huggingface`-Verzeichnis, das leise auf 80 GB angewachsen ist.

Schlimmer noch: AI-Config-Dateien enthalten oft API-Keys, Projektpfade und persönliche Instruktionen. Diese Daten sind sensibel – und niemand weiß, wo sie alle liegen.

## Lösung

Ohm ist ein cross-platform (Windows, macOS, Linux) TUI-Tool, das dein System nach AI-bezogener Software durchsucht, dir zeigt, was installiert ist, wie viel Platz es frisst, und ein Deinstallations-Skript generiert, das du vor der Ausgabe prüfen kannst. **Ohm löscht selbst nie etwas.**

### Privacy-First-Architektur

Das ist eine bewusste Designentscheidung und ein zentraler Differenzierer:

- **Kein HTTP-Client im Codebase** – Ohm hat keine Netzwerkfähigkeit, keine Telemetrie, kein Phone-Home
- **100 % offline** – Scan-Ergebnisse werden nie übertragen
- **Lokaler State** – `~/.ohm/state.json`, bleibt auf deinem Rechner
- **Generierte Skripte laufen lokal** – dein Rechner, deine Kontrolle

> Wenn du Netzwerk-Code in Ohm findest, ist das ein Bug.

### Scan-Kategorien

| # | Kategorie | Was es findet |
|---|-----------|---------------|
| 1 | Agents & Harnesses | Claude Code, Aider, Continue, Cline, Codex CLI, Cursor Agent, etc. |
| 2 | Modelle & Caches | Hugging Face, Ollama, LM Studio, etc. |
| 3 | Config-Verzeichnisse | `.cursorrules`, `.windsurfrules`, `AGENTS.md`, `CLAUDE.md`, soul files, MCP configs |
| 4 | Credentials (warn-only) | API-Keys, Token – flagged, nie exponiert |
| 5 | Docker Images | AI-bezogene Container |
| 6 | Stragglers | Verwaiste Modell-Dateien, tote PATH-Einträge |

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Sprache | Go |
| TUI | Bubble Tea |
| Binär | Single binary, zero runtime dependencies |
| Netzwerk | **Keins** (by design) |

## Warum dieses Projekt

Ohm ist die Antwort auf eine Frage, die sich jeder stellt, der ernsthaft mit AI-Tools arbeitet: *Was ist eigentlich alles auf meinem Rechner passiert?* Die Privacy-First-Architektur ist nicht Marketing – sie ist die Konsequenz aus der Tatsache, dass AI-Config-Dateien API-Keys und Geschäftslogik enthalten. Ein Tool, das diese scannt, darf selbst nicht telefonieren.
