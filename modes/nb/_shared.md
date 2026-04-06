# Delt kontekst -- career-ops (Norsk Bokmål)

<!-- ============================================================
     SLIK TILPASSER DU DENNE FILEN
     ============================================================
     Denne filen inneholder delt kontekst for alle career-ops-moduser.
     Før du bruker career-ops MÅ du:
     1. Fylle ut config/profile.yml med dine personlige data
     2. Opprette cv.md i prosjektroten
     3. (Valgfritt) Opprette article-digest.md med bevisstykker
     4. Tilpasse seksjonene merket med [TILPASS]
     ============================================================ -->

## Sannhetskilder (LES ALLTID før evaluering)

| Fil | Sti | Når |
|-----|-----|-----|
| cv.md | `cv.md` (prosjektrot) | ALLTID |
| article-digest.md | `article-digest.md` (om den finnes) | ALLTID (detaljerte bevisstykker) |
| profile.yml | `config/profile.yml` | ALLTID (kandidatidentitet og mål) |

**REGEL: ALDRI hardkode metrikker fra bevisstykker.** Les dem fra cv.md + article-digest.md ved evalueringstidspunkt.
**REGEL: For artikkel-/prosjektmetrikker har article-digest.md forrang over cv.md** (cv.md kan ha eldre tall).

---

## Nordstjerne -- Målroller

Systemet gjelder med LIK grundighet for ALLE målroller. Ingen er primær eller sekundær -- enhver er en suksess hvis kompensasjon og vekst stemmer:

| Arketype | Tematiske akser | Hva de kjøper |
|----------|-----------------|---------------|
| **AI Platform / LLMOps-ingeniør** | Evaluering, observerbarhet, pålitelighet, pipelines | Noen som setter AI i produksjon med metrikker |
| **Agentiske arbeidsflyter / Automatisering** | HITL, verktøy, orkestrering, multi-agent | Noen som bygger pålitelige agentsystemer |
| **Teknisk AI-produktsjef** | GenAI/Agenter, PRD-er, oppdagelse, leveranse | Noen som oversetter forretning til AI-produkt |
| **AI-løsningsarkitekt** | Hyperautomatisering, enterprise, integrasjoner | Noen som designer ende-til-ende AI-arkitekturer |
| **AI Forward Deployed Engineer** | Kundevendt, rask leveranse, prototyping | Noen som leverer AI-løsninger raskt til kunder |
| **AI-transformasjonsleder** | Endringsledelse, adopsjon, organisasjonsutvikling | Noen som leder AI-transformasjon i en organisasjon |

<!-- [TILPASS] Rediger arketypene over for å matche DINE målroller.
     For eksempel, om du er backend-utvikler, bytt ut med:
     - Senior Backend-utvikler
     - Staff Platform-ingeniør
     - Engineering Manager
     osv. -->

### Adaptiv innramming etter arketype

> **Konkrete metrikker: les fra `cv.md` + `article-digest.md` ved evalueringstidspunkt. ALDRI hardkode tall her.**

| Hvis rollen er... | Fremhev om kandidaten... | Beviskilder |
|-------------------|--------------------------|-------------|
| Platform / LLMOps | Produksjonssystembygger, observerbarhet, evals, lukket loop | article-digest.md + cv.md |
| Agentisk / Automatisering | Multi-agent-orkestrering, HITL, pålitelighet, kostnad | article-digest.md + cv.md |
| Teknisk AI PM | Produktoppdagelse, PRD-er, metrikker, interessenthåndtering | cv.md + article-digest.md |
| Løsningsarkitekt | Systemdesign, integrasjoner, enterprise-klar | article-digest.md + cv.md |
| Forward Deployed Engineer | Rask leveranse, kundevendt, prototype til prod | cv.md + article-digest.md |
| AI-transformasjonsleder | Endringsledelse, teamutvikling, adopsjon | cv.md + article-digest.md |

<!-- [TILPASS] Koble DINE spesifikke prosjekter/artikler til hver arketype over -->

### Avgangsnarrativ (bruk i ALLE innramminger)

<!-- [TILPASS] Bytt ut med DIN fortelling. Eksempler:
     - "Bygde og solgte min SaaS etter 5 år. Nå fokusert på anvendt AI i skala."
     - "Ledet engineering i et Series B-startup gjennom 10x vekst. Søker neste utfordring."
     - "Gikk fra konsulentvirksomhet til å bygge produkt. Ser etter roller med høyt eierskap."
     Les fra config/profile.yml → narrative.exit_story -->

Bruk kandidatens avgangshistorie fra `config/profile.yml` til å ramme inn ALT innhold:
- **I PDF-sammendrag:** Bro fra fortid til fremtid -- "Bruker nå samme [ferdighet] innen [JD-domene]."
- **I STAR-historier:** Referer bevisstykker fra article-digest.md
- **I utkast til svar (Seksjon G):** Overgangsnarrativet bør være i første svar.
- **Når JD-en ber om "entreprenørskap", "eierskap", "bygger", "ende-til-ende":** Dette er #1 differensiatoren. Øk match-vekt.

### Kryssgående fordel

Innram profilen som **"Teknisk bygger med bevis fra virkeligheten"** som tilpasser innrammingen til rollen:
- For PM: "bygger som reduserer usikkerhet med prototyper og produksjonssetter med disiplin"
- For FDE: "bygger som leverer raskt med observerbarhet og metrikker fra dag 1"
- For SA: "bygger som designer ende-til-ende-systemer med reell integrasjonserfaring"
- For LLMOps: "bygger som setter AI i produksjon med lukket-loop kvalitetssystemer"

Gjør "bygger" til et profesjonelt signal, ikke en "hobbymaker". Ekte bevisstykker gjør dette troverdig.

### Portefølje som bevisstykke (bruk i høyverdi-søknader)

<!-- [TILPASS] Hvis du har en live demo, dashboard, eller offentlig prosjekt, konfigurer det her.
     Eksempel:
     dashboard:
       url: "https://dittsite.dev/demo"
       password: "demo-2026"
       when_to_share: "LLMOps, AI Platform, observerbarhet-roller"
     Les fra config/profile.yml → narrative.proof_points og narrative.dashboard -->

Hvis kandidaten har en live demo/dashboard (sjekk profile.yml), tilby tilgang i søknader for relevante roller.

### Kompensasjonsintelligens

<!-- [TILPASS] Undersøk kompensasjonsrammer for DINE målroller og oppdater disse verdiene -->

**Generell veiledning:**
- Bruk WebSearch for aktuelle markedsdata (Glassdoor, Levels.fyi, Blind, Kode24 lønnsstatistikk, Tekna lønnskalkulator)
- Innram etter stillingstittel, ikke etter ferdigheter -- titler bestemmer lønnsbånd
- Kontraktørpriser er typisk 30-50% høyere enn fastansatt grunnlønn for å kompensere for goder
- Geografisk arbitrasje fungerer for remote-roller: lavere levekostnader = bedre netto

**Norsk arbeidsmarked -- viktige faktorer:**
- Feriepenger: 10,2% (12% for 60+) av feriepengegrunnlaget -- kommer i tillegg til lønn
- Obligatorisk tjenestepensjon (OTP): minimum 2% av lønn, mange arbeidsgivere tilbyr 5-7%
- Arbeidsgiveravgift: 14,1% (varierer med sone) -- dette er arbeidsgivers kostnad, ikke din
- 5 uker ferie (25 virkedager) er standard, mange har 5+1 uke
- Sykepenger: 100% av lønn i inntil 1 år (arbeidsgiver betaler de første 16 dagene)
- Oppsigelsestid: typisk 1-3 måneder gjensidig
- Lønnsforhandlinger i Norge er generelt mer moderate enn i USA -- fokuser på totalpakning

### Forhandlingsscript

<!-- [TILPASS] Tilpass disse til din situasjon -->

**Lønnsforventninger (generelt rammeverk):**
> "Basert på markedsdata for denne rollen sikter jeg mot [OMRÅDE fra profile.yml]. Jeg er fleksibel på struktur -- det som betyr mest er totalpakken og muligheten."

**Geografisk rabatt-tilbakevisning:**
> "Rollene jeg er konkurransedyktig for er resultatbaserte, ikke stedsbaserte. Min track record endres ikke basert på postnummer."

**Når tilbudet er under mål:**
> "Jeg sammenligner med muligheter i [høyere område]. Jeg tiltrekkes av [selskap] på grunn av [grunn]. Kan vi utforske [mål]?"

**Norsk-spesifikke forhandlingspunkter:**
> Vurder totalpakken: grunnlønn + OTP-sats + bonusordning + aksjer/opsjoner + forsikringer + fleksibilitet
> Spør alltid om OTP-sats, da forskjellen mellom 2% og 7% er betydelig over tid

### Stedspolicy

<!-- [TILPASS] Tilpass til din situasjon. Les fra config/profile.yml → location -->

**I skjemaer:**
- Binære "kan du være på kontoret?"-spørsmål: følg din faktiske tilgjengelighet fra profile.yml
- I fritekstfelt: spesifiser din tidssoneavdekning og tilgjengelighet

**I evalueringer (scoring):**
- Remote-dimensjon for hybrid utenfor ditt land: score **3.0** (ikke 1.0)
- Score bare 1.0 hvis JD-en eksplisitt sier "må være på kontoret 4-5 dager/uke, ingen unntak"

### Tid-til-tilbud-prioritet
- Fungerende demo + metrikker > perfeksjon
- Søk før > lær mer
- 80/20-tilnærming, tidsboks alt

---

## Globale regler

### ALDRI

1. Oppfinne erfaring eller metrikker
2. Endre cv.md eller porteføljefiler
3. Sende søknader på vegne av kandidaten
4. Dele telefonnummer i genererte meldinger
5. Anbefale kompensasjon under markedspris
6. Generere PDF uten å lese JD-en først
7. Bruke bedriftsklisjeer (corporate-speak)
8. Ignorere trackeren (hver evaluert stilling blir registrert)

### ALLTID

0. **Søknadsbrev:** Hvis skjemaet har mulighet for å legge ved eller skrive et søknadsbrev, ALLTID inkluder ett. Generer PDF med samme visuelle design som CV-en. Innhold: JD-sitater koblet til bevisstykker, lenker til relevante case-studier. Maks 1 side.
1. Les cv.md og article-digest.md (om den finnes) før evaluering av enhver stilling
1b. **Første evaluering i hver økt:** Kjør `node cv-sync-check.mjs` med Bash. Hvis den rapporterer advarsler, varsle kandidaten før du fortsetter
2. Gjenkjenn rollens arketype og tilpass innrammingen
3. Siter eksakte linjer fra CV-en ved matching
4. Bruk WebSearch for kompensasjon og selskapsdata
5. Registrer i tracker etter evaluering
6. Generer innhold på språket i JD-en (NO default for norske stillinger, EN for engelske)
7. Vær direkte og handlingsrettet -- ingen fyllprat
8. Når du genererer engelsk tekst (PDF-sammendrag, kulepunkter, LinkedIn-meldinger, STAR-historier): native tech-engelsk, ikke oversatt. Korte setninger, handlingsverb, unngå unødvendig passiv.
8b. **Case study-URL-er i PDF Professional Summary:** Hvis PDF-en nevner case-studier eller demoer, MÅ URL-ene stå i første avsnitt (Professional Summary). Rekruttereren leser kanskje bare sammendraget. Alle URL-er med `white-space: nowrap` i HTML.
9. **Tracker-tillegg som TSV** -- ALDRI rediger applications.md for å legge til nye oppføringer. Skriv TSV i `batch/tracker-additions/` og `merge-tracker.mjs` håndterer sammenslåingen.
10. **Inkluder `**URL:**` i hver rapportheader** -- mellom Score og PDF.

### Verktøy

| Verktøy | Bruk |
|---------|------|
| WebSearch | Kompensasjonsundersøkelser, trender, bedriftskultur, LinkedIn-kontakter, fallback for JD-er |
| WebFetch | Fallback for å hente JD-er fra statiske sider |
| Playwright | Verifiser om stillinger er aktive (browser_navigate + browser_snapshot), hent JD-er fra SPA-er. **KRITISK: ALDRI start 2+ agenter med Playwright parallelt -- de deler én nettleserinstans.** |
| Read | cv.md, article-digest.md, cv-template.html |
| Write | Midlertidig HTML for PDF, applications.md, rapporter .md |
| Edit | Oppdater tracker |
| Bash | `node generate-pdf.mjs` |
