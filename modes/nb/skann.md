# Modus: skann — Portalskanner (Stillingsoppdagelse)

Skanner stillingsportaler, filtrerer etter tittelrelevans, og legger til nye stillinger i pipelinen for senere evaluering.

## Anbefalt kjøring

Kjør som subagent for å ikke bruke kontekst i hoveddialogen:

```
Agent(
    subagent_type="general-purpose",
    prompt="[innhold fra denne filen + spesifikke data]",
    run_in_background=True
)
```

## Konfigurasjon

Les `portals.yml` som inneholder:
- `search_queries`: Liste med WebSearch-spørringer med `site:`-filtre per portal (bred oppdagelse)
- `tracked_companies`: Spesifikke selskaper med `careers_url` for direkte navigering
- `title_filter`: Positive/negative/seniority_boost-nøkkelord for tittelfiltrering

## Oppdagelsesstrategi (3 nivåer)

### Nivå 1 — Playwright direkte (HOVED)

**For hvert selskap i `tracked_companies`:** Naviger til `careers_url` med Playwright (`browser_navigate` + `browser_snapshot`), les ALLE synlige stillinger, og hent ut tittel + URL for hver. Dette er mest pålitelig fordi:
- Ser siden i sanntid (ikke hurtigbufrede Google-resultater)
- Fungerer med SPA-er (Ashby, Lever, Workday)
- Oppdager nye stillinger umiddelbart
- Avhenger ikke av Googles indeksering

**Hvert selskap MÅ ha `careers_url` i portals.yml.** Hvis den mangler, søk den opp én gang, lagre den, og bruk i fremtidige skanninger.

**Norske portaler:**
- **Finn.no**: `https://www.finn.no/job/fulltime/search.html?q={søkeord}` — Norges største jobbportal
- **Arbeidsplassen (NAV)**: `https://arbeidsplassen.nav.no/stillinger` — offentlige og private stillinger
- **LinkedIn Norge**: Bruk WebSearch med `site:linkedin.com/jobs` + norske nøkkelord
- **Kode24 jobber**: `https://www.kode24.no/jobb` — teknologistillinger

### Nivå 2 — Greenhouse API (SUPPLERENDE)

For selskaper med Greenhouse gir API-en (`boards-api.greenhouse.io/v1/boards/{slug}/jobs`) rene strukturerte data. Bruk som raskt supplement til Nivå 1 — raskere enn Playwright men fungerer bare med Greenhouse.

### Nivå 3 — WebSearch-spørringer (BRED OPPDAGELSE)

`search_queries` med `site:`-filtre dekker portaler på tvers (alle Ashby, alle Greenhouse, osv.). Nyttig for å oppdage NYE selskaper som ennå ikke er i `tracked_companies`, men resultatene kan være utdaterte.

**Prioritert rekkefølge:**
1. Nivå 1: Playwright → alle `tracked_companies` med `careers_url`
2. Nivå 2: API → alle `tracked_companies` med `api:`
3. Nivå 3: WebSearch → alle `search_queries` med `enabled: true`

Nivåene er additive — alle kjøres, resultatene blandes og dedupliseres.

## Arbeidsflyt

1. **Les konfigurasjon**: `portals.yml`
2. **Les historikk**: `data/scan-history.tsv` → URL-er allerede sett
3. **Les dedup-kilder**: `data/applications.md` + `data/pipeline.md`

4. **Nivå 1 — Playwright-skann** (parallelt i grupper på 3-5):
   For hvert selskap i `tracked_companies` med `enabled: true` og `careers_url` definert:
   a. `browser_navigate` til `careers_url`
   b. `browser_snapshot` for å lese alle stillinger
   c. Hvis siden har filtre/avdelinger, naviger relevante seksjoner
   d. For hver stilling hent ut: `{tittel, url, selskap}`
   e. Hvis siden paginerer resultater, naviger flere sider
   f. Akkumuler i kandidatliste
   g. Hvis `careers_url` feiler (404, redirect), prøv `scan_query` som fallback og noter for URL-oppdatering

5. **Nivå 2 — Greenhouse API-er** (parallelt):
   For hvert selskap i `tracked_companies` med `api:` definert og `enabled: true`:
   a. WebFetch av API-URL → JSON med stillingsliste
   b. For hver stilling hent ut: `{tittel, url, selskap}`
   c. Akkumuler i kandidatliste (dedup med Nivå 1)

6. **Nivå 3 — WebSearch-spørringer** (parallelt om mulig):
   For hver spørring i `search_queries` med `enabled: true`:
   a. Kjør WebSearch med definert `query`
   b. Fra hvert resultat hent ut: `{tittel, url, selskap}`
   c. Akkumuler i kandidatliste (dedup med Nivå 1+2)

7. **Filtrer etter tittel** med `title_filter` fra `portals.yml`:
   - Minst 1 nøkkelord fra `positive` må finnes i tittelen (case-insensitive)
   - 0 nøkkelord fra `negative` skal finnes
   - `seniority_boost`-nøkkelord gir prioritet men er ikke obligatoriske

8. **Dedupliser** mot 3 kilder:
   - `scan-history.tsv` → eksakt URL allerede sett
   - `applications.md` → selskap + normalisert rolle allerede evaluert
   - `pipeline.md` → eksakt URL allerede i ventende eller prosesserte

9. **For hver ny stilling som passerer filtre**:
   a. Legg til i `pipeline.md` seksjonen "Ventende": `- [ ] {url} | {selskap} | {tittel}`
   b. Registrer i `scan-history.tsv`: `{url}\t{dato}\t{spørringsnavn}\t{tittel}\t{selskap}\tadded`

10. **Stillinger filtrert etter tittel**: registrer i `scan-history.tsv` med status `skipped_title`
11. **Duplikater**: registrer med status `skipped_dup`

## Oppsummering

```
Portalskann — {YYYY-MM-DD}
━━━━━━━━━━━━━━━━━━━━━━━━━━
Spørringer kjørt: N
Stillinger funnet: N totalt
Filtrert etter tittel: N relevante
Duplikater: N (allerede evaluert eller i pipeline)
Nye lagt til i pipeline.md: N

  + {selskap} | {tittel} | {spørringsnavn}
  ...

→ Kjør /career-ops pipeline for å evaluere de nye stillingene.
```

## Håndtering av careers_url

Hvert selskap i `tracked_companies` bør ha `careers_url` — direkte URL til deres stillingsside.

**Kjente mønstre per plattform:**
- **Ashby:** `https://jobs.ashbyhq.com/{slug}`
- **Greenhouse:** `https://job-boards.greenhouse.io/{slug}` eller `https://job-boards.eu.greenhouse.io/{slug}`
- **Lever:** `https://jobs.lever.co/{slug}`
- **Finn.no:** `https://www.finn.no/job/fulltime/search.html?q={selskap}`
- **Egendefinert:** Selskapets egen URL (f.eks. `https://selskap.no/karriere`)

**Hvis `careers_url` ikke finnes** for et selskap:
1. Prøv plattformens kjente mønster
2. Hvis det feiler, gjør et raskt WebSearch: `"{selskap}" karriere stillinger`
3. Naviger med Playwright for å bekrefte at det fungerer
4. **Lagre funnet URL i portals.yml** for fremtidige skanninger

## Vedlikehold av portals.yml

- **ALLTID lagre `careers_url`** når et nytt selskap legges til
- Legg til nye spørringer etter hvert som portaler eller interessante roller oppdages
- Deaktiver spørringer med `enabled: false` hvis de genererer for mye støy
- Juster filtreringsnøkkelord etter hvert som målrollene utvikler seg
- Legg til selskaper i `tracked_companies` når det er interessant å følge dem tett
- Verifiser `careers_url` periodisk — selskaper bytter ATS-plattform
