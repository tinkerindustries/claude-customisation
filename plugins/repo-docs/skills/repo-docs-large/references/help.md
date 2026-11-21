# User-facing help in docs/help/

`docs/help/` holds the help a person using the product reads. It is written to a style guide (`STYLE.md`), and every page is written by one session and reviewed by a different one (`REVIEW.md`). The help may be shipped inside the product, published to a website, or both, from the same files.

This is the largest folder to set up. Propose it separately from the rest of the structure.

## Contents

- Who help is written for: the reader, and what that rules out
- The four kinds of page: tutorial, how-to, reference, explanation
- The files that make up the help pipeline: STYLE.md, REVIEW.md, briefs, exemplars
- Writing and reviewing in two sessions: why the roles are split
- Facts in help pages: sources, generated fragments, no derived values
- Delivering help: a manifest, a linter, a freshness check
- What this skill writes in docs/help/ and what it leaves

## Who help is written for

A person who has installed the product and wants to use it. They know the general concepts of the domain. They have not read the source, and they do not know the product's own nouns until a page tells them.

Every page is written for that person. A page that names a file under the source tree is in the wrong folder: that material belongs in `ARCHITECTURE.md` or `reference/`.

## The four kinds of page

Each page is exactly one kind, and its directory says which: `tutorials/`, `how-to/`, `reference/`, `explanation/`. These are the four kinds of the Diátaxis framework. A page that wants to be two kinds is two pages.

Two questions decide the kind. Does the page tell the reader what to do, or what is true? Is the reader studying, or working?

| The page | The reader | The kind |
|---|---|---|
| Tells them what to do | Is studying | Tutorial |
| Tells them what to do | Is working | How-to guide |
| Tells them what is true | Is working | Reference |
| Tells them what is true | Is studying | Explanation |

- **A tutorial** takes a reader who has never done the thing through doing it once and getting a result they can see. Numbered steps, one action each, each with what the reader then sees. No choices. It must work, so it is run before it is published.
- **A how-to guide** gives a reader with a job the steps to finish it. It opens on one sentence naming the reader's situation and assumes competence. A troubleshooting page is a how-to guide whose title names the fix: "Fix an engine the app cannot find".
- **A reference page** states facts and stops. Its structure mirrors the product's structure, and it is complete: every field, every value, every default. No `you`, no imperatives, no steps.
- **An explanation page** discusses one topic so the reader understands it. Prose, which may hold opinion and say why a decision was made. No steps.

`assets/help-style-template.md` holds the full rules for each kind, including the title form each one takes.

## The files of the help pipeline

```
docs/help/
├── STYLE.md          The contract every page is written against
├── REVIEW.md         The tests a reviewing session applies, and the two roles
├── briefs/
│   ├── TEMPLATE.md   Copied to start a brief
│   └── <kind>/<slug>.md
├── exemplars/
│   └── <kind>/<slug>.md   One passing page per kind
├── tutorials/
├── how-to/
├── reference/
└── explanation/
```

- **`STYLE.md`** is read in full before writing or editing a page. It covers the reader, the four kinds, sentence rules, word rules (the product's own nouns, a banned-word list), formatting, frontmatter, before-and-after examples, and a checklist. Seed it from `assets/help-style-template.md`. The product's vocabulary and banned words are the user's to supply, so they start as TODOs.
- **`REVIEW.md`** is what the reviewing session applies: tests for kind, facts, completeness, sentences and words, the report shape, and the two roles. Seed it from `assets/help-review-template.md`.
- **A brief** (`briefs/<kind>/<slug>.md`, from `assets/help-brief-template.md`) states the one question a page answers, what the reader already knows, every fact the page must carry, where to check each one, what it links to, and what it leaves out. The writing session invents nothing beyond the brief. A brief that turns out to be missing something goes back to whoever wrote it.
- **An exemplar** is one finished page per kind, checked against the running product, that shows what a passing page looks like. Both sessions read it before the brief. The first page written in each kind, once it passes review, can be copied to `exemplars/`.

## Writing and reviewing in two sessions

A page passes through two roles, always played by two different sessions.

- **The writing session** gets the running product, `STYLE.md`, the exemplar for the kind and the brief. It returns the page with complete frontmatter, lint-clean if a linter exists, and applies every finding from review until a review comes back with none.
- **The reviewing session** gets the finished page, the same brief, `REVIEW.md`, the exemplar, and the running product and code. It never gets the writing session's transcript or reasoning. It reports findings and does not edit the page. A finding names the test, quotes the text, and says what is wrong.

The reviewer is kept away from the writer's reasoning so that it catches assumptions the writer could not see it was making. A page lands when a review returns no findings.

## Facts in help pages

- Every fact is checked against the running product, a generated fragment, or the code. The page's `sources:` frontmatter names what each fact was checked against.
- A page never carries a date, a version number, a count, or anything else that will be wrong later. It says where the reader reads the current value.
- A fact derivable from the code (a settings table, a list of shortcuts) is generated into a fragment under `reference/generated/` and included, so a page never retypes it.
- Controls are named exactly as the screen spells them, in bold.

Frontmatter on every page:

```yaml
---
title: Pair two machines
summary: One sentence, under 120 characters, shown in lists and search results.
kind: how-to
sources: [Settings > Services tab]
---
```

A repo that tests which product surfaces have help can add a `surfaces:` list naming them.

## Delivering help

These pieces exist only when the help is delivered somewhere beyond the repository. Set each up only when the repo already has the delivery, and list the rest as follow-up work.

- **A manifest** (`docs/help/manifest.json`) listing each page's section, slug, title and summary, when the product or a website builds its help navigation from one.
- **A linter** for the mechanical rules: sentence length, banned words, frontmatter fields, a `kind` that matches its directory. Wire it into `CLAUDE.md` as the command a page edit runs.
- **A freshness check** that flags a page when a surface it names changes, so help is revisited in the pull request that changes the product.

## What this skill writes in docs/help/ and what it leaves

Write `STYLE.md`, `REVIEW.md` and `briefs/TEMPLATE.md` from the templates, with the product's name and vocabulary filled in where the repo shows them and TODOs where it does not. Move existing user-facing material into the right kind's directory only when it already reads as one kind. Otherwise list it as a candidate brief.

Do not write help pages. A page needs every fact checked against the running product, which this skill does not do. Report the briefs worth writing first: one tutorial for a new user's first success, and a how-to guide for each task the README or the issue list shows people asking about.
