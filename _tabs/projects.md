---
description: "Projekte: Secure AI & Compliance, Wettbewerbe mit Beweis, Open Source (auditiert) und B2B-Integration — jede Karte mit Beleg."
title: Projekte
icon: fas fa-layer-group
order: 1
---

<div class="page-head">
  <p class="page-lead">Ausgewählte Arbeiten aus <strong>Secure AI</strong>, <strong>Cloud Architecture</strong> und <strong>B2B-Integration</strong> — jede mit Beleg: Code, Demo, Audit oder Wettbewerbsergebnis. Private Arbeiten gerne bei Interesse nachfragen.</p>
</div>

<div class="proj-cat">
  <h2><span class="cat-dot dot-red"></span>Secure AI &amp; Compliance</h2>
  <div class="proj-grid-lg">
    <a class="proj-card" href="{{ '/projects/kosicomply/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-red"><i aria-hidden="true" class="fas fa-shield-halved"></i></span>
        <span class="proj-card-name">KosiComply</span>
      </div>
      <p class="proj-card-desc">Compliance-first AI-Plattform für rechtssichere KI-Nutzung im DACH-Markt: LLM-Gateway mit PII-Filter, Compliance-Shields, hash-chained Audit-Logging.</p>
      <div class="pill-row">
        <span class="pill">LLM-Gateway</span><span class="pill">PII-Filter</span><span class="pill">EU AI Act</span><span class="pill">Audit-Log</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/heimdall/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-red"><i aria-hidden="true" class="fas fa-key"></i></span>
        <span class="proj-card-name">Heimdall</span>
      </div>
      <p class="proj-card-desc">OAuth-2.0-Token-Mediationsplattform für Partner-App-Stores. 17 Live-Adapter, AES-256-GCM, custody-Modelle für verschwiegenheitspflichtige Branchen.</p>
      <div class="pill-row">
        <span class="pill">OAuth 2.0</span><span class="pill">AES-256-GCM</span><span class="pill">17 Adapter</span><span class="pill">Key Vault</span>
      </div>
    </a>
  </div>
</div>

<div class="proj-cat">
  <h2><span class="cat-dot dot-amber"></span>Wettbewerbe</h2>
  <div class="proj-grid-lg">
    <a class="proj-card card-win" href="{{ '/projects/signalflow/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-gold"><i aria-hidden="true" class="fas fa-trophy"></i></span>
        <span class="proj-card-name">SignalFlow</span>
        <span class="badge-flag flag-gold">🏆 Gewinner</span>
      </div>
      <p class="proj-card-desc">Erklärbare Ampelsteuerung mit A/B-Beweis auf identischem Verkehrsstrom: −24,5 % Delay, −68,4 % verschwendete Grünzeit.</p>
      <div class="pill-row">
        <span class="pill">Hackatron München 2026</span><span class="pill">Explainable AI</span><span class="pill">Zero-Dep Python</span><span class="pill">Smart Cities</span>
      </div>
    </a>
    <a class="proj-card card-ok" href="{{ '/posts/whest-arc-top23/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-green"><i aria-hidden="true" class="fas fa-ranking-star"></i></span>
        <span class="proj-card-name">whest — ARC White-Box</span>
        <span class="badge-flag flag-ok">Top 23 %</span>
      </div>
      <p class="proj-card-desc">Cumulant-Propagation unter FLOP-Budget (2⁴¹ Ops): Platz 180/800, beste Submission mit nur 9,8 % des Budgets. Pre-Registration mit harten Adopt-Gates.</p>
      <div class="pill-row">
        <span class="pill">AIcrowd</span><span class="pill">ML-Research</span><span class="pill">FLOP-Ökonomie</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/agent-rush/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-amber"><i aria-hidden="true" class="fas fa-box-open"></i></span>
        <span class="proj-card-name">agent-rush</span>
        <span class="badge-flag flag-live">Abgabe 11/2026</span>
      </div>
      <p class="proj-card-desc">Untrusted Python-Pakete im gehärteten Container proben — Risk-Report <em>plus Roh-Evidenz</em> (Deny-Log, Prozess-Result, FS-Diff). „Evidenz statt Behauptung."</p>
      <div class="pill-row">
        <span class="pill">Vultr Agent Rush</span><span class="pill">Container-Härtung</span><span class="pill">109 Tests</span><span class="pill">MIT · GitHub</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/clubsteward/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-amber"><i aria-hidden="true" class="fas fa-user-shield"></i></span>
        <span class="proj-card-name">ClubSteward</span>
      </div>
      <p class="proj-card-desc">Overnight Club-Secretary-Agent: Policy-as-Data-Human-in-the-Loop — autonom auf die 80 %, sauber gestoppt bei den 20 %. Vollständig lokal, ~1 Cent/Nacht.</p>
      <div class="pill-row">
        <span class="pill">AWS × Devpost</span><span class="pill">Strands SDK</span><span class="pill">Policy-as-Data</span><span class="pill">HITL · GitHub</span>
      </div>
    </a>
  </div>
</div>

<div class="proj-cat">
  <h2><span class="cat-dot dot-green"></span>AI Tooling &amp; Privacy</h2>
  <div class="proj-grid-lg">
    <a class="proj-card" href="{{ '/projects/ohm/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-green"><i aria-hidden="true" class="fas fa-bolt"></i></span>
        <span class="proj-card-name">Ohm</span>
        <span class="badge-flag flag-ok">Extern auditiert</span>
      </div>
      <p class="proj-card-desc">Privacy-First-Scanner für AI-Software: erkennt Agents, Modelle, Config-Dateien, Credentials. 100 % offline, kein HTTP-Client im Codebase.</p>
      <div class="pill-row">
        <span class="pill">AGPL · GitHub</span><span class="pill">Go</span><span class="pill">Audit 2026 ✓</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/unixdirstat/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-green"><i aria-hidden="true" class="fas fa-chart-pie"></i></span>
        <span class="proj-card-name">UnixDirStat</span>
        <span class="badge-flag flag-ok">Auditiert</span>
      </div>
      <p class="proj-card-desc">Disk-Usage-Analyzer mit Treemap fürs Terminal. WinDirStat-Feeling in Go + Bubble Tea, ebenfalls 2026 extern auditiert und gehärtet.</p>
      <div class="pill-row">
        <span class="pill">MIT · GitHub</span><span class="pill">Go</span><span class="pill">Bubble Tea</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/comfyui-rig/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-green"><i aria-hidden="true" class="fas fa-microchip"></i></span>
        <span class="proj-card-name">ComfyUI-Rig</span>
      </div>
      <p class="proj-card-desc">Lokale AI-Generierung via MCP-Server: 31 Tools für Workflow-Erstellung, Model-Management, VRAM-Kontrolle. Angebunden an AI-Agenten.</p>
      <div class="pill-row">
        <span class="pill">MCP</span><span class="pill">31 Tools</span><span class="pill">VRAM</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/phon/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-green"><i aria-hidden="true" class="fas fa-microphone"></i></span>
        <span class="proj-card-name">Phon</span>
      </div>
      <p class="proj-card-desc">Offline-Spracherkennung: Hotkey → sprechen → Text im Clipboard. 100 % offline, faster-whisper/VOSK, Legacy-Hardware-tauglich.</p>
      <div class="pill-row">
        <span class="pill">faster-whisper</span><span class="pill">VOSK</span><span class="pill">Offline</span>
      </div>
    </a>
  </div>
</div>

<div class="proj-cat">
  <h2><span class="cat-dot dot-blue"></span>B2B Integration &amp; Tooling</h2>
  <div class="proj-grid-lg">
    <a class="proj-card" href="{{ '/projects/cloudstandins/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-blue"><i aria-hidden="true" class="fas fa-cloud"></i></span>
        <span class="proj-card-name">CloudStandins</span>
      </div>
      <p class="proj-card-desc">„LocalStack for SaaS APIs": Mock-Server für 11 B2B-Provider ohne Testzugang. 213 Tests, .NET 10.</p>
      <div class="pill-row">
        <span class="pill">.NET 10</span><span class="pill">11 Provider</span><span class="pill">213 Tests</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/kanzlei360/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-blue"><i aria-hidden="true" class="fas fa-diagram-project"></i></span>
        <span class="proj-card-name">Kanzlei360</span>
      </div>
      <p class="proj-card-desc">Multi-System-Sync (DATEV, SuperOffice, HubSpot). Clean Architecture mit ArchUnitNET, CQRS, Dead-Letter-Handling. 287 Tests.</p>
      <div class="pill-row">
        <span class="pill">DATEV</span><span class="pill">SuperOffice</span><span class="pill">CQRS</span><span class="pill">287 Tests</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/vatvalidator/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-blue"><i aria-hidden="true" class="fas fa-percent"></i></span>
        <span class="proj-card-name">VatValidator</span>
      </div>
      <p class="proj-card-desc">EU-VAT-Validierung gegen BZSt und VIES. Auto-generiert aus OpenAPI-Specs (NSwag), .NET 8.</p>
      <div class="pill-row">
        <span class="pill">BZSt</span><span class="pill">VIES</span><span class="pill">NSwag</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/projects/tdmobileconverter/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-blue"><i aria-hidden="true" class="fas fa-right-left"></i></span>
        <span class="proj-card-name">TDMobileConverter</span>
      </div>
      <p class="proj-card-desc">Legacy-Migration mit LLM: Gupta/OpenText TD Mobile SAL → Blazor Server. Hybride Pipeline (deterministischer Parser + LLM).</p>
      <div class="pill-row">
        <span class="pill">Delphi-Ära</span><span class="pill">Blazor</span><span class="pill">Ollama</span>
      </div>
    </a>
  </div>
</div>

<div class="proj-cat">
  <h2><span class="cat-dot dot-gray"></span>Ferner liefen</h2>
  <div class="proj-grid-lg">
    <a class="proj-card" href="{{ '/projects/baugenie/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-gray"><i aria-hidden="true" class="fas fa-helmet-safety"></i></span>
        <span class="proj-card-name">BauGenie</span>
      </div>
      <p class="proj-card-desc">Multilinguale Construction-SaaS (26 Sprachen, DIN-Norm-Checks, DXF-Export). Next.js, Supabase, Vercel CI/CD.</p>
      <div class="pill-row">
        <span class="pill">Next.js</span><span class="pill">Supabase</span><span class="pill">26 Sprachen</span>
      </div>
    </a>
    <a class="proj-card" href="{{ '/posts/struktur-schlaegt-sprache/' | relative_url }}">
      <div class="proj-card-head">
        <span class="proj-ico ico-gray"><i aria-hidden="true" class="fas fa-gauge-high"></i></span>
        <span class="proj-card-name">sprachencheck</span>
        <span class="badge-flag">Fachartikel</span>
      </div>
      <p class="proj-card-desc">micrograd bitwise in 6 Sprachen: „Struktur schlägt Sprache ~15×, Sprache schlägt Struktur ~1×" — inkl. Lean-4-Korrektheitsbeweis ohne sorry.</p>
      <div class="pill-row">
        <span class="pill">Rust</span><span class="pill">C#</span><span class="pill">Lean 4</span><span class="pill">bitwise</span>
      </div>
    </a>
  </div>
</div>

<div class="page-footnote">
  <i aria-hidden="true" class="fas fa-circle-info"></i> Weitere Arbeiten im Bereich Agent-CLI-Entwicklung, Platform Operations (Musketier, Proxmox/Ansible) und Legacy-Migration auf <a href="{{ '/about/' | relative_url }}">Anfrage</a>.
</div>
