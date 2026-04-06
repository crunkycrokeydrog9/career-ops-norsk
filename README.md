# Career-Ops

**[:gb: English](#what-is-this)** | **[:norway: Norsk](#no-norsk-versjon)**

> AI-powered job search pipeline built on Claude Code. Evaluate offers, generate tailored CVs, scan portals, and track everything -- powered by AI agents.

![Claude Code](https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

---

<p align="center">
  <img src="docs/demo.gif" alt="Career-Ops Demo" width="800">
</p>

## What Is This

Career-Ops turns Claude Code into a full job search command center. Instead of manually tracking applications in a spreadsheet, you get an AI-powered pipeline that:

- **Evaluates offers** with a structured A-F scoring system (10 weighted dimensions)
- **Generates tailored PDFs** -- ATS-optimized CVs customized per job description
- **Scans portals** automatically (Greenhouse, Ashby, Lever, company pages)
- **Processes in batch** -- evaluate 10+ offers in parallel with sub-agents
- **Tracks everything** in a single source of truth with integrity checks

> **Important: This is NOT a spray-and-pray tool.** Career-ops is a filter -- it helps you find the few offers worth your time out of hundreds. The system strongly recommends against applying to anything scoring below 4.0/5. Your time is valuable, and so is the recruiter's. Always review before submitting.

Career-ops is agentic: Claude Code navigates career pages with Playwright, evaluates fit by reasoning about your CV vs the job description (not keyword matching), and adapts your resume per listing.

> **Heads up: the first evaluations won't be great.** The system doesn't know you yet. Feed it context -- your CV, your career story, your proof points, your preferences, what you're good at, what you want to avoid. The more you nurture it, the better it gets. Think of it as onboarding a new recruiter: the first week they need to learn about you, then they become invaluable.

Built by someone who used it to evaluate 740+ job offers, generate 100+ tailored CVs, and land a Head of Applied AI role. [Read the full case study](https://santifer.io/career-ops-system).

## Features

| Feature | Description |
|---------|-------------|
| **Auto-Pipeline** | Paste a URL, get a full evaluation + PDF + tracker entry |
| **6-Block Evaluation** | Role summary, CV match, level strategy, comp research, personalization, interview prep (STAR+R) |
| **Interview Story Bank** | Accumulates STAR+Reflection stories across evaluations -- 5-10 master stories that answer any behavioral question |
| **Negotiation Scripts** | Salary negotiation frameworks, geographic discount pushback, competing offer leverage |
| **ATS PDF Generation** | Keyword-injected CVs with Space Grotesk + DM Sans design |
| **Portal Scanner** | 45+ companies pre-configured (Anthropic, OpenAI, ElevenLabs, Retool, n8n...) + custom queries across Ashby, Greenhouse, Lever, Wellfound |
| **Batch Processing** | Parallel evaluation with `claude -p` workers |
| **Dashboard TUI** | Terminal UI to browse, filter, and sort your pipeline |
| **Human-in-the-Loop** | AI evaluates and recommends, you decide and act. The system never submits an application -- you always have the final call |
| **Pipeline Integrity** | Automated merge, dedup, status normalization, health checks |

## Quick Start

```bash
# 1. Clone and install
git clone https://github.com/santifer/career-ops.git
cd career-ops && npm install
npx playwright install chromium   # Required for PDF generation

# 2. Configure
cp config/profile.example.yml config/profile.yml  # Edit with your details
cp templates/portals.example.yml portals.yml       # Customize companies

# 3. Add your CV
# Create cv.md in the project root with your CV in markdown

# 4. Personalize with Claude
claude   # Open Claude Code in this directory

# Then ask Claude to adapt the system to you:
# "Change the archetypes to backend engineering roles"
# "Translate the modes to English"
# "Add these 5 companies to portals.yml"
# "Update my profile with this CV I'm pasting"

# 5. Start using
# Paste a job URL or run /career-ops
```

> **The system is designed to be customized by Claude itself.** Modes, archetypes, scoring weights, negotiation scripts -- just ask Claude to change them. It reads the same files it uses, so it knows exactly what to edit.

See [docs/SETUP.md](docs/SETUP.md) for the full setup guide.

## Usage

Career-ops is a single slash command with multiple modes:

```
/career-ops                → Show all available commands
/career-ops {paste a JD}   → Full auto-pipeline (evaluate + PDF + tracker)
/career-ops scan           → Scan portals for new offers
/career-ops pdf            → Generate ATS-optimized CV
/career-ops batch          → Batch evaluate multiple offers
/career-ops tracker        → View application status
/career-ops apply          → Fill application forms with AI
/career-ops pipeline       → Process pending URLs
/career-ops contacto       → LinkedIn outreach message
/career-ops deep           → Deep company research
/career-ops training       → Evaluate a course/cert
/career-ops project        → Evaluate a portfolio project
```

Or just paste a job URL or description directly -- career-ops auto-detects it and runs the full pipeline.

## How It Works

```
You paste a job URL or description
        │
        ▼
┌──────────────────┐
│  Archetype       │  Classifies: LLMOps / Agentic / PM / SA / FDE / Transformation
│  Detection       │
└────────┬─────────┘
         │
┌────────▼─────────┐
│  A-F Evaluation   │  Match, gaps, comp research, STAR stories
│  (reads cv.md)    │
└────────┬─────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
 Report  PDF  Tracker
  .md   .pdf   .tsv
```

## Pre-configured Portals

The scanner comes with **45+ companies** ready to scan and **19 search queries** across major job boards. Copy `templates/portals.example.yml` to `portals.yml` and add your own:

**AI Labs:** Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
**Voice AI:** ElevenLabs, PolyAI, Parloa, Hume AI, Deepgram, Vapi, Bland AI
**AI Platforms:** Retool, Airtable, Vercel, Temporal, Glean, Arize AI
**Contact Center:** Ada, LivePerson, Sierra, Decagon, Talkdesk, Genesys
**Enterprise:** Salesforce, Twilio, Gong, Dialpad
**LLMOps:** Langfuse, Weights & Biases, Lindy, Cognigy, Speechmatics
**Automation:** n8n, Zapier, Make.com
**European:** Factorial, Attio, Tinybird, Clarity AI, Travelperk

**Job boards searched:** Ashby, Greenhouse, Lever, Wellfound, Workable, RemoteFront

## Dashboard TUI

The built-in terminal dashboard lets you browse your pipeline visually:

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard
```

Features: 6 filter tabs, 4 sort modes, grouped/flat view, lazy-loaded previews, inline status changes.

## Project Structure

```
career-ops/
├── CLAUDE.md                    # Agent instructions
├── cv.md                        # Your CV (create this)
├── article-digest.md            # Your proof points (optional)
├── config/
│   └── profile.example.yml      # Template for your profile
├── modes/                       # 14 skill modes
│   ├── _shared.md               # Shared context (customize this)
│   ├── oferta.md                # Single evaluation
│   ├── pdf.md                   # PDF generation
│   ├── scan.md                  # Portal scanner
│   ├── batch.md                 # Batch processing
│   └── ...
├── templates/
│   ├── cv-template.html         # ATS-optimized CV template
│   ├── portals.example.yml      # Scanner config template
│   └── states.yml               # Canonical statuses
├── batch/
│   ├── batch-prompt.md          # Self-contained worker prompt
│   └── batch-runner.sh          # Orchestrator script
├── dashboard/                   # Go TUI pipeline viewer
├── data/                        # Your tracking data (gitignored)
├── reports/                     # Evaluation reports (gitignored)
├── output/                      # Generated PDFs (gitignored)
├── fonts/                       # Space Grotesk + DM Sans
├── docs/                        # Setup, customization, architecture
└── examples/                    # Sample CV, report, proof points
```

## Tech Stack

![Claude Code](https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Bubble Tea](https://img.shields.io/badge/Bubble_Tea-FF75B5?style=flat&logo=go&logoColor=white)

- **Agent**: Claude Code with custom skills and modes
- **PDF**: Playwright/Puppeteer + HTML template
- **Scanner**: Playwright + Greenhouse API + WebSearch
- **Dashboard**: Go + Bubble Tea + Lipgloss (Catppuccin Mocha theme)
- **Data**: Markdown tables + YAML config + TSV batch files

## Also Open Source

- **[cv-santiago](https://github.com/santifer/cv-santiago)** -- The portfolio website (santifer.io) with AI chatbot, LLMOps dashboard, and case studies. If you need a portfolio to showcase alongside your job search, fork it and make it yours.

## About the Author

I'm Santiago -- Head of Applied AI, former founder (built and sold a business that still runs with my name on it). I built career-ops to manage my own job search. It worked: I used it to land my current role.

My portfolio and other open source projects → [santifer.io](https://santifer.io)

## License

MIT

---

# :norway: Norsk versjon

## Hva er dette

Career-Ops gjør Claude Code til et komplett kommandosenter for jobbsøk. I stedet for å spore søknader i et regneark, får du en AI-drevet pipeline som:

- **Evaluerer stillinger** med strukturert A-F-scoring (10 vektede dimensjoner)
- **Genererer skreddersydde PDF-er** — ATS-optimaliserte CV-er tilpasset hver stillingsannonse
- **Skanner portaler** automatisk (Greenhouse, Ashby, Lever, Finn.no, Arbeidsplassen, bedriftssider)
- **Prosesserer i batch** — evaluer 10+ stillinger parallelt med sub-agenter
- **Sporer alt** i én sannhetskilde med integritetskontroller

> **Viktig: Dette er IKKE et spray-and-pray-verktøy.** Career-ops er et filter — det hjelper deg finne de få stillingene som er verdt tiden din blant hundrevis. Systemet fraråder sterkt å søke på noe med score under 4.0/5. Tiden din er verdifull, og det er rekruttererens også. Gjennomgå alltid før du sender.

> **Obs: de første evalueringene blir ikke perfekte.** Systemet kjenner deg ikke ennå. Gi det kontekst — CV-en din, karrierehistorien din, bevisene dine, preferansene dine, hva du er god på, hva du vil unngå. Jo mer du gir, desto bedre filtrerer det. Tenk på det som onboarding av en ny rekrutterer: den første uken trenger de å lære om deg, deretter blir de uvurderlige.

Denne forken er oversatt til norsk bokmål med tilpasninger for det norske arbeidsmarkedet (Finn.no, NAV Arbeidsplassen, OTP, feriepenger, arbeidsmiljøloven, tariffavtaler). Original: [santifer/career-ops](https://github.com/santifer/career-ops). [Les hele case study-en](https://santifer.io/career-ops-system).

## Hurtigstart

```bash
# 1. Klone og installer
git clone https://github.com/crunkycrokeydrog9/career-ops-norsk.git
cd career-ops-norsk && npm install
npx playwright install chromium   # Påkrevd for PDF-generering

# 2. Konfigurer
cp config/profile.example.yml config/profile.yml  # Fyll inn dine detaljer
cp templates/portals.example.yml portals.yml       # Tilpass selskaper

# 3. Legg til CV-en din
# Opprett cv.md i prosjektroten med CV-en din i markdown

# 4. Tilpass med Claude
claude   # Åpne Claude Code i denne mappen

# Be Claude tilpasse systemet til deg:
# "Endre arketypene til backend-roller"
# "Legg til disse selskapene i portals.yml"
# "Oppdater profilen min med denne CV-en"

# 5. Begynn å bruke
# Lim inn en stillings-URL eller kjør /career-ops
```

> **Systemet er designet for å tilpasses av Claude selv.** Moduser, arketyper, scoring-vekter, forhandlingsscript — bare spør. Claude leser de samme filene den bruker, så den vet nøyaktig hva den skal redigere.

Komplett guide i [docs/SETUP.md](docs/SETUP.md).

## Inkluderte portaler

Skanneren kommer med **45+ selskaper** forhåndskonfigurert og **19 søkespørringer** på tvers av store jobbportaler, pluss norske portaler. Kopier `templates/portals.example.yml` til `portals.yml` og legg til dine egne:

**AI Labs:** Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
**Voice AI:** ElevenLabs, PolyAI, Parloa, Hume AI, Deepgram, Vapi, Bland AI
**AI-plattformer:** Retool, Airtable, Vercel, Temporal, Glean, Arize AI
**Contact Center:** Ada, LivePerson, Sierra, Decagon, Talkdesk, Genesys
**Enterprise:** Salesforce, Twilio, Gong, Dialpad
**LLMOps:** Langfuse, Weights & Biases, Lindy, Cognigy, Speechmatics
**Automatisering:** n8n, Zapier, Make.com
**Europa:** Factorial, Attio, Tinybird, Clarity AI, Travelperk
**Norge:** Finn.no, Arbeidsplassen (NAV), Kode24

**Jobbportaler:** Ashby, Greenhouse, Lever, Wellfound, Workable, RemoteFront

## Bruk

Career-ops er én slash-kommando med flere moduser:

```
/career-ops                    → Vis alle tilgjengelige kommandoer
/career-ops {lim inn en JD}    → Komplett auto-pipeline (evaluer + PDF + tracker)
/career-ops skann              → Skann portaler etter nye stillinger
/career-ops pdf                → Generer ATS-optimalisert CV
/career-ops batch              → Masseevaluer flere stillinger
/career-ops tracker            → Vis søknadsoversikt
/career-ops soknad             → Fyll ut søknadsskjemaer med AI
/career-ops pipeline           → Prosesser ventende URL-er
/career-ops kontakt            → LinkedIn-oppsøkingsmelding
/career-ops dybde              → Dybdeundersøkelse av selskap
/career-ops opplaering         → Evaluer kurs/sertifisering
/career-ops prosjekt           → Evaluer porteføljeprosjekt
```

Eller bare lim inn en stillings-URL eller -beskrivelse direkte — career-ops oppdager det automatisk og kjører hele pipelinen.

## Også open source

- **[cv-santiago](https://github.com/santifer/cv-santiago)** — Porteføljen (santifer.io) med AI-chatbot, LLMOps-dashboard og case-studier. Hvis du trenger en portefølje til jobbsøket ditt, fork den og gjør den til din.

## Dokumentasjon

- [SETUP.md](docs/SETUP.md) — Installasjonsguide
- [CUSTOMIZATION.md](docs/CUSTOMIZATION.md) — Slik tilpasser du
- [ARCHITECTURE.md](docs/ARCHITECTURE.md) — Slik fungerer systemet
