# Modus: pdf — ATS-optimalisert PDF-generering

## Komplett pipeline

1. Les `cv.md` som sannhetskilde
2. Be brukeren om JD-en hvis den ikke er i kontekst (tekst eller URL)
3. Trekk ut 15-20 nøkkelord fra JD-en
4. Gjenkjenn språk i JD → CV-språk (NO for norske stillinger, EN default)
5. Gjenkjenn selskapslokasjon → papirformat:
   - US/Canada → `letter`
   - Resten av verden (inkl. Norge) → `a4`
6. Gjenkjenn arketype → tilpass innramming
7. Omskriv Professional Summary med nøkkelord fra JD + avgangsnarrativ-bro ("Bygde og solgte virksomhet. Bruker nå systemtenkning innen [JD-domene].")
8. Velg topp 3-4 mest relevante prosjekter for stillingen
9. Omorganiser kulepunkter i erfaring etter relevans for JD-en
10. Bygg kompetansegrid fra JD-krav (6-8 nøkkelfraser)
11. Injiser nøkkelord naturlig i eksisterende prestasjoner (ALDRI oppfinn)
12. Generer komplett HTML fra template + personalisert innhold
13. Skriv HTML til `/tmp/cv-kandidat-{selskap}.html`
14. Kjør: `node generate-pdf.mjs /tmp/cv-kandidat-{selskap}.html output/cv-kandidat-{selskap}-{YYYY-MM-DD}.pdf --format={letter|a4}`
15. Rapporter: PDF-sti, antall sider, % dekning av nøkkelord

## ATS-regler (ren parsing)

- Layout med én kolonne (ingen sidebarer, ingen parallelle kolonner)
- Standard overskrifter: "Professional Summary", "Work Experience", "Education", "Skills", "Certifications", "Projects"
  - For norske CV-er: "Faglig sammendrag", "Arbeidserfaring", "Utdanning", "Ferdigheter", "Sertifiseringer", "Prosjekter"
- Ingen tekst i bilder/SVG-er
- Ingen kritisk informasjon i headers/footers (ATS ignorerer dem)
- UTF-8, klikkbar tekst (ikke rasterisert)
- Ingen nestede tabeller
- Nøkkelord fra JD distribuert: Summary (topp 5), første kulepunkt i hver rolle, Skills-seksjon

## PDF-design

- **Fonter**: Space Grotesk (overskrifter, 600-700) + DM Sans (brødtekst, 400-500)
- **Fonter self-hosted**: `fonts/`
- **Header**: navn i Space Grotesk 24px bold + gradientlinje `linear-gradient(to right, hsl(187,74%,32%), hsl(270,70%,45%))` 2px + kontaktrad
- **Seksjonsoverskrifter**: Space Grotesk 13px, uppercase, letter-spacing 0.05em, farge cyan primary
- **Brødtekst**: DM Sans 11px, line-height 1.5
- **Selskapsnavn**: aksentfarge lilla `hsl(270,70%,45%)`
- **Marger**: 0.6in
- **Bakgrunn**: ren hvit

## Seksjonsrekkefølge (optimalisert "6-sekunders rekruttererskann")

1. Header (stort navn, gradient, kontakt, porteføljelenke)
2. Professional Summary (3-4 linjer, nøkkelordtett)
3. Core Competencies (6-8 nøkkelfraser i flex-grid)
4. Work Experience (omvendt kronologisk)
5. Projects (topp 3-4 mest relevante)
6. Education & Certifications
7. Skills (språk + tekniske)

## Nøkkelordinjeksjonsstrategi (etisk, sannhetsbasert)

Eksempler på legitim omformulering:
- JD sier "RAG pipelines" og CV sier "LLM-arbeidsflyter med henting" → endre til "RAG pipeline-design og LLM-orkestreringsarbeidsflyter"
- JD sier "MLOps" og CV sier "observerbarhet, evals, feilhåndtering" → endre til "MLOps og observerbarhet: evals, feilhåndtering, kostnadsovervåkning"
- JD sier "interessenthåndtering" og CV sier "samarbeidet med teamet" → endre til "interessenthåndtering på tvers av engineering, drift og forretning"

**ALDRI legg til ferdigheter som kandidaten ikke har. Bare omformuler reell erfaring med det eksakte ordforrådet fra JD-en.**

## HTML-template

Bruk templaten i `cv-template.html`. Erstatt plassholderne `{{...}}` med personalisert innhold:

| Plassholder | Innhold |
|-------------|---------|
| `{{LANG}}` | `no` eller `en` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) eller `210mm` (A4) |
| `{{NAME}}` | (fra profile.yml) |
| `{{EMAIL}}` | (fra profile.yml) |
| `{{LINKEDIN_URL}}` | [fra profile.yml] |
| `{{LINKEDIN_DISPLAY}}` | [fra profile.yml] |
| `{{PORTFOLIO_URL}}` | [fra profile.yml] (eller /no etter språk) |
| `{{PORTFOLIO_DISPLAY}}` | [fra profile.yml] (eller /no etter språk) |
| `{{LOCATION}}` | [fra profile.yml] |
| `{{SECTION_SUMMARY}}` | Professional Summary / Faglig sammendrag |
| `{{SUMMARY_TEXT}}` | Personalisert sammendrag med nøkkelord |
| `{{SECTION_COMPETENCIES}}` | Core Competencies / Kjernekompetanser |
| `{{COMPETENCIES}}` | `<span class="competency-tag">nøkkelord</span>` × 6-8 |
| `{{SECTION_EXPERIENCE}}` | Work Experience / Arbeidserfaring |
| `{{EXPERIENCE}}` | HTML for hver jobb med omordnede kulepunkter |
| `{{SECTION_PROJECTS}}` | Projects / Prosjekter |
| `{{PROJECTS}}` | HTML for topp 3-4 prosjekter |
| `{{SECTION_EDUCATION}}` | Education / Utdanning |
| `{{EDUCATION}}` | HTML for utdanning |
| `{{SECTION_CERTIFICATIONS}}` | Certifications / Sertifiseringer |
| `{{CERTIFICATIONS}}` | HTML for sertifiseringer |
| `{{SECTION_SKILLS}}` | Skills / Ferdigheter |
| `{{SKILLS}}` | HTML for ferdigheter |

## Etter generering

Oppdater tracker hvis stillingen allerede er registrert: endre PDF fra ❌ til ✅.
