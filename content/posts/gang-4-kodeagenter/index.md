---
title: "Gang 4 — Kodeagenter i softwareudvikling"
date: 2026-04-20
description: "Vi brugte AI-kodeagenter til at løse en reel programmeringsopgave og dokumenterede hele workflowet fra opgaveformulering til færdigt, testet resultat."
tags: ["ai", "claude", "python", "webapp", "refleksion"]
series: ["AI-drevne applikationer"]
---

## Hvad er en kodeagent?

En kodeagent er en LLM der ikke bare besvarer spørgsmål, men aktivt løser programmeringsopgaver — den kan læse filer, skrive kode, køre tests og iterere på resultatet. Den arbejder som en kollega der kender kodebasen, ikke som en søgemaskine der returnerer snippets.

Værktøjer vi kiggede på:

| Agent | Hvad den gør |
|-------|-------------|
| **Claude Code** | Kørende i terminalen, kan læse og redigere hele projekter |
| **OpenAI Codex** | GitHub Copilot-lignende integrationer |
| **Gemini CLI** | Googles CLI-agent, tæt integreret med Google-stack |

---

## Opgaven vi løste

Vi tog udgangspunkt i det eksisterende stadeholder-generator projekt og udvidede det med en ny funktion:

> **Opgave:** Byg et batch-script der læser en liste af stadeholdere fra en CSV-fil og genererer samlet Squarespace-HTML for alle på én gang — uden at Lise skal udfylde formularen manuelt for hver enkelt.

Det er en reel use case: ved slutningen af ansøgningsperioden har Lise 60+ stadeholdere at behandle. Et batch-script sparer hende for timevis af arbejde.

---

## Workflowet med kodeagenten

### 1. Opgaveformulering
Det første skridt var at give agenten en præcis opgavebeskrivelse med kontekst:

- Hvad eksisterer allerede (Flask-app, system-prompt, API-opsætning)
- Hvad det nye script skal gøre (læse CSV, kalde API per række, gruppere output per zone)
- Hvilke edge cases der skal håndteres (manglende hjemmeside, ukendte zoner)

**Læring:** En god opgaveformulering til en kodeagent minder mere om en teknisk spec end et spørgsmål. Jo mere kontekst, jo færre iterationer.

### 2. Første implementation

Agenten genererede `batch.py` med:
- CSV-læsning via `csv.DictReader`
- Generer HTML per stadeholder via Anthropic API
- Gruppering af output per zone med HTML-kommentarer som separatorer

```python
# Kernefunktionen agenten genererede
def generer_html(client, navn, beskrivelse, hjemmeside, zone):
    besked = f"Zone: {zone}\nNavn: {navn}\nBeskrivelse: {beskrivelse}"
    if hjemmeside:
        besked += f"\nHjemmeside: {hjemmeside}"
    response = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=512,
        system=SYSTEM_PROMPT,
        messages=[{"role": "user", "content": besked}],
    )
    return response.content[0].text.strip()
```

### 3. Test afslører en fejl

Vi testede mod en CSV med 3 stadeholdere. Output for den tredje var pakket ind i markdown kodeblokke:

```
```html
<p style="text-align: center;"><strong>Casa Italy</strong></p>
```
```

AI'en returnerede markdown i stedet for ren HTML — samme problem vi stødte på i webapp'en. Agenten identificerede mønsteret og tilføjede rensning:

```python
# Agentens fix — regex der stripper markdown kodeblokke
html = re.sub(r'```(?:html)?\n?(.*?)```', r'\1', html, flags=re.DOTALL).strip()
```

### 4. Verificeret output

Efter rettelsen kørte scriptet rent mod alle 3 testdatalinjer og producerede korrekt formateret HTML grupperet per zone.

---

## Refleksion

**Hvad gik godt**

Agenten forstod konteksten fra det eksisterende projekt uden at jeg skulle forklare strukturen fra bunden. Den valgte de rigtige biblioteker, håndterede edge cases og fulgte samme kodningsmønstre som resten af projektet.

**Hvad krævede iteration**

Output-rensning var ikke perfekt første gang. LLM'er er ikke deterministiske — samme prompt kan give lidt forskelligt output. Det betød at fejlen ikke altid reproducerede sig, hvilket gør det sværere at spotte under udvikling. Agenten løste det ved at tilføje defensiv rensning der fanger alle varianter.

**Den vigtigste observation**

Kodeagenter er ikke bedst til at skrive kode fra ingenting — de er bedst til at *udvide* eksisterende kode de har kontekst på. Jo mere agenten ved om projektet, jo mere præcist og konsistent bliver resultatet.

---

## Kildekode

[github.com/Zuhami/stadeholder-generator](https://github.com/Zuhami/stadeholder-generator) — se `batch.py`
