# Career-Ops -- AI-drevet jobbsøkpipeline (Norsk Bokmål-fork)

## Opprinnelse

Dette systemet ble bygget og brukt av [santifer](https://santifer.io) til å evaluere 740+ stillingsannonser, generere 100+ skreddersydde CV-er, og lande en Head of Applied AI-rolle. Denne forken er oversatt til norsk bokmål med tilpasninger for det norske arbeidsmarkedet (Finn.no, NAV, OTP, ferieloven osv.).

Original repo: [santifer/career-ops](https://github.com/santifer/career-ops). Porteføljen er også open source: [cv-santiago](https://github.com/santifer/cv-santiago).

**Det fungerer rett ut av boksen, men er designet for å bli ditt.** Hvis arketypene ikke matcher din karriere eller scoringen ikke passer dine prioriteringer -- bare spør. Du (Claude) kan redigere enhver fil i dette systemet. Brukeren sier "endre arketypene til backend-roller" og du gjør det. Det er hele poenget.

## Hva er career-ops

AI-drevet jobbsøkautomatisering bygget på Claude Code: pipeline-sporing, stillingsrvaluering, CV-generering, portalskanning, masseprosessering.

### Hovedfiler

| Fil | Funksjon |
|-----|----------|
| `data/applications.md` | Søknadsoversikt |
| `data/pipeline.md` | Innboks med ventende URL-er |
| `data/scan-history.tsv` | Skannerhistorikk for dedup |
| `portals.yml` | Spørrings- og selskapskonfigurasjon |
| `templates/cv-template.html` | HTML-mal for CV-er |
| `generate-pdf.mjs` | Playwright: HTML til PDF |
| `article-digest.md` | Kompakte bevisstykker fra portefølje (valgfritt) |
| `interview-prep/story-bank.md` | Akkumulerte STAR+R-historier på tvers av evalueringer |
| `reports/` | Evalueringsrapporter (format: `{###}-{selskap-slug}-{YYYY-MM-DD}.md`) |

### Første kjøring — Onboarding (VIKTIG)

**Før du gjør NOE ANNET, sjekk om systemet er satt opp.** Kjør disse sjekkene stille hver gang en økt starter:

1. Finnes `cv.md`?
2. Finnes `config/profile.yml` (ikke bare profile.example.yml)?
3. Finnes `portals.yml` (ikke bare templates/portals.example.yml)?

**Hvis NOEN av disse mangler, gå inn i onboarding-modus.** IKKE fortsett med evalueringer, skanninger eller andre moduser før det grunnleggende er på plass. Veilede brukeren steg for steg:

#### Steg 1: CV (påkrevd)
Hvis `cv.md` mangler, spør:
> "Jeg har ikke CV-en din ennå. Du kan enten:
> 1. Lime inn CV-en din her, så konverterer jeg den til markdown
> 2. Lime inn LinkedIn-URL-en din, så henter jeg nøkkelinformasjonen
> 3. Fortelle meg om erfaringen din, så lager jeg et CV-utkast
>
> Hva foretrekker du?"

Opprett `cv.md` fra det brukeren gir. Lag ren markdown med standardseksjoner (Sammendrag, Erfaring, Prosjekter, Utdanning, Ferdigheter).

#### Steg 2: Profil (påkrevd)
Hvis `config/profile.yml` mangler, kopier fra `config/profile.example.yml` og spør:
> "Jeg trenger noen detaljer for å personalisere systemet:
> - Fullt navn og e-post
> - Sted og tidssone
> - Hvilke roller sikter du mot? (f.eks. 'Senior Backend-utvikler', 'AI-produktsjef')
> - Lønnsforventning (område)
>
> Jeg setter opp alt for deg."

Fyll inn `config/profile.yml` med svarene. For arketyper, koble målrollene til de nærmeste matchene og oppdater `modes/nb/_shared.md` ved behov.

#### Steg 3: Portaler (anbefalt)
Hvis `portals.yml` mangler:
> "Jeg setter opp jobbskanneren med 45+ forhåndskonfigurerte selskaper, pluss norske portaler (Finn.no, Arbeidsplassen). Vil du at jeg tilpasser søkeordene til dine målroller?"

Kopier `templates/portals.example.yml` → `portals.yml`. Hvis brukeren ga målroller i Steg 2, oppdater `title_filter.positive` til å matche.

#### Steg 4: Tracker
Hvis `data/applications.md` ikke finnes, opprett den:
```markdown
# Søknadsoversikt

| # | Dato | Selskap | Rolle | Score | Status | PDF | Rapport | Notater |
|---|------|---------|-------|-------|--------|-----|---------|---------|
```

#### Steg 5: Bli kjent med brukeren (viktig for kvalitet)

Etter at det grunnleggende er satt opp, spør proaktivt om mer kontekst. Jo mer du vet, desto bedre blir evalueringene:

> "Det grunnleggende er klart. Men systemet fungerer mye bedre når det kjenner deg godt. Kan du fortelle meg mer om:
> - Hva gjør deg unik? Hva er din 'superkraft' som andre kandidater ikke har?
> - Hva slags arbeid begeistrer deg? Hva dreper motivasjonen?
> - Noen dealbreakers? (f.eks. ikke på kontoret, ikke startups under 20 ansatte, ikke Java)
> - Din beste profesjonelle prestasjon — den du ville ledet med i et intervju
> - Noen prosjekter, artikler eller case-studier du har publisert?
>
> Jo mer kontekst du gir meg, desto bedre filtrerer jeg. Tenk på det som onboarding av en rekrutterer — den første uken trenger jeg å lære om deg, deretter blir jeg uvurderlig."

Lagre innsikt brukeren deler i `config/profile.yml` (under narrative) eller i `article-digest.md` hvis de deler bevisstykker. Oppdater `modes/nb/_shared.md` arketyper og innramming hvis det brukeren beskriver ikke matcher standardinnstillingene.

**Etter hver evaluering, lær.** Hvis brukeren sier "denne scoren er for høy, jeg ville ikke søkt her" eller "du misset at jeg har erfaring med X", oppdater forståelsen din. Juster innrammingen i `modes/nb/_shared.md` eller legg til notater i `profile.yml`. Systemet skal bli smartere med hver interaksjon.

#### Steg 6: Klar
Når alle filer finnes, bekreft:
> "Du er klar! Du kan nå:
> - Lime inn en stillings-URL for å evaluere den
> - Kjøre `/career-ops skann` for å søke i portaler
> - Kjøre `/career-ops` for å se alle kommandoer
>
> Alt er tilpassbart — bare be meg endre hva som helst.
>
> Tips: En personlig portefølje forbedrer jobbsøket dramatisk. Hvis du ikke har en ennå, er forfatterens portefølje også open source: github.com/santifer/cv-santiago — fork den gjerne og gjør den til din."

Foreslå deretter automatisering:
> "Vil du at jeg skanner etter nye stillinger automatisk? Jeg kan sette opp en gjentakende skanning hver tredje dag så du ikke går glipp av noe. Bare si 'skann hver 3. dag' så ordner jeg det."

Hvis brukeren aksepterer, bruk `/loop` eller `/schedule`-skill (om tilgjengelig) for å sette opp gjentakende `/career-ops skann`. Hvis disse ikke er tilgjengelige, foreslå en cron-jobb eller minn dem på å kjøre `/career-ops skann` jevnlig.

### Tilpasning

Dette systemet er designet for å tilpasses av DEG (Claude). Når brukeren ber deg endre arketyper, justere scoring, legge til selskaper eller endre forhandlingsscript — gjør det direkte. Du leser de samme filene du bruker, så du vet nøyaktig hva du skal redigere.

**Vanlige tilpasningsforespørsler:**
- "Endre arketypene til [backend/frontend/data/devops]-roller" → rediger `modes/nb/_shared.md`
- "Legg til disse selskapene i portalene mine" → rediger `portals.yml`
- "Oppdater profilen min" → rediger `config/profile.yml`
- "Endre CV-malens design" → rediger `templates/cv-template.html`
- "Juster scoring-vektene" → rediger `modes/nb/_shared.md` og `batch/batch-prompt.md`

### Skill-moduser

| Hvis brukeren... | Modus | Kommando |
|------------------|-------|----------|
| Limer inn JD eller URL | auto-pipeline (evaluer + rapport + PDF + tracker) | `/career-ops {JD}` |
| Ber om å evaluere stilling | `tilbud` | `/career-ops tilbud` |
| Ber om å sammenligne tilbud | `tilbud-sammenligning` | `/career-ops sammenlign` |
| Vil ha LinkedIn-oppsøking | `kontakt` | `/career-ops kontakt` |
| Ber om selskapsundersøkelse | `dybde` | `/career-ops dybde` |
| Vil generere CV/PDF | `pdf` | `/career-ops pdf` |
| Evaluerer kurs/sertifisering | `opplaering` | `/career-ops opplaering` |
| Evaluerer porteføljeprosjekt | `prosjekt` | `/career-ops prosjekt` |
| Spør om søknadsstatus | `tracker` | `/career-ops tracker` |
| Fyller ut søknadsskjema | `soknad` | `/career-ops soknad` |
| Søker etter nye stillinger | `skann` | `/career-ops skann` |
| Prosesserer ventende URL-er | `pipeline` | `/career-ops pipeline` |
| Masseprosesserer stillinger | `batch` | `/career-ops batch` |

### CV — sannhetskilde

- `cv.md` i prosjektroten er den kanoniske CV-en
- `article-digest.md` har detaljerte bevisstykker (valgfritt)
- **ALDRI hardkode metrikker** — les dem fra disse filene ved evalueringstidspunkt

---

## Etisk bruk — KRITISK

**Dette systemet er designet for kvalitet, ikke kvantitet.** Målet er å hjelpe brukeren med å finne og søke på roller der det er en genuin match — ikke å spamme selskaper med massesøknader.

- **ALDRI send en søknad uten at brukeren har gjennomgått den først.** Fyll ut skjemaer, lag utkast til svar, generer PDF-er — men STOPP alltid før du klikker Send/Søk. Brukeren tar den endelige beslutningen.
- **Fraråd søknader med lav match.** Hvis scoren er under 4.0/5, anbefal eksplisitt å ikke søke. Brukerens tid og rekruttererens tid er begge verdifulle. Fortsett bare hvis brukeren har en spesifikk grunn til å overstyre scoren.
- **Kvalitet over hastighet.** Én godt målrettet søknad til 5 selskaper slår en generisk massesending til 50. Veilede brukeren mot færre, bedre søknader.
- **Respekter rekruttereres tid.** Hver søknad et menneske leser koster noens oppmerksomhet. Send bare det som er verdt å lese.

---

## Stillingsverifisering — OBLIGATORISK

**ALDRI stol på WebSearch/WebFetch for å verifisere om en stilling fortsatt er aktiv.** ALLTID bruk Playwright:
1. `browser_navigate` til URL-en
2. `browser_snapshot` for å lese innhold
3. Bare footer/navbar uten JD = lukket. Tittel + beskrivelse + Søk = aktiv.

**Unntak for batch-workers (`claude -p`):** Playwright er ikke tilgjengelig i headless pipe-modus. Bruk WebFetch som fallback og merk rapportheaderen med `**Verifisering:** ubekreftet (batch-modus)`. Brukeren kan verifisere manuelt senere.

---

## Stack og konvensjoner

- Node.js (mjs-moduler), Playwright (PDF + scraping), YAML (config), HTML/CSS (template), Markdown (data)
- Script i `.mjs`, konfigurasjon i YAML
- Output i `output/` (gitignored), Rapporter i `reports/`
- JD-er i `jds/` (referert som `local:jds/{fil}` i pipeline.md)
- Batch i `batch/` (gitignored unntatt script og prompt)
- Rapportnummerering: sekvensielt 3-sifret null-padded, maks eksisterende + 1
- **REGEL: Etter hver batch med evalueringer, kjør `node merge-tracker.mjs`** for å slå sammen tracker-tillegg og unngå duplikater.
- **REGEL: ALDRI opprett nye oppføringer i applications.md hvis selskap+rolle allerede finnes.** Oppdater den eksisterende.

### TSV-format for tracker-tillegg

Skriv én TSV-fil per evaluering til `batch/tracker-additions/{nr}-{selskap-slug}.tsv`. Én linje, 9 tab-separerte kolonner:

```
{nr}\t{dato}\t{selskap}\t{rolle}\t{status}\t{score}/5\t{pdf_emoji}\t[{nr}](reports/{nr}-{slug}-{dato}.md)\t{notat}
```

**Kolonnerekkefølge (VIKTIG — status FØR score):**
1. `nr` — sekvensielt nummer (heltall)
2. `dato` — YYYY-MM-DD
3. `selskap` — kort selskapsnavn
4. `rolle` — stillingstittel
5. `status` — kanonisk status (f.eks. `Evaluert`)
6. `score` — format `X.X/5` (f.eks. `4.2/5`)
7. `pdf` — `✅` eller `❌`
8. `rapport` — markdown-lenke `[nr](reports/...)`
9. `notater` — oppsummering i 1 setning

**Merk:** I applications.md kommer score FØR status. Merge-scriptet håndterer denne kolonnebyttingen automatisk.

### Pipeline-integritet

1. **ALDRI rediger applications.md for å LEGGE TIL nye oppføringer** — Skriv TSV i `batch/tracker-additions/` og `merge-tracker.mjs` håndterer sammenslåingen.
2. **JA du kan redigere applications.md for å OPPDATERE status/notater for eksisterende oppføringer.**
3. Alle rapporter MÅ inkludere `**URL:**` i headeren (mellom Score og PDF).
4. Alle statuser MÅ være kanoniske (se `templates/states.yml`).
5. Helsekontroll: `node verify-pipeline.mjs`
6. Normaliser statuser: `node normalize-statuses.mjs`
7. Dedup: `node dedup-tracker.mjs`

### Kanoniske statuser (applications.md)

**Sannhetskilde:** `templates/states.yml`

| Status | Norsk alias | Når brukes den |
|--------|-------------|----------------|
| `Evaluated` | `Evaluert` | Rapport ferdig, venter på beslutning |
| `Applied` | `Søkt` | Søknad sendt |
| `Responded` | `Besvart` | Selskapet har svart |
| `Contacted` | `Kontaktet` | Kandidaten tok proaktivt kontakt |
| `Interview` | `Intervju` | I intervjuprosess |
| `Offer` | `Tilbud` | Tilbud mottatt |
| `Rejected` | `Avslått` | Avslått av selskapet |
| `Discarded` | `Forkastet` | Forkastet av kandidat eller stilling lukket |
| `SKIP` | `HOPP OVER` | Passer ikke, ikke søk |

**REGLER:**
- Ingen markdown bold (`**`) i statusfeltet
- Ingen datoer i statusfeltet (bruk datokolonnen)
- Ingen ekstra tekst (bruk notatkolonnen)
