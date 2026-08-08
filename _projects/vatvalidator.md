---
title: VatValidator – EU VAT-Validierung
order: 12
icon: fas fa-file-invoice-dollar
tags: [.NET 8, C#, NSwag, OpenAPI, BZSt, VIES, B2B]
summary: ".NET 8 Library für EU-VAT-Nummer-Validierung gegen BZSt und VIES. Auto-generiert aus OpenAPI-Specs. NUnit + Coverlet."
category: ferner-liefen
---

# VatValidator – EU VAT-Validierung

> **Ferner liefen** – kleines, aber polished API-Engineering-Beispiel.

## Was es ist

Eine .NET 8 Library zur Validierung europäischer USt-IdNr. gegen die offiziellen **BZSt**- (Bundeszentralamt für Steuern) und **VIES**- (VAT Information Exchange System) APIs.

### BZSt eVATR API
- Für deutsche Unternehmen zur Bestätigung ausländischer EU-VAT-IDs
- Simple (nur Gültigkeit) und qualified (Gültigkeit + Firmendaten-Verifikation)
- Status-Code-Beschreibungen, EU-Mitgliedsstaaten-Liste

### VIES REST API
- Europäische Kommission – VAT-Nummer-Check über alle EU-Mitgliedsstaaten
- Simple und extended Validation (Trader-Name, Adresse, Company-Type-Matching)
- Status-Endpoint für Echtzeit-Verfügbarkeit pro Land

## API-Engineering-Ansatz

Beide Clients sind **auto-generiert aus OpenAPI-Spezifikationen** via [NSwag](https://github.com/RicoSuter/NSwag). Das stellt sicher, dass sie synchron mit den Upstream-API-Verträgen bleiben – manuelle Client-Pflege entfällt.

## Tech-Stack

| Komponente | Technologie |
|------------|-------------|
| Sprache | C# (.NET 8) |
| API-Generierung | NSwag v14.4 |
| Serialisierung | Newtonsoft.Json v13.0.3 |
| Tests | NUnit 4.3, Shouldly 4.3 |
| Coverage | Coverlet 6.0 |

## Warum hier

Ein kleines, fokussiertes Beispiel für sauberes API-Engineering: OpenAPI-first, auto-generierte Clients, voll getestet. Zeigt, dass ich auch für kleine Utilities auf Code-Generierung und Tests setze – nicht nur für Großprojekte.
