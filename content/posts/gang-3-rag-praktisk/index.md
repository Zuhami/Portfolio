---
title: "Gang 3 — RAG i praksis: Vi bygger fra bunden"
date: 2026-04-17
description: "Vi byggede en RAG-løsning fra bunden — konverterede data til markdown, optimerede chunking og promptdesign, og deployede en chatbot direkte på portfoliosiden."
tags: ["rag", "ai", "llm", "dify", "refleksion", "webapp"]
series: ["AI-drevne applikationer"]
---

## Dagens mål

Denne gang gik fra teori til praksis. Vi skulle bygge en RAG-løsning helt fra bunden — vælge en case, forberede data, sætte retrieval op og tune promptdesignet til noget der faktisk virker.

Fokuspunkterne var:

- Konverter rå data til markdown for bedre chunking
- Optimer datakilder og dokumentstruktur
- Tune retrieval og promptdesign for bedre svarkvalitet
- Deploy en chatbot direkte på portfolio-sitet

---

## Hvad vi byggede

Vi brugte **Dify.ai** som platform — det giver fuld kontrol over hele pipelinen uden at man skal bygge embedding-infrastruktur fra bunden.

**Dataforberedelse**

Det første og vigtigste skridt: konverter kildematerialet fra PDF til Markdown. Rå PDF-udtrækning er fyldt med støj — sidetal midt i sætninger, ødelagte tabeller, manglende overskriftshierarki. Markdown giver chunker'en rent input, og det mærkes direkte på kvaliteten af svarene.

**Chunking og indeksering**

Dify splitter dokumentet op i overlappende chunks og embedder dem som vektorer. Størrelsen på chunks har stor betydning — for store chunks giver for meget irrelevant kontekst, for små chunks mister sammenhæng. Vi testede os frem.

**Promptdesign**

System-prompten instruerer modellen i kun at svare ud fra det hentede materiale. Det reducerer hallucination og holder svarene relevante. Vi itererede på formuleringen for at finde den rette balance mellem præcision og fleksibilitet.

---

## Refleksion

Det mest overraskende var igen dataforberedelsen. Samme spørgsmål mod rå PDF og renset Markdown gav mærkbart forskellige svar — ikke fordi modellen er bedre, men fordi den har bedre kontekst at arbejde med.

Promptdesign viste sig at være ligeså vigtigt som selve retrieval-opsætningen. En uklar systemprompt giver upræcise svar selv med perfekte chunks. Det er to separate problemer der begge skal løses.

---

## Live demo

Prøv chatbotten herunder — den er bygget med Dify og deployede direkte på dette site:

<iframe
 src="https://udify.app/chatbot/PV3bkgzY8cly4dy8"
 style="width: 100%; height: 100%; min-height: 700px"
 frameborder="0"
 allow="microphone">
</iframe>
