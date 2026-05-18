---
title: "Gang 1 — Course Intro & Portfolio Setup"
date: 2026-04-10
description: "Setting up a Hugo portfolio with Blowfish, and a reflection on expectations for AI in software development."
tags: ["hugo", "portfolio", "ai", "reflection"]
series: ["AI-Driven Applications"]
---

## What We Did

The first session introduced the elective's scope: AI-driven applications, how LLMs fit into modern software architecture, and what the exam looks like. We immediately got hands-on — every student set up a portfolio website to document the cases we'll build throughout the course.

The stack for the portfolio:

- **Hugo** — static site generator, fast builds and simple deployment
- **Blowfish** — a clean Hugo theme with dark mode and a profile layout
- **GitHub Pages** — free hosting directly from the repo's `docs/` folder

Getting from zero to a deployed site took roughly 30 minutes: install Hugo, clone the Blowfish template, configure `params.toml`, run `hugo` to generate the `docs/` folder, push to GitHub, and enable Pages in the repo settings.

---

## Reflection — Expectations for AI in Software Development

Before this course I'd used LLMs mainly as a smarter autocomplete — answering questions, generating boilerplate, explaining error messages. Useful, but mostly a productivity shortcut layered on top of the same old workflow.

What I'm curious to explore here is the step further: AI not just assisting a human writing code, but as an active component inside the software itself. An LLM that reads data, makes decisions, and calls other systems changes the architecture of an application in ways that purely statistical autocomplete does not.

A few things I expect to learn more about:

**Prompt engineering as software design.** The way you frame a task for an LLM determines correctness just as much as the surrounding code does. I expect this to feel closer to writing precise specifications than to writing instructions.

**When not to use an LLM.** Deterministic problems have deterministic solutions. I'm going into this with the assumption that the interesting design challenge is knowing where the boundary sits.

**Reliability and evaluation.** Traditional software has unit tests. LLM output is probabilistic. I'm curious how teams define "correct" for a model-in-the-loop system and what evaluation tooling exists.

The part I'm most sceptical about is hype versus substance — a lot of "AI-powered" products are thin wrappers. I'm hoping the course gives me enough depth to tell the difference.
