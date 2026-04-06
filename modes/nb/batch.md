# Modus: batch — Masseprosessering av stillinger

To bruksmåter: **conductor --chrome** (navigerer portaler i sanntid) eller **standalone** (script for allerede innsamlede URL-er).

## Arkitektur

```
Claude Conductor (claude --chrome --dangerously-skip-permissions)
  │
  │  Chrome: navigerer portaler (innloggede økter)
  │  Leser DOM direkte — brukeren ser alt i sanntid
  │
  ├─ Stilling 1: les JD fra DOM + URL
  │    └─► claude -p worker → rapport .md + PDF + tracker-linje
  │
  ├─ Stilling 2: klikk neste, les JD + URL
  │    └─► claude -p worker → rapport .md + PDF + tracker-linje
  │
  └─ Slutt: merge tracker-additions → applications.md + oppsummering
```

Hver worker er en `claude -p`-prosess med ren kontekst på 200K tokens. Conductoren bare orkestrerer.

## Filer

```
batch/
  batch-input.tsv               # URL-er (fra conductor eller manuelt)
  batch-state.tsv               # Fremdrift (autogenerert, gitignored)
  batch-runner.sh               # Orkestreringsskript standalone
  batch-prompt.md               # Promptmal for workers
  logs/                         # Én logg per stilling (gitignored)
  tracker-additions/            # Tracker-linjer (gitignored)
```

## Modus A: Conductor --chrome

1. **Les tilstand**: `batch/batch-state.tsv` → se hva som allerede er prosessert
2. **Naviger portal**: Chrome → søke-URL
3. **Hent URL-er**: Les DOM av resultater → hent ut URL-liste → append til `batch-input.tsv`
4. **For hver ventende URL**:
   a. Chrome: klikk på stillingen → les JD-tekst fra DOM
   b. Lagre JD til `/tmp/batch-jd-{id}.txt`
   c. Beregn neste sekvensielle RAPPORT_NR
   d. Kjør via Bash:
      ```bash
      claude -p --dangerously-skip-permissions \
        --append-system-prompt-file batch/batch-prompt.md \
        "Prosesser denne stillingen. URL: {url}. JD: /tmp/batch-jd-{id}.txt. Rapport: {num}. ID: {id}"
      ```
   e. Oppdater `batch-state.tsv` (completed/failed + score + report_num)
   f. Logg til `logs/{report_num}-{id}.log`
   g. Chrome: gå tilbake → neste stilling
5. **Paginering**: Hvis ingen flere stillinger → klikk "Neste" → gjenta
6. **Slutt**: Merge `tracker-additions/` → `applications.md` + oppsummering

## Modus B: Standalone script

```bash
batch/batch-runner.sh [ALTERNATIVER]
```

Alternativer:
- `--dry-run` — list ventende uten å kjøre
- `--retry-failed` — bare prøv feilede på nytt
- `--start-from N` — start fra ID N
- `--parallel N` — N workers parallelt
- `--max-retries N` — forsøk per stilling (default: 2)

## Format batch-state.tsv

```
id	url	status	started_at	completed_at	report_num	score	error	retries
1	https://...	completed	2026-...	2026-...	002	4.2	-	0
2	https://...	failed	2026-...	2026-...	-	-	Feilmelding	1
3	https://...	pending	-	-	-	-	-	0
```

## Gjenopptakelighet

- Hvis den dør → kjør på nytt → leser `batch-state.tsv` → hopper over fullførte
- Låsefil (`batch-runner.pid`) hindrer dobbel kjøring
- Hver worker er uavhengig: feil i stilling #47 påvirker ikke de andre

## Workers (claude -p)

Hver worker mottar `batch-prompt.md` som systemprompt. Den er selvforsynt.

Workeren produserer:
1. Rapport `.md` i `reports/`
2. PDF i `output/`
3. Tracker-linje i `batch/tracker-additions/{id}.tsv`
4. JSON-resultat via stdout

## Feilhåndtering

| Feil | Gjenoppretting |
|------|----------------|
| URL utilgjengelig | Worker feiler → conductor merker `failed`, neste |
| JD bak innlogging | Conductor prøver å lese DOM. Hvis feil → `failed` |
| Portal endrer layout | Conductor resonnerer over HTML, tilpasser seg |
| Worker krasjer | Conductor merker `failed`, neste. Retry med `--retry-failed` |
| Conductor dør | Kjør på nytt → leser tilstand → hopper over fullførte |
| PDF feiler | Rapport .md lagres. PDF forblir ventende |
