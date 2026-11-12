---
title: "agent-rush – Package-Triage-Sandbox"
order: 16
icon: fas fa-flask
tags: [Python, FastAPI, Docker, Security Sandbox, Supply Chain, AI Agents, Vultr, Evidence-based]
summary: "Agent inspiziert nicht vertrauenswürdiges Python-Paket in einem gehärteten Container (Egress-Deny-Proxy, read-only rootfs, non-root, cap-drop ALL) und liefert Risk-Report + Roh-Evidenz. Submission für den Vultr Agent Rush Hackathon (LabLab), Track 1 „Blast Radius Zero“."
category: hackathons
---

> **Wettbewerb:** Vultr Agent Rush (LabLab, 03.–08.11.2026) · Track 1: *Blast Radius Zero*
> **Status:** **eingereicht (08.11.2026)** — Ergebnis steht noch aus, wird hier nachgetragen.
**Deutsch**

> **Quellcode:** [github.com/derKosi/agent-rush](https://github.com/derKosi/agent-rush) (public, MIT)

<div class="video-embed"><iframe src="https://www.youtube-nocookie.com/embed/U8ssu2KqMwo" title="Quarantine: proof before the install (2 min)" loading="lazy" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></div>
<p class="video-caption"><i aria-hidden="true" class="fas fa-play"></i> „Quarantine: proof before the install“ (2 Min., YouTube) — warum das Paket erst in die Sandbox kommt, bevor es irgendwas darf.</p>

## In einfachen Worten

Wenn ein KI-Agent fremden Code installiert, ist die Frage nicht „was sagt das README?" — sondern „was macht das Paket wirklich?". agent-rush lässt einen Agenten ein Python-Paket untersuchen, aber nicht auf dem Rechner, sondern in einem Container, der nichts nach außen lassen kann und nichts verändern kann. Am Ende gibt es zwei Dinge: einen kurzen Risk-Report — und die rohen Beweise dazu. Was der Agent behauptet, ist immer gegenlaufbar gegen Log.

## Das Prinzip: Evidenz statt Behauptung

Der Kernsatz des Projekts: *„An agent that only chats is a demo; an agent that executes safely is a product."* Deshalb produziert jeder Lauf drei Evidenz-Artefakte neben dem Verdict:

1. **Deny-Log des Egress-Proxys** — jeder Verbindungsversuch des Packages nach draußen, deny-by-default, als JSONL
2. **Prozess-Result** — was im Container tatsächlich ausgeführt wurde
3. **Filesystem-Diff** — was das Paket angefasst oder angelegt hat

Die Sandbox ist dabei **Policy als ausführbare Flags**: `--network none` / Egress-Proxy, read-only rootfs, non-root, `cap-drop ALL`, pids/mem/cpu-Limits — keine interpretierte Policy-Datei, sondern Container-Flags, die man prüfen kann.

## Wie es gebaut ist

- **Python/FastAPI**: Orchestrator-Pipeline (Probe-Planung → Sandbox-Run → Verdict), Run-Store, regelbasierter Verdict-Generator
- **Docker-Härtung** als eigene `sandbox/`-Einheit (Dockerfile + `run.sh`)
- **Deny-Proxy** als eigenes Modul — jeder Egress-Versuch wird geloggt, nichts durchgelassen
- **Modelle** über Vultr Serverless Inference (OpenAI-kompatibel) für die Probe-Planung
- **Tests**: 109 Tests in 3 Suiten, End-to-End grün — kein CI, bewusst (README): Verifikation soll durch echte Container-Runs passieren, nicht durch grüne Haken

## Ehrliche Grenzen (selbst dokumentiert)

Zwei Schwächen stehen offen in der Doku, weil das das Projektprinzip ist: ein detached Child-Prozess wird aktuell **gespawnt, nicht blockiert** (der Pid-Namespace begrenzt ihn, aber stoppt ihn nicht), und der `probe.json`-Selbstreport des Packages ist prinzipiell spoofbar — die Evidenz-Artefakte sind es nicht.

## Stand

Abgabe am 08.11.2026 (End-to-End-Run lokal verifiziert: Egress 2×403 geloggt, read-only rootfs hält, Decoy-Key nicht lesbar, Verdict `high`, 18 s End-to-End). **Das Wettbewerbsergebnis wird hier und im Begleit-Post nachgetragen**, sobald judging abgeschlossen ist.

---

*→ [Alle Wettbewerbe](/wettbewerbe/) · [Zur Projekt-Übersicht](/projects/)*
