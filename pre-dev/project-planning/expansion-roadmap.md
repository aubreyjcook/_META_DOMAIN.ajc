# Repository Expansion Roadmap

> **Status:** Proposal — a suggested path for growing this repo, not a commitment.
> **Scope:** How `_META_DOMAIN.ajc` could expand in structure, content, and tooling.

## Summary

This repo's job, per the tagging semantics, is to be a **non-coding, informational repository that documents the developer's domain of knowledge**. Growth should therefore be judged by one test: *does this addition make the documented knowledge domain wider, deeper, or easier to navigate?* — not by whether the directory tree looks fuller.

The highest-leverage moves, in order:

1. **Finish what is already half-written** before adding anything new — several notes end mid-thought, and several files already declare a "planned" list that is a ready-made backlog.
2. **Add a small metadata layer** (YAML front-matter) so the repo becomes searchable, sortable, and toolable as it grows, instead of relying on filenames alone.
3. **Expand the two halves deliberately** — `guidelines/` (rules) and `notes/` (knowledge) — filling the stubs that already have names, then adding domains the developer actually works in.

The rest of this document expands each of those into concrete, prioritized suggestions.

## Guiding Principles

- **Document knowledge, not scaffolding.** Empty directories and one-line overviews are placeholders, not progress. A stub is fine as a *plan*, but the value comes from filling it.
- **Respect the `_META_` / `_DOMAIN_` split.** `guidelines/` and `templates/` are operating rules (`_META_`); `notes/`, `llm/`, and `agents/` are the knowledge domain (`_DOMAIN_`). Don't blur the line — a rule belongs under guidelines, a reflection belongs under notes.
- **Keep the invariant.** Every subject directory has an `{topic}-overview.md` that lists *only* files that exist. Growth must keep this true.
- **Fill before you grow.** The cheapest, highest-quality content is completing what already has a heading.

## 1. Finish Before Growing (near-term, highest value)

These are items that already have a place in the repo but are incomplete. Completing them adds real depth with no structural churn.

### Complete half-written notes

- [hyper-minimalization.md](../notes/personal-observations/hyper-minimalization.md) — the **Disadvantages** section is empty and **Applications** has headers only.
- [sub-disciplines-of-development.md](../notes/personal-observations/sub-disciplines-of-development.md) — the "Fundamental Sub-Disciplines" list is empty; Security/Data-Handling/Optimization sections are headers only.
- [learning-overview.md](../notes/learning/learning-overview.md) — "Understanding vs. Memorization" and "Mental Modelling" are empty.
- [numeric-pin-ciphering.md](../notes/hacking/numeric-pin-ciphering.md) — the 3/4/5/6-digit ciphering methods are marked as stubs.
- [project-initializations-overview.md](../guidelines/project-management/project-preparation/project-initializations-overview.md) — the checklist, naming, description, and directory-structure sections are empty.
- [nextjs.md](../guidelines/technology-specific-guidelines/frameworks/nextjs.md) — "'Vanilla' Next vs Next with TypeScript/Tailwind" is an empty heading.

### Fill the "planned" lists already declared in the repo

The repo already contains its own backlog. Execute it first:

- **Hacking fundamentals** ([fundamentals-overview.md](../notes/hacking/fundamentals/fundamentals-overview.md)) — character encoding (ASCII/UTF-8/Unicode), bitwise operations and bitmasks, endianness, boolean logic.
- **Hacking topics** ([hacking-overview.md](../notes/hacking/hacking-overview.md)) — cryptography, forensics, networking, reverse engineering.
- **Git** ([git-overview.md](../guidelines/source-control/git/git-overview.md)) — the "Not written yet" list: synchronization, pull requests, forking, merging, reverts, configuration, ignore-file notes.
- **Agentic AI** ([agentic-ai-overview.md](../agents/agentic-ai-overview.md)) — agent types/orchestration, delegation and review workflows, guardrails.

## 2. Infrastructure to Make It Scale

Right now the repo is navigable by hand because it's small. As the knowledge domain grows, three additions prevent it from becoming unmanageable.

### YAML front-matter

Add a small header block to every note so it becomes machine-readable:

```yaml
---
title: Binary
type: note        # note | guideline | template
status: complete  # complete | wip | stub
tags: [hacking, fundamentals, number-systems]
updated: 2025-01-01
---
```

Benefits: stubs become programmatically detectable, tags enable cross-cutting views (e.g. "everything about networking"), and a future static-site or search build becomes trivial.

### Formalize the status taxonomy

We already introduced `> **Status:** Stub — planned but not yet written.` Make it a three-value system and apply it consistently: **complete**, **wip**, **stub**. This gives the repo an honest, scan-able health signal.

### A root index

The nested overviews are good, but a single `INDEX.md` at the root (auto-listing every page by section) would serve as the authoritative map. This can be generated by a tiny script rather than hand-maintained, which also lets us **enforce the "overview lists only what exists" invariant** with a check (a shell script or pre-commit hook that verifies every link resolves).

### Optional: stable addresses (Johnny Decimal)

If the domain grows large, a numbering scheme (e.g. `10-19` = hacking, `10.1` = fundamentals, `10.1.2` = binary) gives every note a stable address that survives renames. This is optional and can be deferred until navigation actually hurts.

### Commit hygiene

The repo *is* the authority on commit conventions, so its own history should follow [commit-messages.md](../guidelines/source-control/git/commit-messages.md) going forward (and, optionally, the `update`-only history could be squashed once).

## 3. Guidelines Expansion (the `_META_` half)

The rules side should stay lean and genuinely prescriptive. Suggested additions, grouped:

### Source control

- Complete the [Git "not written yet" list](../guidelines/source-control/git/git-overview.md) (sync, pull requests, forking, merging, reverts, config, ignore-files).
- Add a **`.gitignore` reference** — common per-language ignore entries.

### Licensing

- [open-source-licensing-overview.md](../guidelines/open-source-licensing/open-source-licensing-overview.md) is a stub. Expand it into: a comparison of the major licenses (MIT, Apache-2.0, GPL-2.0/3.0, AGPL, MPL, BSD, Unlicense), how to choose one, and a permissive-vs-copyleft framing.

### Project management

- Write actual **SOPs** (currently an empty [sops-overview.md](../guidelines/project-management/sops/sops-overview.md)) — e.g. "starting a new repo", "releasing a version", "onboarding to an existing project".
- Provide real **pseudo-code templates** (currently empty).
- Finish the **project initialization checklist** (naming, description, directory structure, initial docs).

### Technology-specific

- **Windows / Linux** — fill the two stub overviews with the OS-management notes they promise (environment setup, package managers, permissions, shell).
- **Apache** — the [htaccess](../guidelines/technology-specific-guidelines/web-server/apache.md) section is empty; document redirects, rewrites, auth, caching.
- **Applications** — fill with concrete app-specific notes (browsers, terminal, git clients).
- New areas worth having guidelines for, if they're part of the developer's workflow: **testing**, **CI/CD**, **deployment**, **databases**, **API design**, **accessibility**, **security**, **performance**.

## 4. Knowledge-Domain Expansion (the `_DOMAIN_` half)

This is the core of the repo's purpose. Prioritize by what the developer actually knows and works with, not by what sounds impressive.

### Hacking (the natural flagship)

Build a two-track structure:

- **Fundamentals path** — continue from binary/hex: character encoding → bitwise ops → boolean logic → endianness → how memory is laid out. Each builds on the last.
- **Applied path** — cryptography (hashing, symmetric/asymmetric, the math from the fundamentals), networking (TCP/IP, ports, packets), forensics (file formats, carving), reverse engineering (reading machine code, disassembly), and web security (injection, auth, XSS/CSRF).

### Mathematics

The [mathematics-overview.md](../notes/mathematics/mathematics-overview.md) stub should grow into the logical foundation the repo already gestures at: **number bases** (done, cross-linked), **boolean algebra**, **propositional/predicate logic**, **discrete math**, **modular arithmetic** (feeds directly into cryptography), and **asymptotic complexity (big-O)**.

### Electronics

The [electronics-overview.md](../notes/electronics/electronics-overview.md) stub is the physical root of the hacking section. Expand into: **digital logic gates** (AND/OR/NOT, and how they produce the binary states), **microcontrollers**, **signals and voltage**. This closes the loop back to "why computers use binary."

### Web

[web-overview.md](../notes/web/web-overview.md) currently has only responsive breakpoints. Grow it into: **HTTP** (methods, status codes, headers), **HTML/CSS/JS fundamentals**, **accessibility**, **performance**, and **security** — the topics its own section title implies.

### Machine learning

Fill the [machine-learning-overview.md](../notes/machine-learning/machine-learning-overview.md) stub with: fundamentals (supervised/unsupervised/reinforcement), core model families, evaluation (accuracy/precision/recall), and when *not* to use ML (which ties back to the "AI hysteria" observation).

### Learning and personal development

Complete the two empty subsections in [learning-overview.md](../notes/learning/learning-overview.md), and consider adding: spaced repetition, deliberate practice, and reading-source-code strategies.

### Personal observations

Finish [hyper-minimalization.md](../notes/personal-observations/hyper-minimalization.md) and [sub-disciplines-of-development.md](../notes/personal-observations/sub-disciplines-of-development.md). These are the most original content in the repo and deserve completion before new note stubs.

### New domains (only if genuinely part of the knowledge domain)

Candidates: **data structures & algorithms**, **databases/SQL**, **networking**, **operating-system internals**, **programming-language theory**, **compilers/interpreters**.

## 5. Templates Expansion

Templates are cheap wins because they're reusable across all future repos:

- Complete [drive-template.md](../templates/drive-template.md) (the "Directories" section is empty).
- Add: an **SOP template**, a **commit-message cheat sheet**, a **license-header block**, a **software-architecture outline**, a **testing plan**, a **deployment checklist**, and **issue/bug templates**.

## 6. LLM and Agents Expansion

- **LLM** — add a **prompt library** (reusable, proven prompts), a **model/tool comparison**, and **workflow notes** (research vs implementation, as already sketched in [chatgpt-overview.md](../llm/chatgpt/chatgpt-overview.md)).
- **Agents** — fill the planned list: agent types, orchestration patterns, delegation/review workflows, and guardrails (when to let an agent act vs ask). The [agent-instructions template](../agents/agent-instructions/agent-instructions.md) could gain concrete per-project examples.

## 7. Project Planning

- Fill [edu-repos-consolidation.md](edu-repos-consolidation.md) — actually list the educational/legacy repos to consolidate, with a migration order.

## Suggested Phased Roadmap

**Phase 1 — Finish existing (highest value, no structure change).**
Complete the half-written notes (§1) and fill the declared "planned" lists.

**Phase 2 — Infrastructure.**
Add YAML front-matter, formalize statuses, add a generated `INDEX.md` and a link/structure check.

**Phase 3 — Guidelines depth.**
Git backlog, open-source licensing, real SOPs, project-init checklist.

**Phase 4 — Knowledge depth.**
Hacking fundamentals → applied track; mathematics; web; electronics; machine learning.

**Phase 5 — Breadth and tooling.**
New templates, LLM/agents expansion, new domains as the developer's knowledge grows.

## Decision Points to Resolve Before Expanding

1. **Which knowledge domains are actually in scope?** — The single most important question. The repo should document what the developer knows, so prioritize domains by real experience.
2. **Front-matter and tooling now or later?** — Adding front-matter is easiest *before* the file count doubles.
3. **Numbering scheme?** — Johnny Decimal is valuable once navigation hurts; optional now.
4. **Commit-history rewrite?** — Squashing the `update`-only history would let the repo demonstrate its own commit convention, but requires a force-push.

## Bottom Line

The repo is now structurally sound and honestly self-described. The next phase should be **depth over breadth**: finish the notes that are already half-written and the backlog the repo already lists for itself, add the small metadata layer that makes growth tractable, and only then add new domains — chosen by what the developer genuinely knows, which is exactly what `_DOMAIN_` exists to record.
