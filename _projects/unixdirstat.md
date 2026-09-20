---
title: UnixDirStat – Disk-Usage im Terminal
order: 8
icon: fas fa-chart-simple
tags: [Go, Bubble Tea, TUI, Disk Usage, Treemap, Open Source]
summary: "Terminal-Disk-Usage-Analyzer mit squarified Treemap, File-Type-Breakdown und interaktivem Tree-View – WinDirStat fürs Terminal. Öffentlich, MIT, mit CI und Releases."
category: open-source
---

# UnixDirStat – Disk-Usage im Terminal

> **Quellcode:** [github.com/derKosi/UnixDirStat](https://github.com/derKosi/UnixDirStat) (public, MIT)
> **Download:** [Releases](https://github.com/derKosi/UnixDirStat/releases/latest) – Linux/macOS/Windows

## Was es ist

Ein terminalbasierter Disk-Usage-Analyzer: squarified Treemap, Aufschlüsselung nach Dateitypen, interaktiver Tree-View mit Delete. Inspiriert von WinDirStat, gebaut fürs Terminal. Go + Bubble Tea.

## Features

- **Squarified Treemap** – proportional flächentreue Visualisierung der Plattenbelegung
- **File-Type-Breakdown** – Extension-Statistiken mit Farbcodierung
- **Interaktiver Tree-View** – navigieren, vergleichen, direkt löschen (mit Confirmation)
- **Headless-Mode** (`-scan`) – für Scripts und CI
- **Härtung** – alle untrusted Output-Sinks sanitiziert (Terminal-Injection-Proof), Delete mit TOCTOU-Guard

## Security-Story

Das Projekt wurde 2026 von [OpenVuln](https://openvuln.vulnhunter.pro) (powered by [z.ai](https://huggingface.co/spaces/zai-org/OpenVuln)) auditiert: 10 Findings, u. a. Terminal-Injection über Dateinamen (OSC-52-Payloads) und ein TOCTOU-Fenster im Delete-Pfad. Alle Findings verifiziert, gefixt, regression-getestet und als v0.1.1 released – der [Blogpost](/posts/how-an-ai-audit-found/) beschreibt den Ablauf.
