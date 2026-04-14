# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Content repository for a **course about DeerFlow 2.0** (ByteDance's open-source super-agent harness). This is **not** the DeerFlow source code — it is didactic material (roteiros, slides, labs) written in Markdown. There is no build, no tests, no lint. All "work" is editing `.md` files.

Primary language of the content is **PT-BR** (the course targets Brazilian devs/PMs/researchers). Keep new content in PT-BR unless the user says otherwise — the "idioma final" is still an open decision in `docs/plano-do-curso.md`.

## Architecture of the course (what lives where)

The course is split into **two tracks + one transversal module**, mirrored by three top-level directories:

- `docs/plano-do-curso.md` — **the anchor document**. Master plan: modules, labs, deliverables, open decisions. Always read this first when asked to add/change course content — every module's scope is defined here, and the track READMEs are just indexes that point back to it.
- `track-1-fundamentos/` — "O que é e como usar". Modules 0–6. Audience: non-coders (devs curiosos, PMs, pesquisadores). No Python required.
- `track-2-avancado/` — "Para quem vai estender o harness". Modules A1–A10. Audience: AI/platform engineers. Prereq: Track 1 + Python 3.12 + LangGraph familiarity.
- `comparativo/` — Transversal module comparing **DeerFlow vs. Claude Code vs. Antigravity**. Designed as a standalone module that can run after Track 1 Módulo 3. Uses a fixed 10-axis comparison matrix and 4 side-by-side labs.

Each track README is an index table (`# | Título | Status`) where every module is currently marked `a produzir`. When a module's content is produced, update its row status and add the actual material as new files in that track directory.

## Key conventions

- **Single source of truth**: module scope, labs, and deliverables live in `docs/plano-do-curso.md`. Track READMEs must not contradict it — if scope changes, update the plan first, then the track README.
- **Lab-first pedagogy**: the plan explicitly says "só teoria não sedimenta". Every module except Módulo 0 has a hands-on lab. When drafting new module content, always include a lab section with concrete commands/prompts, not hand-wavy descriptions.
- **Open decisions block content**: the end of `plano-do-curso.md` has a "Decisões em aberto" checklist (format, idioma, lab depth, duration, Track 2 prereqs). Several of these gate how detailed new content should be — if asked to produce a module, check this list and flag unresolved decisions instead of guessing.
- **DeerFlow v1 ≠ v2**: v2 is a full rewrite and shares no code with v1 (Deep Research). Don't conflate them in course material.
