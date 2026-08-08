---
title: TDMobileConverter – Legacy-Migration mit LLM
order: 13
icon: fas fa-code-branch
tags: [.NET, Blazor, LLM, Legacy Migration, Gupta, OpenText TD, Roslyn]
summary: "Konvertiert Gupta/OpenText TD Mobile SAL-Quellcode in eine Blazor Server-App. Hybride Pipeline: deterministischer Parser für Struktur, LLM für Businesslogik."
category: ferner-liefen
---

# TDMobileConverter – Legacy-Migration mit LLM

> **Ferner liefen** – zeigt die AI×Legacy-Kreuzung, die im Hauptberuf relevant ist.

## Was es ist

Konvertiert **Gupta / OpenText TD Mobile** SAL-Quellcode (`.apt` / `.apl`) in eine moderne **Blazor Server**-Anwendung. Eine hybride Pipeline: ein deterministischer Parser übernimmt die Struktur, ein LLM übersetzt die Businesslogik.

## Problem

Legacy-Migration ist traditionell langwierig, fehleranfällig und teuer. Reine manuelle Übersetzung von SAL nach C# ist repetitiv und fehlerträchtig. Reine LLM-Übersetzung scheitert an struktureller Konsistenz. Die Lösung ist eine *hybride* Pipeline, die beides kombiniert.

## Lösung

```
SAL-Quellcode (.apt/.apl)
        ↓
[Deterministischer Parser] → Struktur (Window-Layout, Controls, Event-Bindings)
        ↓
[LLM-Übersetzung] → Businesslogik (SQL-Handles, Variablen, Algorithmik)
        ↓
[Roslyn-Validierung] → Compile-Check, Syntax-Korrektur
        ↓
Blazor Server Solution (kompilierbar)
```

### LLM-Provider-Registry

Unterstützt verschiedene Provider für die Logik-Übersetzung:

- **Z.ai (GLM-4.6)** – Default
- **OpenAI / Azure** – Cloud-Option
- **Ollama** – *vollständig on-prem* (`qwen2.5-coder:7b`, localhost:11434)
- Custom Provider via Interface

Die Ollama-Option ist besonders relevant: Legacy-Code enthält oft Geschäftlogik, die das Unternehmen nicht verlassen darf. On-Prem-Inferenz macht die Migration DSGVO-konform möglich.

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Parser | Custom (deterministisch, SAL-AST) |
| Ziel | Blazor Server (.NET) |
| LLM-Anbindung | Provider-Registry (Z.ai, OpenAI, Azure, Ollama) |
| Validierung | Roslyn (Compile-Check) |
| UI | CLI + Web-UI (ASP.NET Core) |
| Tests | xUnit, Fixtures aus realer SAL-Codebasis |

## Status

Funktionierendes Gerüst, getestet an realer SAL-Codebasis. Generiert kompilierbare Blazor-Solutions. Ehrliche Limitationen dokumentiert (nicht alles übersetzt sauber – siehe FAQ).

## Warum hier

Dieses Projekt zeigt die konkrete Kreuzung aus **AI** und **Legacy-Migration**, die im Hauptberuf relevant ist: Gupta/PPJ-Landschaften modernisieren, aber mit LLM-Unterstützung – und zwar *mit on-prem Option*, sodass auch sensibler Legacy-Code das Unternehmen nicht verlassen muss.
