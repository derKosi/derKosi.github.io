---
title: ComfyUI-Rig – Lokale AI-Generierung via MCP
order: 5
icon: fas fa-microchip
tags: [ComfyUI, MCP, Python, Local AI, RTX 3090, Stable Diffusion, Agents]
summary: "Programmatische Steuerung eines lokalen ComfyUI-Generators (RTX 3090) über MCP-Server: 31 Tools für Workflow-Erstellung, Model-Management und VRAM-Kontrolle, angebunden an AI-Agenten."
---

# ComfyUI-Rig – Lokale AI-Generierung via MCP

> **Mission:** AI-Bild- und Video-Generierung lokal betreiben – kontrolliert durch Agenten, ohne Cloud-Abhängigkeit.

## Setup

Ein lokaler ComfyUI-Generator auf einer RTX 3090 (24 GB VRAM), gesteuert über einen **MCP-Server** (Model Context Protocol), der AI-Agenten wie Claude Code programmatischen Zugriff gibt.

- ComfyUI v0.18.5
- 1.132 Node-Types installiert
- Modelle: SDXL, Wan 2.2 14B (Video), ACE Step (Audio)
- Angebunden an: Claude Code, Pi, und eigene Harnesses

## Der MCP-Server

Der `comfyui-mcp`-Server exposes 31 Tools, die Agenten nutzen können:

| Kategorie | Tools (Auswahl) |
|-----------|----------------|
| Workflow | `create_workflow`, `enqueue_workflow`, Workflow-Templates (txt2img, img2img, upscale, inpaint) |
| Modelle | `list_local_models`, Model-Loading |
| System | `get_system_stats`, `clear_vram`, Queue-Management |
| CLI | `/comfy:gen`, `/comfy:batch` Slash-Commands für Agent-CLIs |

## Architektur-Prinzip

> *„I am the LLM."*

Der Agent (Claude Code etc.) übernimmt das Reasoning – welche Art von Workflow, welche Parameter, welche Modelle. ComfyUI führt aus. Der MCP-Server ist die Brücke, die dem Agenten kontrollierten Zugriff gibt, ohne dass dieser die ComfyUI-HTTP-API direkt orchestrieren muss.

```
Agent (Claude Code / Pi / Harness)
       ↓ MCP (31 Tools)
[comfyui-mcp server]
       ↓ HTTP (localhost:8188)
[ComfyUI (RTX 3090)]
       ↓
Bild/Video/Audio-Output
```

## Warum lokal

Cloud-Generatoren (Midjourney, DALL-E, Runway) sind bequem, aber sie haben Nachteile: Kosten pro Generierung, Daten verlassen das Unternehmen, Modelle sind nicht kontrollierbar. Eine lokale RTX 3090-Rig mit ComfyUI gibt volle Kontrolle – über Modelle, Prompt-Engineering, Workflow-Anpassung und vor allem über die Daten.

## Warum dieses Projekt

Diese Rig ist der Beweis, dass lokale AI-Generierung nicht nur möglich, sondern *besser kontrollierbar* ist als Cloud-Alternativen – wenn man die richtige Tooling-Schicht (MCP) dazwischen baut. Der Agent-ans-MCP-Pattern ist übertragbar: Jedes lokale Tool, das eine API hat, lässt sich so für Agenten zugänglich machen.
