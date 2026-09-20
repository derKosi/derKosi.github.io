---
icon: fas fa-layer-group
order: 1
---

# Projekte

Ausgewählte Arbeiten aus dem Bereich Secure AI, Cloud Architecture und B2B-Integration.
Wo der Quellcode öffentlich ist, ist er verlinkt – private Arbeiten gerne bei Interesse nachfragen.

## Secure AI & Compliance

- [**KosiComply**](/projects/kosicomply/) – Compliance-first AI-Plattform für rechtssichere KI-Nutzung im DACH-Markt. LLM-Gateway mit PII-Filter, Compliance-Shields, hash-chained Audit-Logging, Multi-Agent-Coding.
- [**Heimdall**](/projects/heimdall/) – OAuth-2.0-Token-Mediationsplattform für Partner-App-Stores. 17 Live-Adapter, AES-256-GCM, custody-Modelle für verschwiegenheitspflichtige Branchen.

## AI Tooling & Privacy

- [**Ohm**](/projects/ohm/) – Privacy-First-Scanner für AI-Software ([GitHub](https://github.com/derKosi/Ohm), AGPL-3.0). Erkennt Agents, Modelle, Config-Dateien, Credentials. 100 % offline, kein HTTP-Client im Codebase. 2026 extern auditiert (OpenVuln/z.ai) – alle Findings gefixt in v0.2.0.
- [**UnixDirStat**](/projects/unixdirstat/) – Disk-Usage-Analyzer mit Treemap fürs Terminal ([GitHub](https://github.com/derKosi/UnixDirStat), MIT). WinDirStat-Feeling in Go + Bubble Tea; ebenfalls 2026 auditiert und gehärtet.
- [**ComfyUI-Rig**](/projects/comfyui-rig/) – Lokale AI-Generierung via MCP-Server. 31 Tools für Workflow-Erstellung, Model-Management, VRAM-Kontrolle. Angebunden an AI-Agenten.
- [**Phon**](/projects/phon/) – Offline-Spracherkennung. Hotkey → sprechen → Text im Clipboard. 100 % offline, faster-whisper/VOSK, Legacy-Hardware-tauglich.

## Hackathons & Wettbewerbe

- [**ClubSteward**](/projects/clubsteward/) – Overnight Club-Secretary Agent ([GitHub](https://github.com/derKosi/clubsteward)). Strands Agents SDK, Policy-as-Data-Human-in-the-Loop (decision cards mit Warum-du?-Begruendung), vollstaendig lokal. Agents-for-Humans-Hackathon (AWS x Devpost).
- [**DaT Parkinson's**](/projects/datparkinsons/) – DaT-SPECT-Klassifikation (DrivenData 2026). GBM+SBR-Features + MIP-CNN gegen Site-Shift; LCO-validiert. Finaler LB-Score 0.3963 (Log Loss), AUROC 0.9004.

## B2B Integration & Tooling

- [**CloudStandins**](/projects/cloudstandins/) – „LocalStack for SaaS APIs": Mock-Server für 11 B2B-Provider ohne Testzugang. 213 Tests. .NET 10.
- [**Kanzlei360**](/projects/kanzlei360/) – Multi-System-Sync (DATEV, SuperOffice, HubSpot). Clean Architecture mit ArchUnitNET, CQRS, Dead-Letter-Handling. 287 Tests. .NET 8.
- [**VatValidator**](/projects/vatvalidator/) – EU-VAT-Validierung gegen BZSt und VIES. Auto-generiert aus OpenAPI-Specs (NSwag). .NET 8.

## Weitere Arbeiten

- [**TDMobileConverter**](/projects/tdmobileconverter/) – Legacy-Migration mit LLM: Gupta/OpenText TD Mobile SAL → Blazor Server. Hybride Pipeline (deterministischer Parser + LLM), mit on-prem Ollama-Option.
- [**BauGenie**](/projects/baugenie/) – Multilinguale Construction-SaaS (26 Sprachen, DIN-Norm-Checks, DXF-Export, 3D-Visualisierung). Next.js, Supabase, Vercel CI/CD.

---

*Weitere Arbeiten im Bereich Agent-CLI-Entwicklung, Platform Operations (Musketier, Proxmox/Ansible) und Legacy-Migration auf Anfrage.*
