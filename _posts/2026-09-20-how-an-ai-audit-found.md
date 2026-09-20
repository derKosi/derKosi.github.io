---
layout: post
title: "Wie ein KI-Audit 21 Schwachstellen in meinen Open-Source-Tools fand – und was ich daraus lernte"
date: 2026-09-20
author: derKosi
lang: de
categories: [Security, Open Source]
tags: [Security Audit, OpenVuln, z.ai, Go, Secure Coding, Supply Chain]
---

> *Dieser Beitrag beschreibt einen echten Audit-Durchlauf über zwei meiner öffentlichen Tools – inklusive reproduzierbarer PoCs, Fix-Strategie und Release-Hygiene. Credits gehen an [OpenVuln](https://openvuln.vulnhunter.pro), powered by [z.ai](https://huggingface.co/spaces/zai-org/OpenVuln).*

## Der Setup

[Ohm](https://github.com/derKosi/Ohm) – mein Privacy-First-Scanner für AI-Software – und [UnixDirStat](https://github.com/derKosi/UnixDirStat) – ein Terminal-Disk-Usage-Analyzer – sind kleine, aber echte Open-Source-Tools: Go, CI auf drei Plattformen, Installations-Skripte. Ich reichte beide bei OpenVuln ein, einem automatisierten Security-Audit-Service.

Zurück kamen zwei Reports mit **21 Findings** – und ehrlich gesagt war meine erste Reaktion Skepsis. Automatisierte Audits produzieren gerne Rauschen. Also machte ich das, was ich jedem raten würde: **jedes einzelne Finding verifizieren, bevor man irgendetwas ändert.**

## Findings, die real waren

Die Reports waren besser als erwartet – mit ehrlichen Exploitability-Downgrades und Code-Ankern. Aber „real im Report" ist nicht „real im System". Also baute ich PoCs:

**UnixDirStat: Terminal-Injection über Dateinamen.** Ein Verzeichnis mit einer Datei namens `x.\x1b]52;c;aGVsbG8=\x07` – ein OSC-52-Clipboard-Hijacking-Payload als Dateiname. Der ungepatchte Analyzer gab diese Bytes byte-identisch auf stdout aus. Eine Datei mit eingebettetem Raw-Linefeed erzeugte gefälschte Ausgabe-Zeilen – inklusive `Errors: 0 (verified by CI)`. Wer Terminal-Output parst, liest damit Attacker-Text.

**Ohm: eine komplette Injection-Kette.** Ohm generiert Deinstallations-Skripte aus Scan-Ergebnissen. Der PoC: ein manipuliertes State-File mit einem Finding-Namen, der einen Zeilenumbruch enthält, brach aus dem `# `-Kommentar aus – und landete als **live ausführbare Zeile** im generierten Skript. Dazu: unquoted `rm -rf`-Pfade, ein Symlink-freundlicher Skript-Write (CWE-59/377), und ein State-File, dem `generate` blind vertraute.

## Der Fix-Satz

Interessanter als die Einzelfindings war, was sie über *Design* sagen:

1. **Alle untrusted Sinks sanitizen.** UnixDirStat behandelte Dateinamen als vertrauenswürdige Daten. Dabei ist ein Dateiname ein Angriffsvektor wie jeder andere Input – ESC, BEL, DEL, C1-Bytes, Raw-LF: alles wird neutralisiert, bevor es einen Terminal erreicht.
2. **TOCTOU-Guard vor jedem destruktiven Pfad.** `os.RemoveAll` auf einem gecachten Pfad ist ein Zeitfenster. Ein Lstat + Typ-Check + Symlink-Containment-Check direkt vor dem Delete schließt es.
3. **Generated Scripts sind Code, nicht Daten.** Also: Argument-Quoting (`shellQuote`/`psLiteral`), Single-Line-Asserts gegen Kommentar-Breakouts, `O_EXCL`-Writes gegen gepflanzte Symlinks.
4. **State-Files sind untrusted.** `ohm generate` vertraut dem persistierten JSON nicht mehr, sondern re-scanned und übernimmt nur die Selektion – der State kann von jedem Tool im User-Kontext geschrieben werden.

Jeder Fix bekam einen Regressionstest, jeder Fix wurde gegen den ursprünglichen PoC verifiziert – nicht gegen meine Annahme, sondern gegen den exploitablen Zustand.

## Release-Hygiene

Fixes auf `main` sind die halbe Miete. Der `install.sh`-Installer zieht `latest`:

- **Ohm v0.2.0** – mit vollständigem AGPL-3.0-Text (die vorherige Notice-only-LICENSE verletzte selbst die AGPL-Verbreitungsbedingungen – ein Finding eigener Art)
- **UnixDirStat v0.1.1** – MIT, Lizenz jetzt auch im Release-Tarball
- Alte, verwundbare Releases und Tags entfernt – mit dem bewussten Trade-off, dass der Go-Module-Proxy alte Versionen weiter cached; bei null bekannter Nutzer war saubere Homepage wichtiger als historische Anker

Nebenbei fand der Audit-Durchlauf noch einen 13 Jahre alten Parse-Bug in einem Legacy-Projekt ([Corefig](https://github.com/derKosi/Corefig)): zwei fehlende Leerzeichen in einem PowerShell-`switch` ließen das Lizenz-Modul seit 2013 crashen. Ein CI-Parse-Gate – 20 Zeilen YAML – hätte das 2013 abgefangen. Wer Legacy hält, sollte mindestens syntaktisch gaten können.

## Was ich daraus mitnehme

- **KI-Audits sind ein Hebel, kein Urteil.** Der Report lieferte Struktur und Anker; das Verifizieren, Priorisieren und Fixen blieb Handarbeit. Aber 21 Findings in zwei kleinen Tools, die ich für sauber hielt – das ändert die Basis-Annahme.
- **PoC-first.** Kein Fix ohne Reproduktion, kein „done" ohne PoC-ReRun. Das hat mich vor mindestens zwei Pseudo-Fixes bewahrt.
- **Supply Chain gilt für die eigene Delivery.** Wer `curl | sh`-Installer ausliefert, hat eine besondere Verantwortung für `latest`.

Die Reports bleiben proprietär (OpenVuln-Service), die Fixes sind öffentlich nachvollziehbar: [Ohm PR #9](https://github.com/derKosi/Ohm/pull/9), [UnixDirStat PR #1](https://github.com/derKosi/UnixDirStat/pull/1) – inklusive Regressionstests und PoC-Beschreibungen in den PR-Bodies.

*Danke an das OpenVuln-Team – solche Services machen Open Source messbar sicherer, genau dann, wenn die Zeit des Maintainers knapp ist.*
