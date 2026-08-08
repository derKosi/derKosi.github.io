---
title: Heimdall – OAuth-2.0-Token-Mediation
order: 2
icon: fas fa-key
tags: [.NET 10, OAuth 2.0, Token-Broker, Security, AES-256-GCM, Compliance, Bicep, EF Core]
summary: "OAuth-2.0-Token-Mediationsplattform für Partner-App-Stores: 17 Live-Adapter, AES-256-GCM-Verschlüsselung, custody-Modelle für verschwiegenheitspflichtige Branchen, hash-chained Compliance-Log."
---

# Heimdall – OAuth-2.0-Token-Mediation

> **Mission:** Partner-Client-Apps sicher an Anbieter wie DATEV, SuperOffice, HubSpot anbinden – ohne dass das `client_secret` die Kontrolle verliert.

## Problem

Partner-Apps, die Endkunden an Provider (DATEV, SuperOffice, HubSpot, etc.) anbinden, stehen vor einem Dilemma: Sie brauchen das `client_secret` für den OAuth-Dance, dürfen es aber nicht an Partner-Clients weitergeben. Zusätzlich gelten in verschwiegenheitspflichtigen Branchen (Anwälte, Steuerberater, Ärzte) besondere Anforderungen an Token-Custody und Auditierbarkeit.

## Lösung

Heimdall ist eine Backend-Plattform, die den OAuth-Dance vermittelt: Partner-Clients verbinden ihre Endkunden über Heimdall, der die Tokens brokt und das `client_secret` unter Verschlüsselung hält. Partner-Clients sehen das Secret nie.

### Kernfunktionen

- **17 Live-Adapter** über vier Provider-Familien: OIDC (12 Provider), templated-endpoint (Shopify), API-key (4 Provider), plus In-Memory-Mock und Keycloak
- **Custody-Modelle:** Per-Provider wählbar – `Dual` (HubSpot/SuperOffice) vs. `Sole` (DATEV, rotierende Refresh-Tokens)
- **Kill-Switch:** Missed Heartbeat → Provider-Revoke
- **AES-256-GCM-Verschlüsselung** für Secrets at Rest
- **Hash-chained Compliance-Log** (SHA-256), append-only, WORM-fähig
- **Certification Gate:** Provider können Partner-Voraussetzungen fordern (z. B. DATEV = Schnittstellenanbieter)
- **BFF-Auth-Modell:** API-Key → 15-Min-JWT + Inline-Discovery

## Architektur

```
Partner-Client ──(API-Key)──► Heimdall
                                │
                    ┌───────────┴───────────┐
                    │  Token-Mediation-Service
                    │  ├── AES-256-GCM Vault
                    │  ├── Custody Manager (Dual/Sole)
                    │  ├── Certification Gate
                    │  └── Hash-chained Audit
                    │
                    ▼
              Provider (DATEV, SuperOffice, HubSpot, Sage, ...)
```

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Runtime | .NET 10 (C# 14) |
| Web | ASP.NET Core, Razor Pages, OpenAPI/Scalar |
| Daten | EF Core, SQLite (→ Azure SQL) |
| Auth | Cookie + Keycloak-federiert, bespoke JWT issuer |
| Verschlüsselung | AES-256-GCM, Key Vault-ready |
| Tests | 255/255 deterministisch |

## Deployment-Pfade

| Pfad | Wann |
|------|------|
| Docker Compose | Lokal, Dev |
| Proxmox LXC (Ansible) | Self-hosted |
| Azure (Bicep) | Scale-ready: App Service + Azure SQL + Key Vault |

## Status

M1 + M3 + M4 komplett – runnable end-to-end mit 17 realen Adaptern. 255 Tests deterministisch grün. Drei DACH-LegalTech-Demo-Partner geseedet.

## Warum dieses Projekt

Heimdall zeigt, wie man Sicherheit und Integration richtig zusammendenkt: Token-Custody als architektonisches Problem, nicht als nachträglicher Patch. Die Kombination aus Verschlüsselung, Audit-Log und Provider-Certification ist genau das, was regulierte B2B-Integrationen brauchen.
