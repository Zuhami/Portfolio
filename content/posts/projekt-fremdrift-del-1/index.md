---
title: "Projektfremdrift del 1 — AI-drevet stadeholder-generator"
date: 2026-05-29
description: "Dokumentation af første del af projektforløbet: fra problemidentifikation til fungerende MVP med Claude AI, Flask og Squarespace-integration."
tags: ["ai", "claude", "python", "flask", "webapp", "refleksion"]
series: ["AI-drevne applikationer"]
---

## Projektet kort fortalt

Jeg har bygget en AI-drevet webapp til Engestofte Gods julemarked. Event-koordinator Lise opdaterede tidligere stadeholderlisten på hjemmesiden manuelt — hun skrev HTML direkte i Squarespace for hver enkelt stadeholder. Det er tidskrævende, kræver teknisk viden og er nemt at lave fejl i.

Løsningen: en simpel formular hvor Lise udfylder stadeholderoplysninger, Claude AI genererer korrekt formateret HTML, og hun copy-paster det direkte ind på hjemmesiden. Ingen HTML-kendskab nødvendigt.

**Live demo:** [stadeholder-generator.onrender.com](https://stadeholder-generator.onrender.com)  
**Kildekode:** [github.com/Zuhami/stadeholder-generator](https://github.com/Zuhami/stadeholder-generator)

---

## Hvad jeg har bygget

### MVP — formulargeneratoren

Webapp'en består af tre lag:

**Frontend** (`index.html` + `style.css` + `app.js`)  
En formular med fire felter: virksomhedsnavn, beskrivelse, hjemmeside (valgfri) og zone (dropdown med de 5 faste bygninger på Engestofte). Når Lise klikker "Generer HTML" sendes data til backend, og resultatet vises i en tekstboks med en "Kopiér"-knap.

**Backend** (`server.py` — Flask)  
En letvægts Python-server der modtager formulardata og proxyer det til Anthropic API. API-nøglen lever på serveren og eksponeres aldrig i browseren.

**AI-laget** (Claude Sonnet via Anthropic API)  
Claude modtager strukturerede stadeholderdata og en præcis system-prompt der specificerer det eksakte HTML-format. Den returnerer én klar HTML-blok klar til copy-paste.

### Batch-scriptet

Udover webapp'en har jeg bygget `batch.py` — et CLI-script der læser en hel CSV-fil med stadeholdere og genererer HTML for dem alle på én gang, grupperet per zone. Til slutningen af ansøgningsperioden, hvor Lise har 60+ stadeholdere at behandle, sparer det hende for timevis af arbejde.

---

## Erfaringer og udfordringer

### Prompt engineering er sværere end det lyder

Den største tekniske udfordring var ikke koden — det var at få AI'en til at returnere præcist det format jeg ville have, hver gang. Min første system-prompt var for løs, og AI'en blandede HTML med markdown-syntax i outputtet.

Løsningen var to ting:
1. Give et konkret eksempel på korrekt output direkte i prompten
2. Tilføje server-side rensning med regex der fjerner markdown-elementer selv hvis de dukker op

Det lærte mig at AI-output aldrig er 100% deterministisk — selv en præcis prompt kan give lidt varierende resultater. Defensiv rensning er nødvendig i produktion.

### Felternes oprindelse

En vigtig indsigt kom da jeg analyserede det rigtige ansøgningsskema fra Engestofte. Felterne i webapp'en er direkte mapnet fra skemaet — særligt beskrivelsesfeltet ("Kort og fængende beskrivelse af jeres forretning/stand") som ansøgerne selv udfylder. Det betyder Lise kan copy-paste fra ansøgningen direkte ind i formularen. Ingen genskrivning.

### Zoner er faste — en antagelse der holdt

Jeg antog at zonerne (Den gamle avlsgård, Hestestalden, Laden, Kostalden, Jagtstuen) var faste fra år til år. Det bekræftede stadeplaner og historisk data. Dropdownen er derfor hardcoded — det er den rigtige beslutning for MVP.

### Deployment lærte mig forskellen på dev og produktion

Lokalt kørte appen med `python3 server.py`. I produktion (Render.com) kræves en rigtig webserver — Gunicorn — foran Flask. Det er den klassiske forskel på en udviklingsserver og noget der kan håndtere rigtig trafik. En lille men vigtig detalje.

---

## Hvad der mangler til version 2

Jeg har bevidst afgrænset MVP'en. Disse ting er ikke bygget endnu:

- **Direkte Squarespace-integration** — i stedet for copy-paste, post HTML automatisk via Squarespace API
- **Database over historiske stadeholdere** — genbrug data fra tidligere år
- **Login/brugerstyring** — så kun Lise kan tilgå systemet
- **Batch-upload via UI** — upload en CSV direkte i webapp'en i stedet for CLI

---

## Samlet vurdering

Projektet har vist mig at den sværeste del ved AI-drevne applikationer ikke er at kalde et API — det er at designe et system der er pålideligt nok til at en ikke-teknisk bruger kan stole på outputtet. Prompt engineering, output-validering og brugergrænseflade skal alle stemme overens.

Det er stadig et lille projekt, men det løser et reelt problem for en reel bruger. Det er et godt udgangspunkt.
