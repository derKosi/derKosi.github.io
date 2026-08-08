---
title: KosiComply – Secure-AI-Plattform
order: 1
icon: fas fa-shield-halved
tags: [Secure AI, LLM-Gateway, Compliance, Python, TypeScript, FastAPI, Presidio, LangGraph]
summary: "Compliance-first AI-Plattform für rechtssichere KI-Nutzung im DACH-Markt: LLM-Gateway mit PII-Filter, Compliance-Shields, hash-chained Audit-Logging, Multi-Agent-Coding."
---

# KosiComply – Secure-AI-Plattform

> **Mission:** KI-Nutzung in Unternehmen rechtssicher möglich machen – nach VdS 10000, ISO 27001 und EU AI Act.

## Problem

Unternehmen stehen vor derselben Herausforderung: Sie wollen leistungsstarke KI-Tools (ChatGPT, Claude, Gemini, lokale Modelle) einsetzen, müssen aber gleichzeitig Audit-Pflichten, Datenschutz und Compliance einhalten. Die meisten AI-Lösungen bleiben Demo, weil sie in regulierten Umgebungen nicht rechtssicher betrieben werden können.

## Lösung

KosiComply ist eine Plattform-Familie für Compliance-first AI-Nutzung im DACH-Markt. Sie fungiert als intelligente Middleware zwischen Entwicklern bzw. Anwendern und den KI-Services.

### Kernfunktionen

- **LLM-Gateway (OpenAI-kompatibel):** Drop-in-Proxy, der API-Aufrufe abfängt, PII erkennt und tokenisiert, an das LLM weiterleitet, und die Tokenisierung in der Antwort wiederherstellt
- **PII-Detection:** Microsoft Presidio + spaCy (Deutsch/Englisch), 32+ Regex-Patterns für API-Keys, Session-scoped Substitution
- **Compliance-Shields:** API-Interception für OpenAI und Anthropic, Filterung sensibler Inhalte vor dem Verlassen des Unternehmens
- **Revisionssichere Protokollierung:** Hash-chained Audit-Log (SHA-256), WORM-fähig, für alle KI-Interaktionen
- **Multi-Agent-Coding:** LangGraph-Orchestrierung mit DevAgent, PIIScanner und TestAgent
- **Lokale Spracherkennung:** faster-whisper + VOSK, offline, für Claude Code und andere Agenten

### Bereitstellungsmodelle

1. **VS Code Plugin** – Direkte Integration in den Entwickler-Workflow
2. **Reverse Proxy** – Zentrale Absicherung für das gesamte Unternehmen
3. **Hybrid** – Kombination aus beidem

## Architektur

```
Anwendung/Entwickler
       ↓
[Compliance-Shield / LLM-Gateway]
   ├── PII-Detection (Presidio + spaCy)
   ├── API-Key-Filterung
   ├── Tokenisierung & Restore
   └── Hash-chained Audit-Log
       ↓
[Cloud-LLM (OpenAI/Anthropic) ODER lokale Inferenz (Ollama/vLLM)]
```

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Backend (Gateway) | Python, FastAPI |
| Backend (Shield) | TypeScript, Node.js |
| Frontend | Next.js |
| PII-Detection | Microsoft Presidio + spaCy |
| Multi-Agent | LangGraph |
| Audit | Hash-chained (SHA-256) |
| Tests | 347 Test-Cases |

## Status

Substantielle Implementierung: `complyharnes` (5 npm-Pakete, 347 Tests) und `BetaComply` (Python-Proxy) sind production-grade. ComplyKit (Flagship) teilweise gebaut. complyagent (Multi-Agent-Coding) mit echtem Backend und Tests.

## Warum dieses Projekt

Ich baue KosiComply, weil ich glaube, dass KI in regulierten Umgebungen nicht durch Verbot, sondern durch die richtige Infrastruktur möglich wird. Die Plattform ist der Beweis, dass Compliance und AI-Nutzung kein Widerspruch sind – wenn man es architektonisch richtig angeht.
