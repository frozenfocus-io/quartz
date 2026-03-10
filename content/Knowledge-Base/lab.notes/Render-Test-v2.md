---
title: Render-Test v2
tags: [test, svg, bilder, wikilinks]
date: 2026-03-10
---

# Render-Test v2 — Bilder, SVG & Wikilinks

Dieses File testet alle neuen Features aus Schritt 3.

---

## 1. Obsidian Wikilinks

Interner Link auf eine andere Notiz: [[Render-Test]]

Mit Alias: [[Render-Test|Zum ersten Test]]

Beide sollten grün unterstrichen erscheinen (kein echter Link, nur visuell).

---

## 2. Bild via Wikilink

Das folgende sollte ein Bild aus `content/Attachments/` laden.
Wenn du noch kein Bild hochgeladen hast, siehst du einen broken-image Icon — das ist normal.

![[Code_Generated_Image.png]]

So sieht es mit einem SVG-Bild aus:

![[test-icon.svg]]

> Bilder hochladen: Edit-Modal öffnen → "📎 Bild hochladen" klicken → Datei wählen → `![[dateiname]]` wird automatisch eingefügt → Speichern.

---

## 3. Inline SVG — direktes Rendering

Der folgende SVG-Codeblock wird direkt gerendert (nicht als Code angezeigt):

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 120" width="400">
  <!-- H-Bridge Schematic (vereinfacht) -->
  <rect width="200" height="120" fill="none" stroke="#1a8f52" stroke-width="1" rx="4"/>

  <!-- VCC Label -->
  <text x="95" y="14" font-size="9" fill="#1a8f52" font-family="monospace">VCC</text>

  <!-- High-Side MOSFETs (oben) -->
  <rect x="30" y="18" width="32" height="18" rx="2" fill="#2a3a2a" stroke="#1a8f52" stroke-width="1.2"/>
  <text x="46" y="30" font-size="8" fill="#ccc" text-anchor="middle" font-family="monospace">Q1</text>

  <rect x="138" y="18" width="32" height="18" rx="2" fill="#2a3a2a" stroke="#1a8f52" stroke-width="1.2"/>
  <text x="154" y="30" font-size="8" fill="#ccc" text-anchor="middle" font-family="monospace">Q2</text>

  <!-- Low-Side MOSFETs (unten) -->
  <rect x="30" y="84" width="32" height="18" rx="2" fill="#2a3a2a" stroke="#4a7a4a" stroke-width="1.2"/>
  <text x="46" y="96" font-size="8" fill="#aaa" text-anchor="middle" font-family="monospace">Q3</text>

  <rect x="138" y="84" width="32" height="18" rx="2" fill="#2a3a2a" stroke="#4a7a4a" stroke-width="1.2"/>
  <text x="154" y="96" font-size="8" fill="#aaa" text-anchor="middle" font-family="monospace">Q4</text>

  <!-- Motor in der Mitte -->
  <circle cx="100" cy="60" r="18" fill="none" stroke="#888" stroke-width="1.5"/>
  <text x="100" y="64" font-size="9" fill="#aaa" text-anchor="middle" font-family="monospace">M</text>

  <!-- Verbindungslinien -->
  <line x1="62" y1="27" x2="82" y2="60" stroke="#1a8f52" stroke-width="1"/>
  <line x1="138" y1="27" x2="118" y2="60" stroke="#1a8f52" stroke-width="1"/>
  <line x1="62" y1="93" x2="82" y2="60" stroke="#4a7a4a" stroke-width="1"/>
  <line x1="138" y1="93" x2="118" y2="60" stroke="#4a7a4a" stroke-width="1"/>

  <!-- GND Label -->
  <text x="92" y="115" font-size="9" fill="#666" font-family="monospace">GND</text>
</svg>
```

---

## 4. SVG — Bode-Plot (Tiefpass)

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 180" width="500">
  <!-- Hintergrund -->
  <rect width="320" height="180" fill="#0d1a12" rx="4"/>

  <!-- Titel -->
  <text x="160" y="16" font-size="10" fill="#1a8f52" text-anchor="middle" font-family="monospace">Tiefpass 1. Ordnung — Bode Betrag</text>

  <!-- Achsen -->
  <line x1="50" y1="20" x2="50" y2="150" stroke="#2a4a2a" stroke-width="1"/>
  <line x1="50" y1="150" x2="300" y2="150" stroke="#2a4a2a" stroke-width="1"/>

  <!-- Y-Achse Labels (dB) -->
  <text x="44" y="24" font-size="8" fill="#666" text-anchor="end" font-family="monospace">0</text>
  <text x="44" y="62" font-size="8" fill="#666" text-anchor="end" font-family="monospace">-20</text>
  <text x="44" y="100" font-size="8" fill="#666" text-anchor="end" font-family="monospace">-40</text>
  <text x="44" y="138" font-size="8" fill="#666" text-anchor="end" font-family="monospace">-60</text>

  <!-- X-Achse Labels (Hz) -->
  <text x="70"  y="162" font-size="7" fill="#666" text-anchor="middle" font-family="monospace">10</text>
  <text x="120" y="162" font-size="7" fill="#666" text-anchor="middle" font-family="monospace">100</text>
  <text x="170" y="162" font-size="7" fill="#666" text-anchor="middle" font-family="monospace">1k</text>
  <text x="220" y="162" font-size="7" fill="#666" text-anchor="middle" font-family="monospace">10k</text>
  <text x="270" y="162" font-size="7" fill="#666" text-anchor="middle" font-family="monospace">100k</text>

  <!-- Grid -->
  <line x1="120" y1="20" x2="120" y2="150" stroke="#1a2a1a" stroke-width="0.5" stroke-dasharray="2,3"/>
  <line x1="170" y1="20" x2="170" y2="150" stroke="#1a2a1a" stroke-width="0.5" stroke-dasharray="2,3"/>
  <line x1="220" y1="20" x2="220" y2="150" stroke="#1a2a1a" stroke-width="0.5" stroke-dasharray="2,3"/>
  <line x1="50" y1="62"  x2="300" y2="62"  stroke="#1a2a1a" stroke-width="0.5" stroke-dasharray="2,3"/>
  <line x1="50" y1="100" x2="300" y2="100" stroke="#1a2a1a" stroke-width="0.5" stroke-dasharray="2,3"/>

  <!-- -3dB Marker -->
  <line x1="170" y1="20" x2="170" y2="150" stroke="#f0a020" stroke-width="0.8" stroke-dasharray="3,2" opacity="0.6"/>
  <text x="172" y="35" font-size="7" fill="#f0a020" font-family="monospace">f_c = 159 Hz</text>
  <circle cx="170" cy="76" r="3" fill="#f0a020" opacity="0.8"/>
  <text x="175" y="80" font-size="7" fill="#f0a020" font-family="monospace">-3 dB</text>

  <!-- Bode Kurve: flach bis f_c, dann -20dB/Dekade -->
  <polyline points="50,24 170,24 220,62 270,100 300,124"
            fill="none" stroke="#1a8f52" stroke-width="2" stroke-linejoin="round"/>

  <!-- Asymptoten (gestrichelt) -->
  <polyline points="50,24 170,24"
            fill="none" stroke="#2a5a2a" stroke-width="1" stroke-dasharray="4,3"/>
  <polyline points="170,24 270,100"
            fill="none" stroke="#2a5a2a" stroke-width="1" stroke-dasharray="4,3"/>

  <!-- Legende -->
  <line x1="210" y1="125" x2="230" y2="125" stroke="#1a8f52" stroke-width="2"/>
  <text x="233" y="129" font-size="7" fill="#aaa" font-family="monospace">H(jω)</text>
  <line x1="210" y1="137" x2="230" y2="137" stroke="#2a5a2a" stroke-width="1" stroke-dasharray="4,3"/>
  <text x="233" y="141" font-size="7" fill="#666" font-family="monospace">Asymptote</text>
</svg>
```

---

## 5. Zur Erinnerung — LaTeX noch funktioniert?

Grenzfrequenz: $f_c = \frac{1}{2\pi RC}$

Mit $R = 10\,\text{k}\Omega$, $C = 100\,\text{nF}$:

$$f_c = \frac{1}{2\pi \cdot 10^4 \cdot 10^{-7}} \approx 159\,\text{Hz}$$

---

## 6. PDF Export Test

Wenn du auf "⬇ PDF" klickst sollte sich ein neues Fenster öffnen mit:
- Sauberem Layout (weiß, serifenlose Schrift)
- Titel + Tags + Datum im Header
- Allen gerenderten Inhalten (LaTeX, Code, SVG)
- "lab.notes · exportiert DD.MM.YYYY" im Footer
- Automatisch Browser-Print-Dialog

---

*Render-Test v2 abgeschlossen.*
