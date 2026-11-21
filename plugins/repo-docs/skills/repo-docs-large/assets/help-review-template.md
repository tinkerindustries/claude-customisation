# How to review a help page

A page is written by one session and reviewed by another. The reviewing session did not write
the page and has not read the writing session's reasoning. It reads the page, the page's brief
and [STYLE.md](STYLE.md), and it reports.

The reviewer does not edit the page. It reports findings, and the writing session applies them.
A page lands when a review comes back with no findings.

## The two roles

The writing session is given:

- the product running, so every claim can be checked against the real screen
- [STYLE.md](STYLE.md), in full
- the exemplar page for the page's kind, under `exemplars/`
- the brief, `briefs/<kind>/<slug>.md`
- the generated fragments under `reference/generated/`

It returns the page with complete frontmatter, lint-clean if a linter exists. After each review
it applies every finding and returns the page again. It never invents a fact the brief did not
ask for. A brief that is missing something goes back to whoever wrote it.

The reviewing session is given the finished page, the same brief, this file, the exemplar, and
the running product and code. It is never given the writing session's transcript or reasoning.
A reviewer that has not seen how the page was written catches the assumptions the writer could
not see it was making.

## Before reading the page

Read, in this order:

1. [STYLE.md](STYLE.md), in full.
2. The exemplar page for this kind, named in the brief.
3. The page's brief.

## The tests

Apply every test. A finding names the test, quotes the text, and says what is wrong. Do not
suggest replacement prose.

### Kind

- A reference page carrying `you`, an imperative verb or a numbered step is a finding.
- An explanation page carrying a numbered step is a finding.
- A tutorial offering a choice between two paths is a finding.
- A how-to guide explaining why, beyond one clause with a link, is a finding.
- A tutorial or how-to guide restating a field list that a reference page holds is a finding.

### Facts

- A claim with no source behind it is a finding, whether or not it is true.
- A number, date, version or count written into the prose is a finding.
- A bold control label that does not match the screen is a finding.
- A path, command or setting key that does not exist is a finding.

Check at least five claims against their source yourself. Say in the report which five you
checked and what you found.

### Completeness

- A reference page missing a field, value or default its surface has is a finding.
- A how-to guide with an unstated prerequisite is a finding.
- A tutorial step with no observable result is a finding.
- A page that ends before the reader's question is answered is a finding.

### Sentences

- A sentence over the ceiling in STYLE.md is a finding.
- A sentence with no actor, or passive voice where an actor exists, is a finding.
- `we`, `the user`, `one` or `let's` is a finding.
- Hedging, a filler opener, a sentence about the page, or a closing summary is a finding.

### Words

- A banned word from STYLE.md is a finding.
- A product term used to mean something else is a finding.
- A path, command or settings key not in backticks is a finding.
- A control label not in bold is a finding.

## The report

```markdown
# Review: <section>/<slug>

Verdict: PASS | REWRITE

## Claims checked

1. <claim> against <source>: <what was found>

## Findings

- <test>: "<quoted text>". <What is wrong.>
```

PASS means no findings. Any finding means REWRITE.
