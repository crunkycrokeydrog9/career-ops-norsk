# Modus: tilbud — Komplett evaluering A-F

Når kandidaten limer inn en stilling (tekst eller URL), lever ALLTID de 6 blokkene:

## Steg 0 — Arketypegjenkjenning

Klassifiser stillingen i én av de 6 arketypene (se `_shared.md`). Hvis den er hybrid, angi de 2 nærmeste. Dette bestemmer:
- Hvilke bevisstykker som prioriteres i blokk B
- Hvordan sammendraget omskrives i blokk E
- Hvilke STAR-historier som forberedes i blokk F

## Blokk A — Rolleoversikt

Tabell med:
- Gjenkjent arketype
- Domene (platform/agentisk/LLMOps/ML/enterprise)
- Funksjon (bygge/konsultere/lede/deploye)
- Ansiennitetsnivå
- Remote (full/hybrid/på kontoret)
- Teamstørrelse (om nevnt)
- TL;DR i 1 setning

## Blokk B — Match med CV

Les `cv.md`. Lag tabell der hvert krav fra JD-en er koblet til eksakte linjer i CV-en.

**Tilpasset arketypen:**
- Hvis FDE → prioriter bevisstykker for rask leveranse og kundevendt arbeid
- Hvis SA → prioriter systemdesign og integrasjoner
- Hvis PM → prioriter produktoppdagelse og metrikker
- Hvis LLMOps → prioriter evals, observerbarhet, pipelines
- Hvis Agentisk → prioriter multi-agent, HITL, orkestrering
- Hvis Transformasjon → prioriter endringsledelse, adopsjon, skalering

Seksjon med **mangler** med mitigeringsstrategi for hver. For hver mangel:
1. Er det en hard blokkering eller et pluss?
2. Kan kandidaten demonstrere tilgrensende erfaring?
3. Finnes det et porteføljeprosjekt som dekker denne mangelen?
4. Konkret mitigeringsplan (frase for søknadsbrev, hurtigprosjekt, osv.)

## Blokk C — Nivå og strategi

1. **Gjenkjent nivå** i JD-en vs **kandidatens naturlige nivå for den arketypen**
2. **Plan "selge senior uten å lyve"**: spesifikke fraser tilpasset arketypen, konkrete prestasjoner å fremheve, hvordan posisjonere gründererfaring som fordel
3. **Plan "hvis jeg nedgraderes"**: aksepter hvis kompensasjon er rettferdig, forhandl evaluering etter 6 måneder, tydelige forfremmingskriterier

## Blokk D — Kompensasjon og etterspørsel

Bruk WebSearch for:
- Nåværende lønninger for rollen (Glassdoor, Levels.fyi, Blind, Kode24, Tekna)
- Selskapets kompensasjonsrykte
- Etterspørselstrend for rollen

**Norskspesifikke kompensasjonsfaktorer:**
- Sjekk OTP-sats (2% vs 5-7% utgjør stor forskjell)
- Feriepenger (10,2% / 12%)
- Bonusordninger og aksjeprogram
- Forsikringspakke (helse, uføre, reise)
- Fleksibilitet og hjemmekontorordning

Tabell med data og oppgitte kilder. Hvis det ikke finnes data, si det i stedet for å finne på.

## Blokk E — Personaliseringsplan

| # | Seksjon | Nåværende tilstand | Foreslått endring | Hvorfor |
|---|---------|-------------------|-------------------|---------|
| 1 | Sammendrag | ... | ... | ... |
| ... | ... | ... | ... | ... |

Topp 5 endringer i CV + Topp 5 endringer på LinkedIn for å maksimere match.

## Blokk F — Intervjuplan

6-10 STAR+R-historier koblet til krav i JD-en (STAR + **Refleksjon**):

| # | Krav fra JD | STAR+R-historie | S | T | A | R | Refleksjon |
|---|-------------|-----------------|---|---|---|---|------------|

**Refleksjon**-kolonnen fanger opp hva som ble lært eller hva som ville blitt gjort annerledes. Dette signaliserer senioritet -- juniorer beskriver hva som skjedde, seniorer trekker ut lærdommer.

**Historiebank:** Hvis `interview-prep/story-bank.md` finnes, sjekk om noen av disse historiene allerede er der. Hvis ikke, legg til nye. Over tid bygger dette en gjenbrukbar bank med 5-10 mesterhistorier som kan tilpasses ethvert intervjuspørsmål.

**Valgt og innrammet etter arketypen:**
- FDE → vektlegg leveransehastighet og kundevendt arbeid
- SA → vektlegg arkitekturbeslutninger
- PM → vektlegg oppdagelse og avveininger
- LLMOps → vektlegg metrikker, evals, produksjonsherdning
- Agentisk → vektlegg orkestrering, feilhåndtering, HITL
- Transformasjon → vektlegg adopsjon, organisasjonsendring

Inkluder også:
- 1 anbefalt case study (hvilket prosjekt å presentere og hvordan)
- Røde flagg-spørsmål og hvordan svare på dem (f.eks. "hvorfor solgte du selskapet?", "har du ledelseserfaring?")

---

## Etter evaluering

**ALLTID** etter å ha generert blokkene A-F:

### 1. Lagre rapport .md

Lagre komplett evaluering i `reports/{###}-{selskap-slug}-{YYYY-MM-DD}.md`.

- `{###}` = neste sekvensielle nummer (3 siffer, null-padded)
- `{selskap-slug}` = selskapsnavn i lowercase, uten mellomrom (bruk bindestreker)
- `{YYYY-MM-DD}` = dagens dato

**Rapportformat:**

```markdown
# Evaluering: {Selskap} — {Rolle}

**Dato:** {YYYY-MM-DD}
**Arketype:** {gjenkjent}
**Score:** {X/5}
**URL:** {stillings-URL}
**PDF:** {sti eller ventende}

---

## A) Rolleoversikt
(komplett innhold fra blokk A)

## B) Match med CV
(komplett innhold fra blokk B)

## C) Nivå og strategi
(komplett innhold fra blokk C)

## D) Kompensasjon og etterspørsel
(komplett innhold fra blokk D)

## E) Personaliseringsplan
(komplett innhold fra blokk E)

## F) Intervjuplan
(komplett innhold fra blokk F)

## G) Utkast til søknadssvar
(bare hvis score >= 4.5 — utkast til svar for søknadsskjemaet)

---

## Nøkkelord hentet
(liste med 15-20 nøkkelord fra JD-en for ATS-optimalisering)
```

### 2. Registrer i tracker

**ALLTID** registrer i `data/applications.md`:
- Neste sekvensielle nummer
- Dagens dato
- Selskap
- Rolle
- Score: gjennomsnitt av match (1-5)
- Status: `Evaluert`
- PDF: ❌ (eller ✅ hvis auto-pipeline genererte PDF)
- Rapport: relativ lenke til rapport .md (f.eks. `[001](reports/001-selskap-2026-01-01.md)`)

**Tracker-format:**

```markdown
| # | Dato | Selskap | Rolle | Score | Status | PDF | Rapport |
```
