# Spec 001: Solo-runde med lokal data

**Status:** Aktiv
**Mål:** Kjerneløkka fungerer ende til ende: velg artist, skriv låter, se dem godkjennes,
avslutt og se fasit. Ingen API, ingen database, ingen konto.

## Brukerhistorie

Som spiller vil jeg trykke *Spill*, få en artist og skrive inn låter jeg kan, slik at jeg
ser hvor mange jeg husket og hvilke store låter jeg glemte.

## Flyt

1. **Landingsside (`/`)**: tittelen «Kjenner'n» og én knapp: *Spill*.
2. **Velg artist (`/spill`)**: *Tilfeldig artist* eller en liste/søk over artistene i datafila.
3. **Spillskjerm (`/spill/[subjectId]`)**:
   - Artistnavn øverst, antall funnet (f.eks. «7 låter»)
   - Inputfelt midt på skjermen, alltid i fokus
   - Når input matcher en låt: låten legges i listen, feltet tømmes. Ingen Enter.
   - Listen viser alle funnede låter, nyeste øverst og tydelig fremhevet
   - Knapp: *Jeg er ferdig*
4. **Resultat** (på samme side): antall funnet av totalt, og de 5 høyest rangerte låtene
   spilleren ikke kom på. Knapp: *Spill igjen*.

## Data (lokal JSON)

Fil: `web/src/data/music.json`. 5–10 artister, 10–25 låter hver. Formatet følger den
generelle modellen `category → subject → items`:

```json
{
  "category": "music",
  "subjects": [
    {
      "id": "toto",
      "name": "Toto",
      "items": [
        { "id": "africa", "title": "Africa", "aliases": [], "rank": 1 },
        { "id": "hold-the-line", "title": "Hold the Line", "aliases": [], "rank": 2 }
      ]
    }
  ]
}
```

`rank` = popularitet innen artisten (1 er størst). `aliases` = andre skrivemåter folk bruker.

## Normalisering: `normalizeTitle(text: string): string`

Både spillerens input og låttitler normaliseres før de sammenlignes. Regler:

1. Små bokstaver, trim, flere mellomrom blir ett
2. Fjern innhold i parentes og klammer: `(Mono)`, `[Live]`
3. Fjern versjonshaler etter ` - ` når de inneholder remaster, live, edit, version, mono,
   stereo eller mix
4. `&` blir `and`
5. Fjern aksenter (`é` → `e`), men **behold æ, ø og å**
6. Fjern tegnsetting, også apostrofer

| Input | Forventet |
| --- | --- |
| `Paint It, Black` | `paint it black` |
| `Paint It Black (Mono)` | `paint it black` |
| `Bohemian Rhapsody - Remastered 2011` | `bohemian rhapsody` |
| `Don't Stop Believin'` | `dont stop believin` |
| `  hold   the   line ` | `hold the line` |
| `Café Del Mar` | `cafe del mar` |
| `Kjærlighet på pinne` | `kjærlighet på pinne` |
| `Rock & Roll` | `rock and roll` |
| `Hey Jude [Live]` | `hey jude` |

## Matching: `matchItem(input, items, foundIds)`

- Returnerer låten hvis normalisert input er **lik** normalisert tittel eller et alias
- Returnerer `null` hvis låten allerede er funnet, eller ingen treff
- **Kort-tittel-regelen:** hvis input matcher en tittel, men også er starten på en annen
  ikke-funnet tittel («help» vs «help me»), vent til spilleren har sluttet å skrive i
  600 ms før låten godkjennes
- Skrivefeil-toleranse (fuzzy) er **ikke** med i 001

## Utenfor scope

API-er, lydklipp, hint, tidtaker, konto, database, design utover det enkle.

## Oppgaver og læringsmål

| # | Oppgave | Hvem | Læringsmål |
| --- | --- | --- | --- |
| 1 | Sett opp `web/` med Next.js, TypeScript, Tailwind og Vitest | Claude, forklarer | Prosjektstruktur, App Router, `package.json`-scripts |
| 2 | Typer: `Category`, `Subject`, `Item` i `lib/game/types.ts` | Markus | TypeScript-typer og interfaces |
| 3 | `normalizeTitle` med TDD fra tabellen over | Markus | Rene funksjoner, string-metoder, regex, Vitest |
| 4 | `matchItem` med TDD | Markus | Arrays (`find`, `some`), `Set`, edge cases |
| 5 | `music.json` med 5–10 artister | Claude lager, Markus kontrollerer | JSON-import, datakvalitet |
| 6 | Landingsside og artistvalg | Markus | Komponenter, `Link`, ruter |
| 7 | Spillskjerm: input, state, liste | Markus | `useState`, kontrollerte felt, lister og `key`, `useEffect` for debounce |
| 8 | Resultat med fasit | Markus | Avledet state, `filter`/`sort`/`slice` |
| 9 | Test på mobil, commit og push | Markus | Responsivt oppsett, Git-flyt |

## Akseptansekriterier

- [ ] Fra `/` til første godkjente låt på under 10 sekunder
- [ ] Alle eksemplene i normaliseringstabellen har en grønn test
- [ ] Samme låt kan ikke telle to ganger
- [ ] «Help» godkjennes ikke før spilleren har tatt pause, når «Help Me» også finnes
- [ ] Nyeste låt er tydelig fremhevet i listen
- [ ] Resultatet viser de 5 største låtene spilleren ikke kom på
- [ ] Inputfeltet har `autoCapitalize="off"`, `autoCorrect="off"` og `spellCheck={false}`
- [ ] Fungerer på mobil (smal skjerm) og PC
- [ ] `npm test` og `npm run lint` er grønne

## Åpne spørsmål

- Skal en runde uten tidtaker kunne stå åpen og fortsettes senere?
- Skal funnede låter sorteres nyeste øverst, eller i den rekkefølgen de ble funnet?
