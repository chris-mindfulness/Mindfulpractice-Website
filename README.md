# Psychologische Beratung · MBSR (Astro)

Statische Informationsseite für psychologische Beratung und MBSR.

## Lokal starten

- `npm install`
- `npm run dev`
- Lokal erreichbar unter `http://localhost:4321`.

## Content pflegen

- Kurse: `src/data/courses.json` (Schema: `src/data/courses.schema.json`).
- Angebote: `src/data/offers.json`.
- Testimonials: `src/data/testimonials.json` (derzeit nicht gerendert; keine Einbindung im Frontend).

## Navigation pflegen

- `navItems` sind in `src/components/Header.astro` definiert.
- Navigation ist als `<ul>` mit `.nav-list` umgesetzt (Styles in `src/styles/global.css`).

## Footer/Kontakt

- Footer-Links und Kontaktangaben werden in `src/components/Footer.astro` gepflegt.
- Kontaktadresse ist als `<address>` ausgezeichnet (Footer und Kontaktseite).

## Seitenübersicht (src/pages)

- `/` (`index.astro`)
- `/achtsamkeitstrainingmbsr`
- `/workshopsundvortraege`
- `/yogaundmehr`
- `/chris`
- `/kontakt`
- `/datenschutz`
- `/impressum`

## Build (statisch)

- `npm run build`
- `npm run preview`
