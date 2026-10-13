# Kjenner'n

Et nettspill: du får en artist og skal nevne så mange låter du kan. Låter godkjennes i det
øyeblikket de matcher. Norsk først, mobil først, ingen app.

- Produktet: `docs/konsept.md`
- Aktiv spec: se `specs/README.md` (jobb kun innenfor aktiv spec)
- Beslutninger: `docs/adr/`

## Om utvikleren

Markus er rusten i TypeScript/React og bruker prosjektet til å lære seg det på nytt.
Prosjektet er like mye et læringsløp som et produkt. Han har sterk testbakgrunn
(Playwright, TDD), litt Docker og Kubernetes.

## Læringsmodus: slik jobber du med meg

1. **Kjernelogikk skriver jeg selv.** Spillflyt, matching, state og komponenter: ikke skriv
   ferdig løsning med mindre jeg skriver «fasit».
2. **Før en oppgave:** forklar konseptet kort (3–6 setninger), lenk til offisiell
   dokumentasjon (react.dev, nextjs.org/docs, typescriptlang.org, vitest.dev), og gi meg en
   avgrenset oppgave.
3. **Når jeg står fast, hint i trinn:** (1) retning, (2) hvilket konsept/API jeg bør se på,
   (3) kodeskisse med hull. Full løsning bare når jeg ber om det.
4. **Når jeg har skrevet kode:** gjennomgå den. Si hva som er bra, hva som kan bli bedre og
   *hvorfor*. Foreslå endringer, ikke overskriv koden min.
5. **Boilerplate og konfigurasjon** (oppsett, config-filer, scaffolding) kan du skrive, men
   forklar kort hva hver del gjør.
6. **Forklar hvorfor, ikke bare hva.** Bruk eksempler fra vår egen kode.
7. **Små, ferdige steg.** Én ting om gangen. Hver økt skal ende med noe som fungerer.
8. **Tester først der det passer.** Ren logikk i `src/lib/` skrives med TDD i Vitest.

**Spør meg før du:** legger til avhengigheter, endrer datamodellen, tar arkitekturvalg,
committer eller pusher. Jeg tar beslutningene; du foreslår alternativer med trade-offs.
Større beslutninger skrives som ADR i `docs/adr/`.

## Språk

Snakk norsk med meg. Kode, navn, kommentarer og commit-meldinger skrives på engelsk.
Commit-meldinger følger Conventional Commits (`feat:`, `fix:`, `test:`, `docs:`, `chore:`).

## Stack

- **web/**: Next.js (App Router), React, TypeScript (`strict`), Tailwind CSS
- **Tester:** Vitest for logikk, Playwright for ende-til-ende (senere)
- **Data og brukere:** Supabase (Postgres, auth), fra spec 002/006
- **pipeline/** (senere): Python-synk fra MusicBrainz (maks 1 forespørsel/sek) og Last.fm
- **Lydklipp:** Deezer (30 sek forhåndslytt), hentes ved behov, lagres aldri
- **Hosting:** Vercel. Node 24 (se `.node-version`), npm

## Arkitekturprinsipper

- **Generell datamodell:** `category → subject → items`. Musikk er første kategori
  (category = music, subject = artist, item = song). Ikke hardkod «song» i kjernelogikken.
- **Ren logikk uten React:** normalisering, matching og scoring ligger i `web/src/lib/game/`
  som rene funksjoner med tester. Da kan de flyttes til serveren uendret.
- **Serveren er fasit:** når vi får backend, sjekkes svar og tid på serveren, aldri bare i
  nettleseren.
- **Spillet kaller aldri eksterne API-er under en runde**, unntatt lydklipp. Data kommer fra
  vår egen database (eller lokal JSON i spec 001).
- **Mobil først:** test alltid på smal skjerm.

## Mappestruktur

```
CLAUDE.md          dette dokumentet
docs/              konsept, ADR-er, læringslogg
specs/             én spec per leveranse, nummerert
web/               Next.js-appen
  src/app/         sider og ruter
  src/lib/game/    ren spillogikk (normalize, match, score) + tester
  src/data/        lokal artistdata (spec 001)
pipeline/          Python-datasynk (senere)
```

## Kommandoer

Fylles inn når `web/` er satt opp (`npm run dev`, `npm test`, `npm run lint`).
