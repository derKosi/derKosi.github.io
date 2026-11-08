---
layout: post
title: "Struktur schlägt Sprache: micrograd bitweise in sechs Sprachen"
date: 2026-11-08 13:00:00 +0100
author: derKosi
lang: de
categories: [Performance, Experimente]
tags: [Benchmark, Rust, C#, Go, C, Lean 4, Python, Autograd, Determinismus, Reverse-Mode AD]
---

> *Experiment: karpathy/micrograd (skalares Autograd + MLP-Training) als bitweise reproduzierbarer Benchmark in Python, Rust (4 Varianten), Lean 4, Go, C# (3 Stufen) und C — jede Kurve per Golden Values verifiziert, plus ein maschinengeprüfter Korrektheitsbeweis in Lean. Repo ist noch privat (Link folgt nach Public-Entscheid), der Artikel ist die Fassung der README.*

## Die Frage

„Ist Rust schneller als C#?" ist eine Biergarten-Frage, weil „Rust" und „C#" keine Things sind, die man messen kann. Implementierungen sind Things. Also habe ich die Frage verengt, bis sie meßbar wurde:

> Derselbe Algorithmus (micrograd: skalares Autograd, Hinge-Loss, Full-Batch-SGD), derselbe Datensatz, dieselbe Op-Reihenfolge — wie schnell wird dieselbe Arbeit in jeder Sprache, und was macht eigentlich den Unterschied aus?

Der Clou ist der **bitwise-Vertrag**: Jeder Port muss für jede Epoche **bitwise identische** loss/acc-Kurven produzieren wie die CPython-Golden-Values. Nicht „ungefähr", nicht „in Toleranz" — identische IEEE-754-Bitmuster. Das macht jede Optimierung angreifbar: Wer die Op-Reihenfolge ändert (SIMD, Parallel-Reduktion, andere Summierung), fliegt sofort aus dem Vergleich raus, statt einen falschen Speedup zu produzieren.

## Der vertrackte erste Schritt: Python deterministisch machen

micrograd selbst ist nicht bitweise reproduzierbar: `Value._prev` ist ein `set(...)`, dessen Iterationsreihenfolge vom Objekt-Hash abhängt — die Gradienten-Akkumulations-Reihenfolge schwankt zwischen Läufen, Float-Ergebnisse differieren. Ein Ein-Zeilen-Patch in der vendierten Kopie (`tuple(_children)`, semantikgleich, der `visited`-Check fängt Duplikate) fixt das — und seitdem prüft ein Assert bei jedem Lauf, dass die Kurven stimmen.

Der Vertrag selbst regelt den Rest: float64 überall, sequenzielle Summen, keine Assoziativitäts-Tricks, Backward in umgekehrter topologischer Reihenfolge, Kinder in Einfügereihenfolge. Drei Workloads (37/337/697 Parameter, 100–1000 Punkte) als gemeinsame Messgrundlage, `work_per_s = Punkte·Parameter·Epochen/Zeit` als skalierungsfreies Maß.

## Der zweite Trick: Erzeugungsreihenfolge = topologische Ordnung

Im Tape-Schema werden Kinder immer vor Eltern erzeugt — die Erzeugungsreihenfolge *ist* bereits eine topologische Ordnung. Damit fällt micrograds DFS-Topo-Sort komplett weg (mitsamt dem `RecursionError`-Problem bei Full-Batch-Graphen). Bitwise-neutral ist das, weil nur Parameter mehrere Eltern haben und deren Gradienten-Beiträge in beiden Schemata in derselben Reihenfolge einlaufen. Konstanten (Eingaben, −y, 1.0, 1/n) können zusätzlich ins Op gefused werden (MulC/AddC) — ihr Gradient fließt nirgends hin, also ist Fusing bitwise-safe und spart Knoten und Schreibzugriffe.

## Die Gesamttabelle

*(Median aus 3 Läufen, gleiche Maschine, alle loss/acc-Kurven bitwise identisch)*

| Implementierung | klein | mittel | gross | Speedup (gross) |
|---|---|---|---|---|
| CPython (micrograd) | 0,75 s | 7,00 s | 10,65 s | 1× |
| Rust (rc) | 0,050 s | 0,761 s | 1,300 s | ~8× |
| Lean 4 (tape) | 0,046 s | 0,541 s | 0,982 s | ~11× |
| C# (classic) | 0,022 s | 0,432 s | 0,809 s | ~13× |
| Rust (arena) | 0,014 s | 0,101 s | 0,170 s | ~63× |
| Go (tape) | 0,011 s | 0,075 s | 0,127 s | ~84× |
| C# (fast_aot) | 0,006 s | 0,067 s | 0,083 s | ~128× |
| **C# (fast, JIT)** | **0,004 s** | **0,053 s** | 0,084 s | **~127×** |
| Rust (tape) | 0,005 s | 0,039 s | **0,073 s** | **~146×** |
| C (gcc −O3) | 0,005 s | 0,041 s | 0,071 s | ~151× |

## Was die Tabelle wirklich sagt

**1. Struktur schlägt Sprache — um Größenordnungen.** Innerhalb von Rust differieren rc und tape um **~15×** — der Sprachwechsel von CPython zu Rust(rc) bringt nur ~10×. Der große Sprung kommt nicht aus der Sprache, sondern aus dem Datenlayout: flache Arena/Tape statt Heap-Objekte, Indizes statt Pointer, kein Closure-Dispatch, keine Topo-Sortierung.

**2. Der Sprachwechsel ist fast egal — wenn die Struktur stimmt.** Die Spitzengruppe ist ein Vier-Wege-Gleichstand innerhalb des Messrauschens: Rust(tape), C, C#(fast) und Rust(tapeu), alle bei ~25–34 M Punkte·Parameter·Epochen/s — das ist die Speicherbandbreite dieser Maschine. Wer dort ankommt, ist angekommen, egal in welcher Sprache.

**3. C#-JIT schlägt NativeAOT.** Die idiomatic-Referenz liegt bei ~13×, die struct-Engine mit `AggressiveInlining` + TieredPGO bei ~127× — und die NativeAOT-Version desselben Codes bringt **keinen** zusätzlichen Gewinn (bei kleinen Workloads sogar langsamer, weil kein profilgesteuertes Inlining-Feedback). AOT-Vorteile wären Startup und Deployment, nicht Durchsatz.

**4. `unsafe` lohnt sich nicht.** Die einzige bewusste `unsafe`-Stelle des Projekts (Rust `tapeu`, `get_unchecked` statt Bounds-Checks) bringt keinen verlässlichen Gewinn — LLVM eliminiert die Checks im Hot Path offenbar selbst, und gcc-C ohne jede Prüfung ist gleich schnell. Bounds-Checks sind nicht der Flaschenhals.

**5. Der Beweis war Nebenprodukt und Hauptprodukt zugleich.** Der Lean-4-Port läuft mit derselben Tape-Struktur (Float-Austausch über IEEE-754-Bitmuster, weil Leans `Float.toString` nur 6 Stellen druckt) und landet bei ~11× — auf Höhe des objektbasierten Rust. Sein eigentlicher Output ist `Proof.lean`: Der rückwärts laufende Gradienten-Scan ist für alle Eingaben als exakte Kettenregel-Adjoints bewiesen (`bwd_eq_adj`, per Induktion über die Scan-Invariante). Geprüft mit `#print axioms` — **kein sorry**. Lean macht Netze nicht schneller. Es macht sie *garantiert*.

## Die Kriegsgeschichte (die jeder Hand-Parser verdient)

Die erste Version des C-Ports parste das Bits-Format mit Ziffern-Skipping. Ein Fehler in der LAYERS-Zeile desynchronisierte den Parser, und `npoints` wurde zu **771.804.264** — aus *einem* Punkt-Bitmuster. Linux-Overcommit erlaubte die 12-GB-Allokation, die Füll-Schleife berührte jede Seite, der OOM-Killer feuerte bei ~5,5 GB und warf nebenbei die WLAN-Verbindung raus. Der Parser ist jetzt streng zeilenbasiert, jede Zahl wird validiert, alle Grenzen werden *vor* jeder Allokation geprüft. Lehre: Ein Parser, der "irgendeine Zahl" liest, liest irgendwann 771 Millionen Punkte.

## Ehrliche Grenzen

Gemessen wurde auf *einem* Laptop (i3-3110M, single-threaded, Throttling) — Median aus 3 Läufen mit GC-Off, aber Schwankungen bis Faktor 2 bei kleinen Workloads sind real. Für die Kernaussagen (Layout-Hebel, JIT-vs-AOT, unsafe-Leere) reicht das klar; für belastbare *Sprach-Rankings* wäre mehr Maschine nötig. Effekte unter 20 % sind auf dieser Setup-Methode nicht belastbar — deshalb steht ASM-Cultivierung auch bewusst als „lohnt nicht" in der Doku: Der Algorithmus ist eine Kette skalarer Abhängigkeiten, SIMD würde die Op-Reihenfolge brechen, und am Limit sitzt ohnehin die Speicherbandbreite.

## Fazit

Wer „Sprache X ist schneller" diskutiert, diskutiert meist Datenlayout mit falschen Labels. Der Benchmark sagt: **Struktur schlägt Sprache ~15×, Sprache schlägt Struktur ~1×.** Und der Lean-Teil sagt: Wenn Korrektheit zählt, kann man sie für denselben Algorithmus maschinengeprüft bekommen — nur eben nicht schneller, sondern garantiert.

*Repo inkl. aller 7 Ports, JSON-Vertrag und Proof.lean folgt als Link, sobald die Public-Entscheidung (plus LICENSE) gefallen ist.*
