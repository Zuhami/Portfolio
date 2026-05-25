---
title: "Gang 1 — Kursusintro & Portfolio-opsætning"
date: 2026-04-10
description: "Vi satte Hugo-portfolios op og snakkede om hvad vi egentlig forventer at få ud af kurset."
tags: ["hugo", "portfolio", "ai", "refleksion"]
series: ["AI-drevne applikationer"]
---

## Hvad vi lavede

Første gang gik med at få styr på hvad kurset egentlig handler om — AI-drevne applikationer, hvordan LLM'er bruges i rigtig software, og hvad eksamen kommer til at se ud. Ret hurtigt efter gik vi i gang: alle skulle have et portfolio-site op at køre, som vi kan bruge til at dokumentere det vi bygger hen ad vejen.

Stacken:

- **Hugo** — statisk site-generator, hurtige builds og nemt at deploye
- **Blowfish** — et Hugo-tema med dark mode og et fedt profilayout
- **GitHub Pages** — gratis hosting direkte fra repo'ets `docs/`-mappe

Fra nul til live site tog ca. 30 minutter. Installer Hugo, klon Blowfish, konfigurer `params.toml`, kør `hugo`, push til GitHub og slå Pages til. Ret simpelt.

---

## Refleksion — Hvad jeg håber at få ud af det her

Inden kurset har jeg mest brugt LLM'er som en lidt klogere autocomplete — få svar på spørgsmål, generere boilerplate, forstå fejlmeddelelser. Det er praktisk, men det er bare et lag ovenpå den samme arbejdsgang som altid.

Det jeg synes er interessant er skridtet videre: AI ikke bare som hjælp mens man koder, men som en del af selve applikationen. En LLM der selv læser data, tager beslutninger og kalder andre systemer — det er en anden slags arkitektur end det vi normalt arbejder med.

Et par ting jeg er nysgerrig på:

**Prompt engineering som design.** Måden man formulerer en opgave til en LLM har lige så meget at sige for resultatet som koden rundt om. Det lyder mere som at skrive specifikationer end instruktioner.

**Hvornår skal man ikke bruge en LLM.** Ikke alt skal være AI. Jeg tror den svære del er at finde ud af præcis hvor det giver mening og hvor det bare er unødigt komplekst.

**Hvordan tester man det her.** Normal software har unit tests. LLM-output er ikke deterministisk. Jeg aner ikke rigtigt hvordan man definerer "korrekt" i det setup, og det vil jeg gerne finde ud af.

Den ting jeg er mest skeptisk over for er hype. Mange "AI-drevne" produkter er bare et tyndt lag oven på en API. Jeg håber kurset giver mig nok indsigt til at se forskel på det og noget der faktisk er gennemtænkt.
