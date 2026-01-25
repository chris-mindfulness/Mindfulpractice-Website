# Projektanalyse: mbsr-web

Erstellt: Januar 2025
Kontext: Projekt wurde von einem Nicht-Programmierer mit Cursor Agent erstellt.

---

## Kurzüberblick

**Zweck:** Statische Informationsseite für psychologische Beratung und MBSR-Kurse (Mindfulness-Based Stress Reduction). Zielgruppe sind Interessenten für Achtsamkeitstraining in Leipzig.

**Tech-Stack:**
- **Framework:** Astro 4.16.0 (Static Site Generator)
- **Sprache:** JavaScript (kein TypeScript konfiguriert)
- **Styling:** Vanilla CSS mit CSS Custom Properties
- **Daten:** JSON-Dateien mit Schema-Definition
- **Deployment:** Statische Ausgabe für https://mindfulpractice.de

**Seiten:** 8 Routen (Startseite, MBSR, Workshops, Yoga, Über mich, Kontakt, Datenschutz, Impressum)

---

## Projektstruktur

```
mbsr-web/
├── src/
│   ├── components/        # Wiederverwendbare Astro-Komponenten
│   │   ├── Header.astro   # Navigation (inkl. Mobile-Menü)
│   │   ├── Footer.astro   # Footer mit Kontaktdaten
│   │   ├── Card.astro     # Generische Karten-Komponente
│   │   ├── CourseCard.astro
│   │   ├── PageHero.astro
│   │   └── Section.astro
│   │
│   ├── layouts/
│   │   └── BaseLayout.astro  # Haupt-Layout (HTML-Struktur, Meta-Tags)
│   │
│   ├── pages/             # Routen (1 Datei = 1 Seite)
│   │   ├── index.astro
│   │   ├── achtsamkeitstrainingmbsr.astro
│   │   ├── workshopsundvortraege.astro
│   │   ├── yogaundmehr.astro
│   │   ├── chris.astro
│   │   ├── kontakt.astro
│   │   ├── datenschutz.astro
│   │   └── impressum.astro
│   │
│   ├── data/              # Inhaltsdaten
│   │   ├── courses.json          # Kurstermine
│   │   ├── courses.schema.json   # JSON-Schema (nicht erzwungen)
│   │   ├── offers.json           # Angebote
│   │   └── testimonials.json     # Kundenstimmen
│   │
│   ├── styles/
│   │   └── global.css     # Gesamtes Styling (1.640 Zeilen)
│   │
│   └── assets/            # Bilder (werden von Astro optimiert)
│
├── public/                # Statische Dateien (unverarbeitet)
├── astro.config.mjs       # Astro-Konfiguration
└── package.json           # Abhängigkeiten (nur Astro)
```

**Wichtige Dateien:**
| Datei | Funktion |
|-------|----------|
| `src/layouts/BaseLayout.astro` | HTML-Grundgerüst, Meta-Tags, CSS-Import |
| `src/components/Header.astro` | Navigation + 70 Zeilen Inline-JavaScript |
| `src/styles/global.css` | Komplettes Styling inkl. 6 Farbthemen |
| `src/data/courses.json` | Kursdaten (Titel, Datum, Ort, Preis) |

---

## Aktuelle Stärken

1. **Saubere Komponentenstruktur**
   Klare Trennung von Layout, Komponenten und Seiten. Komponenten sind wiederverwendbar (`Card`, `Section`, `CourseCard`).

2. **Semantisches HTML**
   Korrekte Verwendung von `<article>`, `<address>`, `<dl>`, `<nav>`. Gute Grundlage für Barrierefreiheit.

3. **Responsive Design**
   Mobile-First-Ansatz mit Breakpoint bei 720px. Navigation funktioniert auf allen Geräten.

4. **CSS-Architektur**
   Durchdachtes Theming mit CSS Custom Properties. Jede Seite kann ein eigenes Farbschema haben (`theme--mbsr`, `theme--workshops` etc.).

5. **Datengetriebene Kursliste**
   Kurse werden aus `courses.json` geladen. Schema-Definition vorhanden.

6. **Barrierefreiheit (Grundlagen)**
   - `aria-label` auf interaktiven Elementen
   - `aria-hidden="true"` auf dekorativen SVGs
   - Keyboard-Navigation (Escape schließt Menü)
   - `lang="de"` gesetzt

7. **Performance-Grundlage**
   Statische Generierung, minimales JavaScript, Bilder werden zu WebP konvertiert.

---

## Kritische Probleme

### 1. Riesige Bilddatei
**Datei:** `src/assets/ini_med.jpeg`
**Problem:** 19,9 MB – extrem groß für Web
**Auswirkung:** Lange Ladezeiten, hoher Datenverbrauch
**Lösung:** Bild vor Upload komprimieren (Ziel: < 500 KB)

### 2. Hartcodierte Kontaktdaten
**Betroffene Dateien:**
- `Header.astro`
- `Footer.astro` (Zeile 16)
- `kontakt.astro` (Zeile 22)
- `achtsamkeitstrainingmbsr.astro` (Zeile 208)
- `impressum.astro` (Zeile 28)

**Problem:** Email und Telefon an 5+ Stellen dupliziert
**Risiko:** Bei Änderung werden Stellen vergessen
**Lösung:** Zentrale Datei `data/contact.json`

### 3. Doppelte Kurstermine
**Problem:** Kursdaten existieren in `courses.json`, aber die "Journey Stepper"-Grafik auf der Startseite (`index.astro`, Zeilen 164-195) enthält hartcodierte Daten:
```html
<li><span class="label">Termin 1</span><time>25.04.25</time></li>
```
**Risiko:** Daten werden inkonsistent
**Lösung:** Stepper aus `courses.json` generieren

### 4. Fehlende SEO-Metadaten
**Was fehlt:**
- Open Graph Tags (`og:title`, `og:image`, `og:description`)
- Structured Data (JSON-LD für `LocalBusiness`, `Course`)
- `sitemap.xml`
- `robots.txt`
- Canonical URLs

**Auswirkung:** Schlechte Social-Media-Previews, reduzierte Google-Sichtbarkeit

### 5. Navigation hartcodiert
**Datei:** `Header.astro`, Zeilen 2-16
**Problem:** `navItems`-Array direkt im Code
**Lösung:** Nach `data/navigation.json` verschieben

---

## Mittlere Probleme

### 6. Keine TypeScript-Unterstützung
- Keine Prop-Validierung in Komponenten
- IDE-Autovervollständigung eingeschränkt
- Fehler werden erst zur Laufzeit sichtbar

### 7. Code-Duplikation: Datumsformatierung
**Identischer Code in:**
- `src/pages/index.astro` (Zeilen 20-24)
- `src/components/CourseCard.astro` (Zeilen 4-8)

```javascript
const formatter = new Intl.DateTimeFormat("de-DE", {
  day: "2-digit",
  month: "short",
  year: "numeric"
});
```

### 8. Magische Zahlen
**Datei:** `index.astro`, Zeilen 14-17
```javascript
const courseCount = 25;
const avgParticipants = 11;
```
Diese Statistiken sollten aus einer Datendatei kommen.

### 9. Ungenutztes JSON-Schema
`courses.schema.json` existiert, wird aber nicht zur Build-Zeit validiert. Fehlerhafte Daten könnten unbemerkt live gehen.

### 10. CSS-Umfang
- 1.640 Zeilen für 8 Seiten
- SVG-Icons als Data-URIs eingebettet (schwer zu pflegen)
- Einige ungenutzte Klassen (`.site-header--stacked`)

### 11. Keine Entwickler-Tools
- Kein ESLint (Code-Qualität)
- Kein Prettier (Formatierung)
- Kein Pre-Commit-Hook

---

## Kurzfristige Relevanz

**Sofort relevant (vor nächstem Deployment):**
| Problem | Grund |
|---------|-------|
| Riesige Bilddatei | Performance, Nutzer-Absprungrate |
| Fehlende Open Graph Tags | Social-Media-Verlinkungen funktionieren schlecht |

**Relevant bei nächster Kursänderung:**
| Problem | Grund |
|---------|-------|
| Doppelte Kurstermine | Inkonsistenz-Risiko |
| Hartcodierte Kontaktdaten | Fehleranfälligkeit |

**Relevant für langfristige Wartbarkeit:**
| Problem | Grund |
|---------|-------|
| Keine TypeScript-Unterstützung | Entwicklererfahrung |
| Keine Linting-Tools | Code-Konsistenz |

---

## Empfohlene Reihenfolge für Verbesserungen

Die folgende Reihenfolge ist **minimal-invasiv** – jeder Schritt ist unabhängig und kann einzeln umgesetzt werden.

### Phase 1: Quick Wins (keine Strukturänderung)

1. **Bild komprimieren**
   - `ini_med.jpeg` extern komprimieren (z.B. squoosh.app)
   - Ersetzt eine Datei, kein Code-Change

2. **Open Graph Tags hinzufügen**
   - Nur `BaseLayout.astro` erweitern
   - Ca. 10 Zeilen zusätzlich

3. **`robots.txt` erstellen**
   - Neue Datei in `public/`
   - 3 Zeilen

### Phase 2: Daten zentralisieren (geringe Strukturänderung)

4. **`data/contact.json` erstellen**
   - Neue Datei mit Email, Telefon, Adresse
   - Betroffene Komponenten importieren diese Datei

5. **Navigation nach `data/navigation.json`**
   - `Header.astro` importiert statt hartcodiert

6. **Journey-Stepper aus `courses.json` generieren**
   - `index.astro` anpassen
   - Entfernt Duplikation

### Phase 3: Entwickler-Tooling (optional)

7. **TypeScript aktivieren**
   - `tsconfig.json` erstellen
   - Komponenten-Props typisieren

8. **ESLint + Prettier einrichten**
   - `package.json` erweitern
   - Konfigurationsdateien anlegen

### Phase 4: SEO-Optimierung (optional)

9. **Sitemap-Integration**
   - Astro-Plugin `@astrojs/sitemap` installieren

10. **JSON-LD Structured Data**
    - `LocalBusiness` und `Course` Schema
    - In `BaseLayout.astro` oder seitenspezifisch

---

## Fazit

Das Projekt ist für eine KI-gestützte Entwicklung durch einen Nicht-Programmierer **überdurchschnittlich gut strukturiert**. Die Hauptprobleme sind:

- **Wartbarkeit:** Duplizierte Daten an mehreren Stellen
- **Performance:** Eine zu große Bilddatei
- **Sichtbarkeit:** Fehlende SEO-Metadaten

Alle Probleme sind mit überschaubarem Aufwand lösbar. Die empfohlene Phase 1 kann in wenigen Stunden umgesetzt werden und bringt den größten Nutzen.
