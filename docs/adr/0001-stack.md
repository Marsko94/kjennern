# ADR 0001: TypeScript for appen, Python for datapipeline

**Dato:** 2026-10-13
**Status:** Vedtatt

## Kontekst

Kjenner'n er både et produkt og et læringsprosjekt (frontend, DevOps, senere MLOps).
Vi vurderte Go som backend-språk.

## Beslutning

- Appen (frontend og spill-API) bygges i TypeScript med Next.js og Supabase.
- Datapipelinen (MusicBrainz, Last.fm, deduplisering, popularitet) skrives i Python.
- Go brukes ikke i Kjenner'n.

## Begrunnelse

- Ingenting i spillet krever en egen backend-tjeneste; Next.js og Supabase dekker det.
- Python er standard for dataarbeid, automatisering og MLOps, som er der karriereløpet går.
- Ett språk i appen gjør det lettere å lære TypeScript og React skikkelig.

## Konsekvenser

- Live-multiplayer bygges med Supabase Realtime, ikke en egen server.
- Trenger vi en egen tjeneste senere, tas det opp i en ny ADR.
