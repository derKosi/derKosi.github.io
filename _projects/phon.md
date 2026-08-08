---
title: Phon – Offline-Spracherkennung
order: 11
icon: fas fa-microphone
tags: [Python, faster-whisper, VOSK, Offline, Privacy, Speech-to-Text]
summary: "Offline-Diktierwerkzeug: Hotkey drücken, sprechen, Text landet in beliebiger Sprache direkt im Clipboard. 100 % offline, kein Cloud, keine Telemetrie."
category: ferner-liefen
---

# Phon – Offline-Spracherkennung

> **Ferner liefen** – nützliches Werkzeug aus dem Local-AI-Umfeld.
> *The unit of perceived loudness. Adjusted for how humans actually hear.*

## Was es ist

Offline-Diktierwerkzeug, das einfach funktioniert: Hotkey drücken (F2), sprechen in beliebiger Sprache, Text in beliebiger Sprache bekommen – eingefügt direkt in das, woran du gerade arbeitest. 60 Sekunden von Installation bis erstem Wort.

## Problem

Man spricht mit AI-Tools. Man diktiert E-Mails. Man denkt laut und will es festhalten. Aber jedes Diktier-Tool will dein Cloud-Konto, deine Mikrofon-Berechtigung auf einem Server, oder ein 20 €/Monat-Abo. Dabei läuft Whisper problemlos auf dem Laptop. Die Modelle sind gratis. Die Hardware ist da. Niemand hat es *einfach* gemacht.

## Lösung

Phon macht genau das. Smarte Hardware-Erkennung findet die GPU, lädt das passende Modell, konfiguriert das Mikrofon. Alles automatisch.

### Privacy-First-Architecture

Sprachdaten sind einzigartig sensibel – Gespräche mit Kunden, diktierte Texte mit Geschäftlogik, Audio mit emotionalen/gesundheitlichen Infos. Phon behandelt all das als vertraulich:

- **Keine Cloud** – Modelle laufen lokal via faster-whisper oder VOSK
- **Keine Telemetrie** – keine Analytics, kein Crash-Reporting, keine Update-Checks
- **Audio nur lokal** – `temp/`-Ordner mit konfigurierbarem Auto-Cleanup
- **Text nur ins Clipboard** – Phon loggt deinen Text nie

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Sprache | Python |
| STT | faster-whisper, VOSK |
| Setup | `setup.bat` (auto hardware detection) |
| Distribution | Dual-License (MIT + kommerziell) |

## Status

Produktionsreife Struktur: `setup.bat`, `dist/`-Package, Tests, Docs, THIRD-PARTY-NOTICES, COMMERCIAL-LICENSE. Funktioniert auf Legacy-Hardware (GTX 860M, 4 GB VRAM).

## Warum hier

Phon ist der Beweis, dass Offline-AI nicht nur möglich, sondern *besser* für sensible Workflows ist. Die Privacy-First-Architektur ist kein Marketing, sondern Konsequenz aus dem Umstand, dass Sprachdaten PII, Geschäftlogik und persönliche Muster enthalten.
