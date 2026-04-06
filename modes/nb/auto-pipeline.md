# Modus: auto-pipeline — Komplett automatisk pipeline

Når brukeren limer inn en JD (tekst eller URL) uten eksplisitt underkommando, kjør HELE pipelinen i sekvens:

## Steg 0 — Hent JD

Hvis input er en **URL** (ikke innlimt JD-tekst), følg denne strategien for å hente innholdet:

**Prioritetsrekkefølge:**

1. **Playwright (foretrukket):** De fleste stillingsportaler (Lever, Ashby, Greenhouse, Workday) er SPA-er. Bruk `browser_navigate` + `browser_snapshot` for å rendere og lese JD-en.
2. **WebFetch (fallback):** For statiske sider (ZipRecruiter, Finn.no, company career pages).
3. **WebSearch (siste utvei):** Søk tittel + selskap i sekundære portaler som indekserer JD-en i statisk HTML.

**Hvis ingen metode fungerer:** Be kandidaten lime inn JD-en manuelt eller dele et skjermbilde.

**Hvis input er JD-tekst** (ikke URL): bruk direkte, ingen henting nødvendig.

## Steg 1 — Evaluering A-F
Kjør nøyaktig som modus `tilbud` (les `modes/nb/tilbud.md` for alle blokker A-F).

## Steg 2 — Lagre rapport .md
Lagre den komplette evalueringen i `reports/{###}-{selskap-slug}-{YYYY-MM-DD}.md` (se format i `modes/nb/tilbud.md`).

## Steg 3 — Generer PDF
Kjør den komplette pipelinen fra `pdf` (les `modes/nb/pdf.md`).

## Steg 4 — Utkast til søknadssvar (bare hvis score >= 4.5)

Hvis den endelige scoren er >= 4.5, generer utkast til svar for søknadsskjemaet:

1. **Hent skjemaspørsmål**: Bruk Playwright for å navigere til skjemaet og ta snapshot. Hvis de ikke kan hentes, bruk de generiske spørsmålene.
2. **Generer svar** med riktig tone (se under).
3. **Lagre i rapporten** som seksjon `## G) Utkast til søknadssvar`.

### Generiske spørsmål (bruk hvis de ikke kan hentes fra skjemaet)

- Hvorfor er du interessert i denne stillingen?
- Hvorfor vil du jobbe hos [Selskap]?
- Fortell oss om et relevant prosjekt eller prestasjon
- Hva gjør deg til en god match for denne stillingen?
- Hvordan hørte du om denne stillingen?

### Tone for skjemasvar

**Posisjon: "Jeg velger dere."** Kandidaten har alternativer og velger dette selskapet av konkrete grunner.

**Toneregler:**
- **Selvsikker uten arroganse**: "Jeg har brukt det siste året på å bygge AI-agentsystemer i produksjon — deres rolle er der jeg vil bruke den erfaringen videre"
- **Selektiv uten hovmod**: "Jeg har vært bevisst på å finne et team der jeg kan bidra meningsfullt fra dag én"
- **Spesifikk og konkret**: Alltid referere noe EKTE fra JD-en eller selskapet, og noe EKTE fra kandidatens erfaring
- **Direkte, uten fyllprat**: 2-4 setninger per svar. Ingen "Jeg brenner for..." eller "Jeg vil gjerne ha muligheten til..."
- **Beviset er kroken, ikke påstanden**: I stedet for "Jeg er flink til X", si "Jeg bygde X som gjør Y"

**Rammeverk per spørsmål:**
- **Hvorfor denne stillingen?** → "Deres [spesifikke ting] matcher direkte det [spesifikke ting jeg bygde]."
- **Hvorfor dette selskapet?** → Nevn noe konkret om selskapet. "Jeg har brukt [produkt] til [formål/tid]."
- **Relevant erfaring?** → Et kvantifisert bevisstykke. "Bygde [X] som [metrikk]. Solgte selskapet i 2025."
- **God match?** → "Jeg sitter i skjæringspunktet mellom [A] og [B], som er nøyaktig der denne rollen befinner seg."
- **Hvordan hørte du?** → Ærlig: "Fant via [portal/scan], evaluerte mot mine kriterier, og den scoret høyest."

**Språk**: Alltid på språket i JD-en (NO for norske, EN for engelske). For engelsk tekst: native tech-engelsk, ikke oversatt.

## Steg 5 — Oppdater tracker
Registrer i `data/applications.md` med alle kolonner inkludert Rapport og PDF med ✅.

**Hvis et steg feiler**, fortsett med de neste og merk det feilede steget som ventende i trackeren.
