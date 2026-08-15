---
title: Heimdall – OAuth-2.0-Token-Mediation
order: 2
icon: fas fa-key
tags: [.NET 10, OAuth 2.0, Token-Broker, Security, AES-256-GCM, Key Vault, WORM, Compliance, Bicep, SDK]
summary: "OAuth-2.0-Token-Mediationsplattform für Partner-App-Stores: 17 Live-Adapter, Azure-Production-Backends mit Key-Vault-Envelope-Encryption und Blob-WORM-Compliance, SDK-Plattform mit generierten Provider-Clients."
---

# Heimdall – OAuth-2.0-Token-Mediation

> **Mission:** Partner-Client-Apps sicher an Anbieter wie DATEV, SuperOffice, HubSpot anbinden – ohne dass das `client_secret` die Kontrolle verliert.

## Problem

Partner-Apps, die Endkunden an Provider (DATEV, SuperOffice, HubSpot, etc.) anbinden, stehen vor einem Dilemma: Sie brauchen das `client_secret` für den OAuth-Dance, dürfen es aber nicht an Partner-Clients weitergeben. Zusätzlich gelten in verschwiegenheitspflichtigen Branchen (Anwälte, Steuerberater, Ärzte) besondere Anforderungen an Token-Custody und Auditierbarkeit.

## Lösung

Heimdall ist eine Backend-Plattform, die den OAuth-Dance vermittelt: Partner-Clients verbinden ihre Endkunden über Heimdall, der die Tokens brokt und das `client_secret` unter Verschlüsselung hält. Partner-Clients sehen das Secret nie.

### Kernfunktionen

- **17 Live-Adapter** über vier Provider-Familien: OIDC (12 Provider, u. a. DATEV TX, SuperOffice, HubSpot, Sage, Business Central, Exact Online, Stripe), templated-endpoint (Shopify), API-key (sevDesk, FastBill, Billomat, Holded), plus In-Memory-Mock und Keycloak
- **Custody-Modelle:** Per-Provider wählbar – `Dual` (HubSpot/SuperOffice) vs. `Sole` (DATEV, rotierende Refresh-Tokens)
- **Kill-Switch:** Missed Heartbeat → Provider-Revoke
- **Azure-Production-Backends (M5):** Key-Vault-Envelope-Encryption für Secrets at Rest, Blob-Storage mit WORM (Write-Once-Read-Many) für das Compliance-Log – die Schnittstellen waren schon da, jetzt sind die echten Backends gelandet (ADR 0027)
- **BFF-Auth-Modell:** API-Key → 15-Min-JWT + Inline-Discovery (ADR 0017)
- **Certification Gate:** Provider können Partner-Voraussetzungen fordern (z. B. DATEV = Schnittstellenanbieter)

## SDK-Plattform (neu in M5)

Der zweite große M5-Strang: eine **Provider-SDK-Plattform**, die Client-Bibliotheken generiert statt sie von Hand zu pflegen:

- **Multi-Target-Clients:** .NET 10 + netstandard2.0 + .NET Framework 4.8 – Letzteres für Legacy-Partner-Apps, die noch nicht modernisiert sind (eigener Bridge-Ansatz via openapi-generator)
- **Generierte Provider:** Stripe als zweiter generierter Provider beweist, dass die Generierung auf N Provider skaliert – ohne Bridge-Code
- **DATEV-Tracer-Bullets:** Vier DATEV-APIs durchgängig implementiert – Foundations/Stammdaten, Belege-Online-Upload (typisiertes Multipart), TRAFFIQX-Rechnungen (erster upstream-gepinnter Provider), EXTf-Batch-Import (DATEV-Format, typed Body)
- **Publish-Pipeline:** CI-Pack-Verifikation + privater BaGet-Feed (NuGet)

## Architektur-Entscheidungen

27 Architecture Decision Records (ADR 0001–0027), u. a.:

| ADR | Entscheidung |
|-----|-------------|
| 0023 | Provider-SDK-Plattform (Generierung statt Handpflege) |
| 0024 | Explizite Secrets statt Entity-Felder (keine Side-Channels) |
| 0025 | Einheitlicher API-Error-Envelope, resolve-or-throw |
| 0026 | Connection-Strategy-Seam für Provider-Family-Dispatch |
| 0027 | Azure-Production-Backends (Key Vault + Blob WORM) |

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Runtime | .NET 10 (C# 14) |
| Web | ASP.NET Core, Razor Pages, OpenAPI/Scalar |
| Daten | EF Core, SQLite (→ Azure SQL) |
| Auth | Cookie + Keycloak-federiert, bespoke JWT issuer |
| Verschlüsselung | AES-256-GCM + Key-Vault-Envelope (M5) |
| Compliance | Hash-chained Log → Blob WORM (M5) |
| SDK | Multi-target, openapi-generator, BaGet |
| Tests | 255/255 deterministisch |

## Deployment-Pfade

| Pfad | Wann |
|------|------|
| Docker Compose | Lokal, Dev |
| Proxmox LXC (Ansible) | Self-hosted |
| Azure (Bicep) | Production: App Service + Azure SQL + Key Vault + Storage |

## Status

**M5 code-complete** (ADR 0027). Runnable end-to-end mit 17 realen Adaptern, 255 Tests deterministisch grün, SDK-Plattform mit Publish-Pipeline. Drei DACH-LegalTech-Demo-Partner geseedet.

## Warum dieses Projekt

Heimdall zeigt, wie man Sicherheit und Integration richtig zusammendenkt: Token-Custody als architektonisches Problem, nicht als nachträglicher Patch. Die Kombination aus Envelope-Encryption, WORM-fähigem Audit-Log und Provider-Certification ist genau das, was regulierte B2B-Integrationen brauchen.
