---
title: "Gang 3 — AI-drevet stadeholder-generator til Engestofte julemarked"
date: 2026-05-13
description: "Vi identificerede et konkret problem hos en reel bruger og skitserede en MVP der løser det med Claude API og en simpel formular."
tags: ["ai", "claude", "api", "webapp", "refleksion"]
series: ["AI-drevne applikationer"]
---

## Projektet

Denne gang tog vi fat i et konkret, virkeligt problem — ikke et tutorial-eksempel, men noget der faktisk irriterer et rigtigt menneske i det daglige arbejde.

**Arbejdstitlen:** AI-drevet stadeholder-generator til Engestofte julemarked.

Engestofte Gods afholder julemarked hvert år med en lang liste af stadeholdere. Koordinatoren Lise opdaterer listen på hjemmesiden manuelt — hun skriver HTML direkte i Squarespace. Det er tidskrævende, kræver teknisk viden hun ikke har, og det er nemt at lave fejl, særligt når der er mange stadeholdere.

---

## Problemet

Lises nuværende arbejdsgang ser sådan her ud:

1. Modtager stadeholderinfo via mail
2. Skriver eller retter HTML manuelt i Squarespace
3. Publicerer siden

Ingen standardiseret format, ingen historik, ingen genbrug af data fra tidligere år. Hver opdatering er en manuel operation der kræver at hun husker HTML-strukturen og ikke laver slåfejl i tags.

Det er præcis den slags opgave AI er god til: struktureret, gentagende, formatfølsom output fra varierende input.

---

## Den foreslåede løsning

En simpel webapp hvor Lise udfylder en formular med stadeholderinfo. Klikker hun på en knap, sender appen dataene til Claude (Anthropic API), som genererer færdig, korrekt formateret Squarespace-HTML — klar til copy-paste direkte ind på hjemmesiden.

Ingen kode. Ingen HTML. Ingen teknisk viden nødvendig.

**AI-funktionen:**
Claude modtager strukturerede data (navn, beskrivelse, hjemmeside, zone) og en system-prompt der specificerer præcis det Squarespace-format siden bruger — inkl. korrekte sektioner per zone (Laden, Kostalden, udendørs m.fl.). Output er færdig HTML der matcher den eksisterende sidestruktur.

---

## MVP

Formular med fire felter:

| Felt | Type |
|---|---|
| Navn | Tekstfelt |
| Beskrivelse | Tekstområde |
| Hjemmeside | URL-felt |
| Zone | Dropdown |

Knap → Claude API-kald → HTML vises i en tekstboks klar til copy-paste.

Ingen login. Ingen database. Bare input → AI → output.

---

## Hvad vi ikke bygger (endnu)

Vi afgrænser projektet bevidst. Version 1 inkluderer ikke:

- Direkte Squarespace-integration
- Login eller brugerstyring
- Database over historiske stadeholdere
- Plantegning over markedet
- Trello-integration

Det er version 2-opgaver. MVP skal bevise konceptet, ikke løse alt på én gang.

---

## Åbne spørgsmål

Der er tre ting vi endnu ikke har svar på fra kunden:

- **Hvilke felter indeholder ansøgningsskemaet præcist?** Vi kender ikke det fulde datasæt endnu.
- **Hvilket HTML-format bruger Squarespace-siden i dag?** Det skal reverse-engineers fra den live side.
- **Er zonerne faste hvert år, eller ændrer de sig?** Det har betydning for dropdown-designet.

---

## Næste skridt

1. Hent og analyser HTML-strukturen fra `engestofte.com/da/stadeholderliste` — vi skal kende det præcise format Claude skal generere.
2. Byg formular-interface med de kendte felter og koblet til Anthropic API.
3. Test output mod den rigtige side og finpuds system-prompten til den genererer korrekt HTML hver gang.

---

## Refleksion

Det interessante ved dette projekt er ikke teknologien — det er problem-fittet. Claude kan sagtens generere HTML. Udfordringen er at få system-prompten til at generere *præcis den rigtige* HTML til *præcis dette* Squarespace-setup. Den del kræver at vi forstår formatet til bunds inden vi begynder at kode.

Det er også et godt eksempel på hvornår AI faktisk giver mening: ikke som erstatning for noget komplekst, men som erstatning for noget *kedeligt og fejlprone* — der er en stor forskel.
