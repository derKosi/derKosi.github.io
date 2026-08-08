---
title: Kanzlei360 – Multi-System-Sync
order: 10
icon: fas fa-arrows-rotate
tags: [.NET 8, Clean Architecture, CQRS, DATEV, SuperOffice, HubSpot, ArchUnitNET, EF Core]
summary: "Multi-System-Daten-Synchronisation zwischen DATEV, SuperOffice und HubSpot. Clean Architecture mit ArchUnitNET-enforced Dependencies, CQRS, Dead-Letter-Handling. 287 Tests."
category: ferner-liefen
---

# Kanzlei360 – Multi-System-Sync

> **Ferner liefen** – detailliertes Engineering-Beispiel, nicht primäres Produkt.

## Was es ist

Multi-System-Daten-Synchronisationsplattform für Kanzleien, die **DATEV** (Steuer/Finanzbuchhaltung), **SuperOffice** (CRM) und **HubSpot** (Marketing-CRM) verbindet. Synchronisiert Mandantstammdaten, Kontakte und Geschäftsentitäten über diese Systeme hinweg – mit Multi-Tenancy, konfigurierbaren Sync-Verbindungen, Änderungserkennung, Konfliktlösung und Dead-Letter-Fehlerbehandlung.

## Architektur-Highlights

- **Clean Architecture** mit strikten Dependency-Regeln, erzwungen durch **ArchUnitNET**-Tests (23 Architektur-Tests)
- **CQRS** (Commands, Queries, Handlers, DTOs) in der Application-Schicht
- **10-Step-Sync-Cycle** mit 3 Ausführungsmodi (streaming/batched/snapshot), Multi-Source-Merge, Batch-Fallback
- **Feld-Level-Konfliktlösung** mit 5 Strategien (Entity Linking, Field-Mapping)
- **Dead-Letter-Queue** mit exponential-backoff-retry (Background-Services, konfigurierbare Zeitpläne)
- **Pipeline-Editor** mit LiteGraph-Canvas, Graph-JSON-Save/Load, Live-Preview
- **Verschlüsselte Provider-Credentials** (AES-GCM), Multi-Tenant-Auth
- **Audit Trail + Observability**: IAuditLogger in SyncEngine, Prometheus `/metrics`

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Runtime | .NET 8 (C# 12, nullable reference types) |
| Web | ASP.NET Core Minimal APIs, OpenAPI |
| Daten | EF Core 8, SQLite (→ PostgreSQL) |
| Architektur | Clean Architecture, CQRS, OneOf (functional error propagation) |
| Mapping | Riok.Mapperly |
| Tests | xUnit, FluentAssertions, Moq, ArchUnitNET |
| Code-Qualität | StyleCop, SecurityCodeScan, EditorConfig |

## Status

16 Meilensteine ✅ done, 3 blocked (wartet auf CloudStandins-Specs, Auth-Modell-Entscheidung, PostgreSQL-Migration).
**Test-Ergebnisse (Stand 2026-07-29):** 287 Tests grün (136 Domain, 81 Infrastructure, 35 Application, 23 Architecture, 12 Web), 0 Warnings, 0 Errors.

## Warum hier

Ein Beispiel für serious Enterprise-Integration: nicht „Quick & Dirty", sondern mit durchgetesteter Clean Architecture, CQRS und echtem Konflikt-Handling. Die 287 Tests (davon 23 reine Architektur-Tests) sind der Beweis, dass hier Engineering-Qualität über Speed gestellt wurde.
