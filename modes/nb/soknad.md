# Modus: soknad — Søknadsassistent i sanntid

Interaktiv modus for når kandidaten fyller ut et søknadsskjema i Chrome. Leser det som er på skjermen, laster forrige kontekst fra evalueringen, og genererer personaliserte svar for hvert spørsmål i skjemaet.

## Krav

- **Best med Playwright synlig**: I synlig modus ser kandidaten nettleseren og Claude kan interagere med siden.
- **Uten Playwright**: kandidaten deler et skjermbilde eller limer inn spørsmålene manuelt.

## Arbeidsflyt

```
1. OPPDAGE    → Les aktiv Chrome-fane (skjermbilde/URL/tittel)
2. IDENTIFISER → Hent ut selskap + rolle fra siden
3. SØK       → Match mot eksisterende rapporter i reports/
4. LAST      → Les komplett rapport + Seksjon G (om den finnes)
5. SAMMENLIGN → Er rollen på skjermen den samme som den evaluerte? Hvis endret → varsle
6. ANALYSER  → Identifiser ALLE synlige spørsmål i skjemaet
7. GENERER   → For hvert spørsmål, generer personalisert svar
8. PRESENTER → Vis formaterte svar klare for copy-paste
```

## Steg 1 — Oppdage stillingen

**Med Playwright:** Ta snapshot av aktiv side. Les tittel, URL og synlig innhold.

**Uten Playwright:** Be kandidaten om å:
- Dele et skjermbilde av skjemaet (Read-verktøy leser bilder)
- Eller lime inn spørsmålene fra skjemaet som tekst
- Eller oppgi selskap + rolle slik at vi kan søke det opp

## Steg 2 — Identifisere og søke kontekst

1. Hent ut selskapsnavn og stillingstittel fra siden
2. Søk i `reports/` etter selskapsnavn (Grep case-insensitive)
3. Hvis treff → last den komplette rapporten
4. Hvis Seksjon G finnes → last utkast til svar som grunnlag
5. Hvis INGEN treff → varsle og tilby å kjøre rask auto-pipeline

## Steg 3 — Oppdage endringer i rollen

Hvis rollen på skjermen avviker fra den evaluerte:
- **Varsle kandidaten**: "Rollen har endret seg fra [X] til [Y]. Vil du at jeg re-evaluerer eller tilpasser svarene til den nye tittelen?"
- **Hvis tilpasse**: Juster svarene til den nye rollen uten å re-evaluere
- **Hvis re-evaluere**: Kjør komplett A-F-evaluering, oppdater rapport, regenerer Seksjon G
- **Oppdater tracker**: Endre rolletittel i applications.md om nødvendig

## Steg 4 — Analysere skjemaspørsmål

Identifiser ALLE synlige spørsmål:
- Fritekstfelt (søknadsbrev, hvorfor denne rollen, osv.)
- Nedtrekkslister (hvordan hørte du om oss, arbeidstillatelse, osv.)
- Ja/Nei (relokasjon, visum, osv.)
- Lønnsfelt (område, forventning)
- Opplastingsfelt (CV, søknadsbrev-PDF)

Klassifiser hvert spørsmål:
- **Allerede besvart i Seksjon G** → tilpass eksisterende svar
- **Nytt spørsmål** → generer svar fra rapport + cv.md

## Steg 5 — Generere svar

For hvert spørsmål, generer svaret ved å følge:

1. **Kontekst fra rapporten**: Bruk bevisstykker fra blokk B, STAR-historier fra blokk F
2. **Tidligere Seksjon G**: Hvis det finnes et utkast, bruk det som grunnlag og finjuster
3. **Tone "Jeg velger dere"**: Samme rammeverk som auto-pipeline
4. **Spesifisitet**: Referer noe konkret fra JD-en som er synlig på skjermen
5. **career-ops bevisstykke**: Inkluder i "Tilleggsinformasjon" hvis det finnes felt for det

**Outputformat:**

```
## Svar for [Selskap] — [Rolle]

Basert på: Rapport #NNN | Score: X.X/5 | Arketype: [type]

---

### 1. [Eksakt spørsmål fra skjemaet]
> [Svar klart for copy-paste]

### 2. [Neste spørsmål]
> [Svar]

...

---

Notater:
- [Eventuelle observasjoner om rollen, endringer, osv.]
- [Forslag til personalisering som kandidaten bør gjennomgå]
```

## Steg 6 — Etter søknad (valgfritt)

Hvis kandidaten bekrefter at søknaden er sendt:
1. Oppdater status i `applications.md` fra "Evaluert" til "Søkt"
2. Oppdater Seksjon G i rapporten med de endelige svarene
3. Foreslå neste steg: `/career-ops kontakt` for LinkedIn-oppsøking

## Scrollhåndtering

Hvis skjemaet har flere spørsmål enn det som er synlig:
- Be kandidaten om å scrolle og dele et nytt skjermbilde
- Eller lime inn de gjenværende spørsmålene
- Prosesser i iterasjoner til hele skjemaet er dekket
