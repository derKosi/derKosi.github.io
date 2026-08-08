---
title: CloudStandins – „LocalStack for SaaS APIs"
order: 4
icon: fas fa-flask-vial
tags: [.NET 10, ASP.NET Core, Mock-Server, B2B-Integration, Testing, SQLite, Multi-Tenant]
summary: "Realistische Mock-Server für 11 B2B-SaaS-Provider, die keinen Testzugang gewähren. 213 Tests, Multi-Tenant. .NET 10, ASP.NET Core."
---

# CloudStandins – „LocalStack for SaaS APIs"

> **Tagline:** *Run one process, get 11 mocked API providers with realistic demo data.*

## Problem

B2B-Integrationen sind einfach zu bauen – aber fast unmöglich zu testen. Die Provider, die man anbinden muss, geben keinen Testzugang:

| Provider | Problem |
|----------|---------|
| DATEV | Erfordert zertifizierten Steuerberater-Partnervertrag |
| SuperOffice | Braucht Tenant + Lizenz, keine Sandbox |
| HubSpot | Sandbox erfordert Enterprise-Account |
| Salesforce | Scratch Orgs sind langsam/instabil |
| SAP | Ohne Kunden/Partner-Zugang unmöglich |

Das Ergebnis: Integrationen werden blind in Produktion geschoben, oder Entwickler bauen ad-hoc-Mocks, die nicht realistisch genug sind, um Edge-Cases zu finden.

## Lösung

CloudStandins ist ein einzelner Prozess, der 11 B2B-API-Provider mit realistischen Demo-Daten, echten Auth-Flows, Fehler-Simulation und Multi-Tenant-Support mockt. Gebaut für .NET 10, mit SQLite als Backend.

### Provider (8 voll + 3 partial)

| # | Provider | APIs | Tests |
|---|----------|------|-------|
| 1 | **DATEV** | Master data, Accounting, Desktop | 26 |
| 2 | **SuperOffice** | CRM, UDEF, Listen | 9 |
| 3 | **HubSpot** | CRM mit `properties`-Envelope | 11 |
| 4 | **Pipedrive** | CRM | 9 |
| 5 | **Stripe** | Payments, Webhooks | 17 |
| 6 | **SevDesk** | Accounting | 11 |
| 7 | **Lexoffice** | Voll-API | 24 |
| 8 | **Salesforce** | REST v60.0, SOQL | 21 |
| 9-11 | Collmex, Billomat, Sage Active | Basic CRUD | — |

**213 Tests total, alle grün.**

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Runtime | .NET 10 |
| Web | ASP.NET Core |
| Daten | SQLite |
| Fake-Daten | Bogus |
| Deployment | `dotnet run` oder Docker |

## Warum dieses Projekt

CloudStandins entstand aus einem echten Schmerz: Ich baue B2B-Integrationen und kann sie nicht testen, weil mir der Zugang fehlt. Statt weiter ad-hoc zu mocken, habe ich eine systematische Lösung gebaut, die alle gängigen DACH/EU-Provider abdeckt. Die 213 Tests sind nicht nur QA – sie sind der Beweis, dass die Mocks sich verhalten wie die echten APIs.

Es ist auch ein Statement über Engineering-Kultur: Integrationstests sind kein Luxus, sie sind die Voraussetzung für trustworthy B2B-Software.
