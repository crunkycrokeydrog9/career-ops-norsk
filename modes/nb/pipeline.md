# Modus: pipeline — URL-innboks (Second Brain)

Prosesserer URL-er til stillinger samlet i `data/pipeline.md`. Brukeren legger til URL-er når som helst og kjører deretter `/career-ops pipeline` for å prosessere alle.

## Arbeidsflyt

1. **Les** `data/pipeline.md` → finn elementer `- [ ]` i seksjonen "Ventende"
2. **For hver ventende URL**:
   a. Beregn neste sekvensielle `RAPPORT_NR` (les `reports/`, ta høyeste nummer + 1)
   b. **Hent JD** med Playwright (browser_navigate + browser_snapshot) → WebFetch → WebSearch
   c. Hvis URL-en ikke er tilgjengelig → merk som `- [!]` med notat og fortsett
   d. **Kjør komplett auto-pipeline**: Evaluering A-F → Rapport .md → PDF (hvis score >= 3.0) → Tracker
   e. **Flytt fra "Ventende" til "Prosesserte"**: `- [x] #NNN | URL | Selskap | Rolle | Score/5 | PDF ✅/❌`
3. **Hvis det er 3+ ventende URL-er**, start agenter parallelt (Agent-verktøy med `run_in_background`) for å maksimere hastighet.
4. **Når ferdig**, vis oppsummeringstabell:

```
| # | Selskap | Rolle | Score | PDF | Anbefalt handling |
```

## Format for pipeline.md

```markdown
## Ventende
- [ ] https://jobs.example.com/posting/123
- [ ] https://finn.no/job/fulltime/ad/123456 | Selskap AS | Senior Utvikler
- [!] https://private.url/job — Feil: innlogging kreves

## Prosesserte
- [x] #143 | https://jobs.example.com/posting/789 | Acme AS | AI PM | 4.2/5 | PDF ✅
- [x] #144 | https://finn.no/job/fulltime/ad/789012 | StorSelskap | SA | 2.1/5 | PDF ❌
```

## Intelligent JD-deteksjon fra URL

1. **Playwright (foretrukket):** `browser_navigate` + `browser_snapshot`. Fungerer med alle SPA-er.
2. **WebFetch (fallback):** For statiske sider eller når Playwright ikke er tilgjengelig.
3. **WebSearch (siste utvei):** Søk i sekundære portaler som indekserer JD-en.

**Spesialtilfeller:**
- **LinkedIn**: Kan kreve innlogging → merk `[!]` og be brukeren lime inn teksten
- **Finn.no**: Bruk WebFetch — de fleste Finn-annonser er statisk HTML
- **PDF**: Hvis URL-en peker til en PDF, les den direkte med Read-verktøy
- **`local:` prefiks**: Les lokal fil. Eksempel: `local:jds/linkedin-pm-ai.md` → les `jds/linkedin-pm-ai.md`

## Automatisk nummerering

1. List alle filer i `reports/`
2. Hent ut nummeret fra prefikset (f.eks. `142-medispend...` → 142)
3. Nytt nummer = høyeste funnet + 1

## Kildesynkronisering

Før prosessering av noen URL, verifiser synk:
```bash
node cv-sync-check.mjs
```
Hvis det er desynkronisering, advar brukeren før du fortsetter.
