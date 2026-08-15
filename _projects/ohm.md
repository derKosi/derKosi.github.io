---
title: Ohm – Privacy-First AI-Scanner
order: 3
icon: fas fa-bolt
tags: [Go, Bubble Tea, TUI, Privacy, Offline, AI-Hygiene, Compliance, Security]
summary: "Cross-platform TUI-Scanner für AI-Software: 90+ Signaturen, Compliance-Mode mit CI-Gates und Baselines, reversibler Backup-Mode, Credential-Audit – 100 % offline, kein HTTP-Client im Codebase."
---

# Ohm – Privacy-First AI-Scanner

> **Tagline:** *Resistance against AGI bloat.*
> Ohm misst, was da ist – und hilft dir, zu entfernen, was du nicht brauchst.

## Problem

Du testest AI-Tools. Du installierst Agents, Harnesses, Runtimes, lädst Modelle herunter, richtest SDKs ein. Monate später ist deine Platte voll mit 14 GB Modell-Dateien von diesem einen Experiment, drei AI-Editoren, die du vergessen hast, und einem `.cache/huggingface`-Verzeichnis, das leise auf 80 GB angewachsen ist.

Schlimmer noch: AI-Config-Dateien enthalten oft API-Keys, Projektpfade und persönliche Instruktionen. Diese Daten sind sensibel – und niemand weiß, wo sie alle liegen.

## Lösung

Ohm ist ein cross-platform (Linux/macOS/Windows) TUI-Tool, das dein System nach AI-bezogener Software durchsucht, dir zeigt, was installiert ist, wie viel Platz es frisst, und ein Deinstallations-Skript generiert, das du vor der Ausgabe prüfen kannst. **Ohm löscht selbst nie etwas.**

```bash
ohm scan                 # Interaktive TUI — auswählen, was weg soll
ohm generate             # Schreibt ohm-cleanup-<date>.sh — prüfen, dann ausführen
ohm generate --backup    # Reversibel: verschiebt nach ~/.ohm/trash/ + Restore-Skript
```

### Privacy-First-Architektur

Das ist eine bewusste Designentscheidung und ein zentraler Differenzierer:

- **Kein HTTP-Client im Codebase** – keine Netzwerkfähigkeit, keine Telemetrie, kein Phone-Home
- **100 % offline** – Scan-Ergebnisse werden nie übertragen
- **Lokaler State** – `~/.ohm/state.json`, bleibt auf deinem Rechner

> Wenn du Netzwerk-Code in Ohm findest, ist das ein Bug.

## v0.2: Compliance-Tooling (neu)

Aus dem persönlichen Aufräum-Tool wurde ein auditierbares Instrument:

| Feature | Was es tut |
|---------|-----------|
| **`--fail-on <risk>`** | Exit-Codes für CI/Audit-Gates – bricht Pipelines ab bei kritischen Findings |
| **Baselines + Diff** | `--save-baseline` / `--diff` – verfolgt AI-Software-Drift über Zeit |
| **Reports** | `--csv` / `--markdown` – teilbare Berichte für IT/Compliance |
| **`ohm audit`** | Flaggt API-Key-Locations, die für andere lokale Nutzer lesbar sind (nur Berechtigungen, nie Inhalte) |
| **Custom Signatures** | Eigene Tool-Definitionen in `~/.ohm/signatures/*.yaml` – kein Fork nötig |

### Weitere v0.2-Highlights

- **90+ Signaturen** – Agents (Claude Code, Aider, Codex CLI, Cursor Agent, ...), Runtimes (Ollama, LM Studio, vLLM, KTransformers, MLX, ...), ComfyUI + Checkpoints/LoRAs, Model-Caches, SDKs, Docker-Images
- **Browser-AI-Extensions** – erkennt AI-Erweiterungen in Chromium-Browsern (Chrome, Edge, Brave, Vivaldi, Opera, Arc) und Firefox, inklusive Local Storage
- **Windows-Registry** – installer-basierte AI-Apps über Uninstall-Entries (LM Studio, GPT4All, ...)
- **Agent Memory & Sessions** – findet Conversation-History, Session-Daten, akkumulierte Kontexte
- **TUI-Power-Features** – Category-Folding, Suchfilter (`/`), Detail-Ansicht (`d`) mit Uninstall-Commands
- **Ignore-List** – `ohm ignore <id>` blendet behaltenne Tools aus
- **Shell-Completions** – bash/zsh/fish/powershell

## Deployment & Distribution

- **Einzelbinary** (Go), amd64 + arm64, null Runtime-Dependencies
- **Installer:** `install.sh` (Linux/macOS), `install.ps1` (Windows PowerShell)
- **Paketmanager-Manifeste:** Winget, Scoop, Homebrew
- **IT-Fleet-Guide:** Deployment-Anleitung für Rollouts auf Maschinenparks inkl. Compliance-JSON

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Sprache | Go |
| TUI | Bubble Tea |
| Netzwerk | **Keins** (by design) |
| Tests | Unit-Tests für Pure Functions (Model, Generator) |

## Warum dieses Projekt

Ohm ist die Antwort auf eine Frage, die sich jeder stellt, der ernsthaft mit AI-Tools arbeitet: *Was ist eigentlich alles auf meinem Rechner passiert?* Mit v0.2 wird daraus auch eine Antwort für IT-Abteilungen: AI-Software-Bestand erfassen, drift tracken, in CI-Gates erzwingen. Die Privacy-First-Architektur bleibt dabei der Kern – ein Tool, das API-Keys scannt, darf selbst nicht telefonieren.
