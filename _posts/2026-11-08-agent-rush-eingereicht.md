---
description: "agent-rush: untrusted Python-Pakete im gehärteten Container proben — Risk-Report plus Roh-Evidenz. Vultr Agent Rush, abgegeben."
layout: post
title: "agent-rush: Abgegeben bei Vultr Agent Rush — Evidenz statt Behauptung"
date: 2026-11-08 12:00:00 +0100
author: derKosi
lang: de
categories: [AI Agents, Wettbewerbe]
tags: [AI Security, Supply Chain, Docker, Sandbox, Vultr, Hackathon, LabLab]
---

> *Einreichung für den Vultr Agent Rush Hackathon (LabLab, 03.–08.11.), Track 1 „Blast Radius Zero" — **heute abgegeben, Ergebnis steht noch aus** und wird hier nachgetragen. [Quellcode](https://github.com/derKosi/agent-rush) public (MIT), [Projektseite](/projects/agent-rush/) mit allen Details.*

## Das Problem, das keiner haben will, aber alle haben

Ein KI-Agent soll ein Python-Paket installieren, das *niemand* gelesen hat. Was macht das Paket? Das README sagt: Datenverarbeitung. Das README lügt vielleicht. Security-Scanner sagen: keine bekannten CVEs — aber neuere Packages haben noch keine.

Die Frage ist also nicht „ist das Paket verdächtig?", sondern: **Wie kriegt man Beweise, ohne den eigenen Rechner zu opfern?**

## Was agent-rush tut

Ein Agent inspiziert das Source-Paket (sdist), plant Probes — und führt sie aus, aber in einem Container, der nichts tun darf:

- **Egress-Deny-Proxy** — jeder Verbindungsversuch nach draußen wird geloggt (JSONL), nichts durchgelassen
- **Read-only rootfs, non-root, cap-drop ALL**, pids/mem/cpu-Limits
- Auf einer Vultr-VM, damit das Ganze remote und wegwerfbar ist

Am Ende gibt es den Risk-Report (Verdict) — und das entscheidende Detail: **die Roh-Evidenz dazu.** Deny-Log, Prozess-Result, Filesystem-Diff. Was der Agent behauptet, kann man gegenloggen. Der Kernsatz des Projekts: *„An agent that only chats is a demo; an agent that executes safely is a product."*

## Die ehrlichen Teile

Zwei Grenzen stehen offen in der Doku, weil das Prinzip des Projekts Evidenz ist und Evidenz auch vor mir selbst nicht Halt macht: Ein detached Child-Prozess wird gespawnt, nicht blockiert (der Pid-Namespace begrenzt ihn, stoppt tut er nicht) — und der Selbstreport des Packages ist spoofbar; die Evidenz-Artefakte sind es nicht.

Der verifizierte End-to-End-Run: Egress 2×403 geloggt, read-only rootfs hält, Decoy-Key nicht lesbar, Verdict `high` (regelbasiert), **18 Sekunden** von Paket zu Report. 109 Tests in 3 Suiten, kein CI — bewusst: Verifikation soll durch echte Container-Runs passieren, nicht durch grüne Haken.

## Stand und was als Nächstes

Abgabe war heute (08.11.). Das Judging-Ergebnis trage ich **in diesem Post und auf der [Projektseite](/projects/agent-rush/) nach** — als Update am selben Ort, damit die Story komplett bleibt, ob der Preis kommt oder nicht.

Auch ohne Preis ist die These des Projekts für mich gültig: Agenten, die Code ausführen, brauchen Container mit ausführbaren Policy-Flags und Beweise statt Selbstauskunft. Genau das ist auch der Kern meiner Compliance-Linie — dieses Projekt ist der Containment-Baustein dafür.

*→ [Alle Wettbewerbe auf einen Blick](/wettbewerbe/)*
