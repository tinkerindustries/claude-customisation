# How to write <Product>'s help

Every page under `docs/help/` follows this document. Read it fully before writing or editing a
page. <If a linter exists: `<lint command>` checks the mechanical rules; the rest is on the writer.>

## Who reads these pages

A person who has installed <Product> and wants to use it. <What they already know: the domain
concepts a reader brings.> They have not read the source. They do not know what <Product> calls
anything until a page tells them.

Write for that person. A page that mentions a file in the source tree is in the wrong folder.
That material belongs in `ARCHITECTURE.md` or `docs/reference/`.

## The four kinds

Each page is exactly one kind, and its directory says which. A page that wants to be two kinds
is two pages.

Decide with two questions. Does the page tell the reader what to do, or tell them what is true?
Is the reader studying, or working?

| The page | The reader | The kind |
|---|---|---|
| Tells them what to do | Is studying | Tutorial |
| Tells them what to do | Is working | How-to guide |
| Tells them what is true | Is working | Reference |
| Tells them what is true | Is studying | Explanation |

### A tutorial

A tutorial takes a reader who has never done the thing through doing it once, completely, and
getting a result they can see.

- The title starts with a possessive or an article: `Your first project`, `Running a report`.
- Open with a `## By the end` section: one paragraph on what the reader will have, and how long
  it takes.
- Then a `## Before you start` section: a bulleted list of what they need.
- Number every step. One action per step.
- Every step says what the reader does and what they then see. A step with no observable result
  gives the reader no way to know it worked.
- Use the exact words on the screen, in bold.
- Never offer a choice. Pick one path. Alternatives belong in a how-to guide.
- Never explain why beyond one clause. Link to an explanation page instead.
- If a step commonly goes wrong, say what the reader sees when it does and what to do.
- End with a short list of links to the how-to guides that follow on.

A tutorial must work. Run it before you publish it.

### A how-to guide

A how-to guide gives a reader with a job the steps to finish it.

- The title starts with a verb in the infinitive without `to`: `Export a report`.
- Open with one sentence naming the situation the reader is in.
- Then the steps, numbered. State prerequisites as a step.
- Offer alternatives where they genuinely exist, and say when each applies.
- Assume competence. Do not describe what a button looks like.
- End with the reference pages the guide draws on.

A troubleshooting page is a how-to guide. Its title names the fix as a goal: `Fix a sign-in
that never finishes`.

### A reference page

A reference page states facts and stops.

- The title is a noun phrase: `Settings`, `Keyboard shortcuts`.
- Its structure mirrors the product's structure. Fields appear in the order the screen shows them.
- Tables for anything with more than two columns of fact. Every table has a header row.
- Describe. Do not instruct. No `you`, no imperative verb, no numbered steps.
- Be complete. Every field, every value, every default.
- Where a fact is derivable from the code, include the generated fragment from
  `docs/help/reference/generated/` rather than typing the fact.

### An explanation page

An explanation page discusses one topic so the reader understands it.

- The title is a question or a topic: `Why <Product> stores no password`.
- Prose, in paragraphs. Headings for the turns in the argument.
- It may hold opinion and say why a decision was made.
- No numbered steps and no instruction to press anything.
- It links to the reference it interprets and the how-to guides it makes sense of.

## Sentences

- One idea per sentence. <Set a ceiling, for example twenty-five words.>
- A person or a thing does something. Name the actor.
- Present tense, active voice.
- `You` for the reader and `<Product>` for the product. Never `we`, `the user` or `one`.
- Say what happens in the order it happens.
- No hedging: remove `generally`, `typically`, `usually`, `should`, `may want to`. When a thing
  varies, say what it depends on.
- No filler openers: `Note that`, `Keep in mind`, `Simply`, `Just`, `In order to`.
- No sentences about the page itself: `This guide will show you`, `Below you will find`.
- No summary at the end. When the information stops, the page stops.

## Words

Use <Product>'s own nouns, consistently. <Link the glossary page, for example
`reference/words-<product>-uses.md`, once it exists.>

<TODO: the product's vocabulary, and any words the team bans.>

Write paths, commands, file names and settings keys in backticks. Write the visible label of a
control in bold. An icon-only control has no label to bold, so name what it does instead.

## Formatting

- One `#` heading per page, matching the frontmatter `title`.
- `##` for sections, `###` for subsections. Never deeper.
- Sentence case in every heading.
- Paragraphs of one to four sentences.
- A bulleted list for things with no order. A numbered list only for steps in sequence.
- Code fences carry a language.
- Never write a date, a version number, a count, or anything else that will be wrong later.
  Say where the reader reads the current value.

## Frontmatter

```yaml
---
title: Export a report
summary: One sentence, under 120 characters, shown in lists and search results.
kind: how-to
sources: [Reports screen, Export dialog]
---
```

`kind` matches the directory. `sources` names what each fact was checked against, and the
reviewing session reads it. `summary` is a full sentence with a full stop.

## The checklist

Before a page is done:

- It is one kind, and its directory says which.
- Its frontmatter is complete and `kind` matches the directory.
- Every fact has been checked against the running product, a generated fragment or the code.
- Every control it names is bold and spelled as the screen spells it.
- It has no opening about itself and no summary at the end.
- Every link resolves.
