---
name: career-ops
description: AI job search command center -- evaluate offers, generate CVs, scan portals, track applications
user_invocable: true
args: mode
---

# career-ops -- Router

## Mode Routing

Determine the mode from `{{mode}}`:

| Input | Mode | Mode file |
|-------|------|-----------|
| (empty / no args) | `discovery` -- Show command menu | — |
| JD text or URL (no sub-command) | **`auto-pipeline`** | `modes/nb/auto-pipeline.md` |
| `oferta` / `tilbud` | `tilbud` | `modes/nb/tilbud.md` |
| `ofertas` / `sammenlign` | `tilbud-sammenligning` | `modes/nb/tilbud-sammenligning.md` |
| `contacto` / `kontakt` | `kontakt` | `modes/nb/kontakt.md` |
| `deep` / `dybde` | `dybde` | `modes/nb/dybde.md` |
| `pdf` | `pdf` | `modes/nb/pdf.md` |
| `training` / `opplaering` | `opplaering` | `modes/nb/opplaering.md` |
| `project` / `prosjekt` | `prosjekt` | `modes/nb/prosjekt.md` |
| `tracker` | `tracker` | `modes/nb/tracker.md` |
| `pipeline` | `pipeline` | `modes/nb/pipeline.md` |
| `apply` / `soknad` | `soknad` | `modes/nb/soknad.md` |
| `scan` / `skann` | `skann` | `modes/nb/skann.md` |
| `batch` | `batch` | `modes/nb/batch.md` |

**Auto-pipeline-gjenkjenning:** Hvis `{{mode}}` ikke er en kjent underkommando OG inneholder JD-tekst (nøkkelord: "responsibilities", "requirements", "qualifications", "about the role", "we're looking for", "ansvarsområder", "kvalifikasjoner", "vi ser etter", "om stillingen", selskapsnavn + rolle) eller en URL til en JD, kjør `auto-pipeline`.

Hvis `{{mode}}` ikke er en underkommando OG ikke ser ut som en JD, vis discovery.

---

## Discovery Mode (ingen argumenter)

Vis denne menyen:

```
career-ops -- Kommandosenter (Norsk Bokmål)

Tilgjengelige kommandoer:
  /career-ops {JD}          → AUTO-PIPELINE: evaluer + rapport + PDF + tracker (lim inn tekst eller URL)
  /career-ops pipeline      → Prosesser ventende URL-er fra innboks (data/pipeline.md)
  /career-ops tilbud        → Kun evaluering A-F (uten auto-PDF)
  /career-ops sammenlign    → Sammenlign og ranger flere tilbud
  /career-ops kontakt       → LinkedIn-trekk: finn kontakter + skriv melding
  /career-ops dybde         → Dybdeundersøkelse om selskap
  /career-ops pdf           → Kun PDF, ATS-optimalisert CV
  /career-ops opplaering    → Evaluer kurs/sertifisering mot Nordstjerne
  /career-ops prosjekt      → Evaluer porteføljeprosjekt-idé
  /career-ops tracker       → Søknadsoversikt
  /career-ops soknad        → Søknadsassistent i sanntid (leser skjema + genererer svar)
  /career-ops skann         → Skann portaler og oppdag nye stillinger
  /career-ops batch         → Masseprosessering med parallelle workers

Innboks: legg til URL-er i data/pipeline.md → /career-ops pipeline
Eller lim inn en JD direkte for å kjøre hele pipelinen.
```

---

## Context Loading by Mode

After determining the mode, load the necessary files before executing:

### Moduser som krever `_shared.md` + sin modusfil:
Les `modes/nb/_shared.md` + `modes/nb/{modus}.md`

Gjelder: `auto-pipeline`, `tilbud`, `tilbud-sammenligning`, `pdf`, `kontakt`, `soknad`, `pipeline`, `skann`, `batch`

### Frittstående moduser (kun sin modusfil):
Les `modes/nb/{modus}.md`

Gjelder: `tracker`, `dybde`, `opplaering`, `prosjekt`

### Moduser delegert til subagent:
For `skann`, `soknad` (med Playwright), og `pipeline` (3+ URL-er): start som Agent med innholdet av `_shared.md` + `modes/nb/{modus}.md` injisert i subagent-promptet.

```
Agent(
  subagent_type="general-purpose",
  prompt="[innhold fra modes/nb/_shared.md]\n\n[innhold fra modes/nb/{modus}.md]\n\n[invokasjonsspesifikke data]",
  description="career-ops {modus}"
)
```

Kjør instruksjonene fra den lastede modusfilen.
