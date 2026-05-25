---
title: "Gang 2 — Retrieval Augmented Generation (RAG)"
date: 2026-04-17
description: "Vi prøvede tre forskellige RAG-værktøjer og endte med at bygge en chatbot til vores studieordning."
tags: ["rag", "ai", "llm", "dify", "refleksion"]
series: ["AI-drevne applikationer"]
---

## Hvad er RAG?

En normal LLM ved kun det den er trænet på. RAG — Retrieval Augmented Generation — løser det ved at give modellen en videnskilde den kan søge i, når der kommer et spørgsmål. Flowet ser nogenlunde sådan her ud:

**Spørgsmål → embed spørgsmålet → søg i vector store → hent de mest relevante chunks → smid dem ind i prompten → LLM svarer**

Det giver to fordele: modellen kan svare på spørgsmål om dokumenter den aldrig har set, og den kan pege på konkrete passager frem for at finde på et svar der lyder rigtigt.

RAG giver bedst mening når kildematerialet skifter tit, er privat, eller er for stort til at putte direkte i en prompt. Til generelle spørgsmål hvor modellen allerede ved nok er det bare unødigt komplekst.

---

## Tre værktøjer vi kiggede på

| Værktøj | Godt | Skidt |
|---|---|---|
| **ChatGPT CustomGPT** | Nul opsætning, man kender interfacet | Låst til OpenAI, ingen reel kontrol |
| **customGPT.ai** | Nem at embed'e på en side | Betalt fra start, black-box |
| **Dify.ai** | Open source, fuld kontrol over pipelinen | Lidt mere at sætte op |

Vi endte med at bruge **Dify.ai** til den praktiske del. Det er det eneste af de tre der lader dig se og justere hvad der faktisk sker — chunk-størrelse, embedding-model, retrieval-indstillinger og det hele.

---

## Hvad vi byggede

Vi lavede en chatbot der kan svare på spørgsmål om vores studieordning. Det er et tæt PDF-dokument fyldt med regler og formuleringer som ingen gider læse igennem manuelt — perfekt use case.

**Dataforberedelse**

Inden vi uploadede dokumentet konverterede vi det fra PDF til Markdown. Rå PDF-udtrækning er ret rodet — overskrifter blandes med brødtekst, tabeller går i stykker, sidetal dukker op midt i sætninger. Markdown giver chunker'en noget rent at arbejde med, og det mærkes på kvaliteten af svarene.

**Chunking og indeksering**

Dify deler dokumentet op i overlappende chunks og kører dem igennem en embedding-model der laver dem om til vektorer. De vektorer gemmes og søges via cosine similarity når der kommer et spørgsmål.

**Chatbotten**

Når man stiller et spørgsmål henter chatbotten de mest relevante chunks og putter dem ind i prompten. System-prompten siger til modellen at den kun må svare ud fra det dokument — det reducerer hallucination og holder den on-topic.

---

## Refleksion

Det mest interessante var ikke selve chatbotten — det var at se hvor meget dataforberedelsen betød. Det samme spørgsmål mod rå PDF og renset Markdown gav mærkbart forskellige svar. Garbage in, garbage out gælder åbenbart lige så meget her som alle andre steder.

Sammenligningen mellem værktøjerne viste også noget ret tydeligt: jo nemmere det er at komme i gang, jo mindre kan du se hvad der sker indeni. CustomGPT virker på fem minutter, men når det fejler ved du ikke hvorfor. Dify tager længere tid, men du kan faktisk debugge det. Til noget der skal bruges i produktion vil jeg altid foretrække at kunne se hvad der foregår.
