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

## Problem

Vereine leben von Ehrenamtlichen – und ertrinken in Verwaltungsarbeit: Mitgliedsbeiträge nachfassen, Turnier­anmeldungen fristgerecht einreichen, Raumreservierungen bestätigen. Niemand wird dafür bezahlt, alle haben einen Tag-Job. Klassische Automatisierung hilft nicht, weil jedes dieser Dinge *im Einzelfall* menschliches Urteil verlangt.

## Lösung

**ClubSteward** arbeitet den Vereins-Inbox über Nacht ab – und weckt nur für Entscheidungen, die einen Menschen verdienen:

- **Strands Agents SDK** (AWS) als Agent-Framework
- **Policy-as-Data Human-in-the-Loop:** Eingriffsregeln liegen als Daten vor, nicht im Code – der Agent darf nur dann autonom handeln, wenn die Policy es erlaubt
- **GLM (Z.ai) via LiteLLM** als lokales, austauschbares Modell-Backend
- Vollständig lokale Ausführung – Vereinsdaten verlassen die Maschine nicht

## Warum dieses Projekt

Agentic AI wird oft als Autonomie-Fantasie verkauft. Interessanter ist die Gegenfrage: *Wo genau darf ein Agent autonom sein – und wo muss er anhalten?* ClubSteward operationalisiert das: Der overnight-Autonomie-Gewinn wird durch explizite, datenbasierte Entscheidungs-Grenzen bezahlt. Genau die Disziplin, die auch unternehmensekritische Agenten brauchen.

## Status

Submitted zum Hackathon. Der Agent läuft demonstrierbar lokal.
