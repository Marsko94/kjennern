# Kjenner'n – konsept

Fullt konseptdokument (levende): https://claude.ai/code/artifact/592aec58-24a8-465b-a2b8-bfc293f1e413
Denne filen er kortversjonen som kodeverktøyene leser.

## Kjernen

Du får en artist. Du skriver inn så mange låter du kommer på. Hver låt godkjennes i det
øyeblikket den matcher, uten Enter, og legges i en liste der den siste er fremhevet. Når du
er ferdig, ser du fasiten: de mest populære låtene du ikke kom på.

## Designprinsipper

1. **Kjernen først:** nevn flest mulig av noe. Alt annet er pynt.
2. **Rolig som standard:** god tid, som i Wordle eller sjakk. Tidspress er en egen modus.
3. **Start på ett klikk:** fra landingsside til første låt uten innlogging.
4. **Nettspill, mobil først.** Ingen app.
5. **Respekter spilleren:** ingen spam-e-post, push-varsler eller popup-annonser.
6. **Konto er frivillig:** for å utfordre venner eller samle badges.

## MVP (det vi bygger først)

- Landingsside: stor tittel, én knapp: *Spill*
- Velg artist: *Tilfeldig* eller *Velg artist*
- Spillskjerm: inputfelt, automatisk matching, liste over funnede låter
- *Jeg er ferdig* → resultat med fasit
- Deretter: ekte data fra API, lydklipp-hint og andre hint

## Senere (arkitekturen skal tåle det)

Tidskontroller (lyn/hurtig/lang), Dagens Kjenner'n med toppliste, utfordre en venn
(asynkront, Quizkampen-stil), badges, kontoer, live-multiplayer, kategorier (tiår, sjanger,
norske hits), andre temaer (sport, bøker), medlemskap og annonser bak feature flags.

## Hint (senere)

Livlinjer som peker på den mest populære låten spilleren ikke har nevnt: 30 sek lydklipp,
forbokstav, ordmønster (`_ _ _ _  _ _`), utgivelsesår, kort fakta. Aldri sitater fra
sangtekster (opphavsrett).
