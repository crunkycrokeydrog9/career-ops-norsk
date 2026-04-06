# Modus: dybde — Dybdeundersøkelse

Genererer et strukturert prompt for Perplexity/Claude/ChatGPT med 6 akser:

```
## Dybdeundersøkelse: [Selskap] — [Rolle]

Kontekst: Jeg evaluerer en kandidatur for [rolle] hos [selskap]. Jeg trenger handlingsrettet informasjon til intervjuet.

### 1. AI-strategi
- Hvilke produkter/features bruker AI/ML?
- Hva er deres AI-stack? (modeller, infra, verktøy)
- Har de en engineering-blogg? Hva publiserer de?
- Hvilke papers eller foredrag har de gitt om AI?

### 2. Nylige trekk (siste 6 måneder)
- Relevante ansettelser innen AI/ML/produkt?
- Oppkjøp eller partnerskap?
- Produktlanseringer eller pivoter?
- Finansieringsrunder eller lederskapsendringer?

### 3. Engineering-kultur
- Hvordan shipper de? (deploy-kadanse, CI/CD)
- Mono-repo eller multi-repo?
- Hvilke språk/rammeverk bruker de?
- Remote-first eller kontor-first?
- Glassdoor/Blind-anmeldelser om eng-kulturen?

### 4. Sannsynlige utfordringer
- Hvilke skaleringsproblemer har de?
- Pålitelighet-, kostnads-, latency-utfordringer?
- Migrerer de noe? (infra, modeller, plattformer)
- Hvilke smertepunkter nevner folk i anmeldelser?

### 5. Konkurrenter og differensiering
- Hvem er hovedkonkurrentene?
- Hva er deres moat/differensiator?
- Hvordan posisjonerer de seg vs konkurransen?

### 6. Kandidatens vinkel
Gitt min profil (les fra cv.md og profile.yml for spesifikk erfaring):
- Hvilken unik verdi tilfører jeg dette teamet?
- Hvilke av mine prosjekter er mest relevante?
- Hvilken historie bør jeg fortelle i intervjuet?
```

Personaliser hver seksjon med den spesifikke konteksten fra den evaluerte stillingen.
