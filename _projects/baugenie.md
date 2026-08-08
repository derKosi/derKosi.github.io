---
title: BauGenie – Construction-SaaS
order: 14
icon: fas fa-hard-hat
tags: [Next.js, TypeScript, Supabase, Tailwind, CI/CD, Vercel, SaaS, i18n]
summary: "Multilingual Construction-Management-SaaS: Treppen-/Beton-/Materialrechner mit DIN-Norm-Checks, DXF-Export, 3D-Visualisierung. 26 Sprachen, CI/CD, Vercel."
category: ferner-liefen
---

# BauGenie – Construction-SaaS

> **Ferner liefen** – andere Domäne, aber zeigt Full-Stack-Breite und CI/CD-Reife.

## Was es ist

BauGenie ist eine multilinguale Construction-Management-SaaS-Plattform mit einer Suite spezialisierter Rechner und Tools für Bau-Profis. Gebaut mit Next.js 15, TypeScript und Tailwind CSS, mit Supabase für Auth und Backend.

### Rechner & Tools

| Tool | Funktionen |
|------|-----------|
| **TreppenGenie** | Treppendimensionierung (gerade, L-, U-, Wendel-, Wechseltritt), DIN 18065-Checks, Bauvorschriften (DE/AT/CH/EU/US), Material-Schätzung, 3D-Visualisierung, PDF-Report, DXF-Export |
| **Betonrechner** | Volumen, Sack-Mengen, Kostenschätzung für Platten/Fundamente/Stützen, DIN 1045-Referenz |
| **Raumrechner** | Flächen, Umfang, Volumen für verschiedene Raumformen, applikationsspezifische Berechnungen |
| **Farbrechner** | Farbbedarf nach Oberfläche/Farbe/Textur/Schichten, Tür-Fenster-Abzüge |
| **Materialrechner** | Schotter/Sand/Erddeile/Splitt, Bulk-vs-Bag-Kostenvergleich, Verdichtungsfaktor |
| **WetterApp** | Baustellen-Wetter mit Advisorys und gewerkespezifischen Empfehlungen |

### Platform Features

- **26 Sprachen** – alle 24 EU-Amtssprachen + Ukrainisch + Türkisch
- **Mock Mode** – komplette App lokal ohne externe Abhängigkeiten lauffähig
- **Auth** – Login/Registrierung/Passwort-Reset via Supabase
- **Light/Dark Theme** – System-Präferenz + manueller Toggle
- **Responsive** – Mobile-first mit adaptiver Navigation
- **PDF-Reports** – professionelle Berechnungsberichte
- **3D-Visualisierung** – interaktive Treppenmodelle (Three.js)

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Framework | Next.js 15 (App Router) |
| Sprache | TypeScript (strict) |
| Styling | Tailwind CSS 3 |
| Auth/DB | Supabase (PostgreSQL) |
| i18n | next-intl |
| 3D | React Three Fiber / Three.js |
| PDF | jsPDF + autotable |
| Validierung | Zod |
| Tests | Playwright |
| Deployment | Vercel + GitHub Actions CI/CD |

## Status

Live deployed auf Vercel. CI-Grün (Lint, Type-Check, Build, Tests). Produktionssicher.

## Warum hier

BauGenie zeigt die *andere* Seite: nicht Cloud-Architektur oder Secure AI, sondern Full-Stack-SaaS-Engineering mit echtem Produkt-Fokus. 26 Sprachen, DXF-Export, 3D-Visualisierung, durchgehende CI/CD – das ist die Beweis, dass ich auch auf der Produkt- und Frontend-Seite liefere, nicht nur im Backend.
