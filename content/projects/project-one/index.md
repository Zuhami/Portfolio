---
title: "AI-drevet stadeholder-generator"
date: 2026-05-29
draft: false
description: "Webapp der bruger Claude AI til at generere Squarespace-klar HTML for stadeholdere på Engestofte Gods julemarked."
tags: ["python", "flask", "ai", "claude", "html"]
weight: 1
---

## Oversigt

Engestofte Gods afholder hvert år et julemarked med over 60 stadeholdere fordelt på 5 zoner. Event-koordinator Lise opdaterede tidligere stadeholderlisten på hjemmesiden manuelt ved at skrive HTML direkte i Squarespace — tidskrævende, fejlbehæftet og kræver teknisk viden.

Dette projekt løser problemet med en simpel webapp: Lise udfylder en formular med stadeholderoplysninger, AI genererer korrekt formateret HTML, og hun copy-paster det direkte ind på hjemmesiden.

## Funktioner

- Formular med felterne: virksomhedsnavn, beskrivelse, hjemmeside og zone
- 5 faste zoner som dropdown: Den gamle avlsgård, Hestestalden, Laden, Kostalden, Jagtstuen
- Claude AI genererer Squarespace-kompatibel HTML med korrekt centrering og formatering
- "Kopiér HTML"-knap til hurtig copy-paste
- Ingen login, ingen database — ren input → AI → output

## Tech Stack

**Frontend:** HTML, CSS, vanilla JavaScript  
**Backend:** Python, Flask  
**AI:** Claude Sonnet via Anthropic API  

## Problemet der løses

Felterne i webapp'en er direkte mapnet fra Engestoftes officielle ansøgningsskema — særligt beskrivelsesfeltet, som ansøgerne selv udfylder og som Lise modtager og skal publicere. Det eliminerer dobbeltarbejdet med at genskrive information i HTML.

## Hvad jeg lærte

- Prompt engineering: hvordan man specificerer præcist HTML-format til et AI-output uden at modellen blander markdown ind
- Flask som letvægts API-proxy der holder API-nøglen ude af frontend
- Vigtigheden af server-side output-rensning når AI genererer struktureret tekst

## Links

[🚀 Prøv live demo](https://stadeholder-generator.onrender.com) &nbsp;·&nbsp; [Kildekode på GitHub](https://github.com/Zuhami/stadeholder-generator)
