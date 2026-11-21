---
name: repo-docs-large
description: Sets up the full docs structure for a large repo where agents do much of the work. It covers everything repo-docs-simple does (the five core files, docs/issues/, docs/plans/) plus docs/decisions/ for why choices were made, design/ for the current UI design, design-phases/ for frozen design records, features/ for things the product cannot do yet, advisories/ for dependency alerts the repo accepts, help/ for user help written to a style guide and reviewed by a second session, reports/ for dated investigations, and reference/, runbooks/, external/, archive/. Use it when the user asks for the large or full docs structure, names any of those folders, wants a home for why decisions were made, wants current design docs beside frozen design records, wants feature requests kept apart from bugs, or needs to justify a dismissed Dependabot alert. For the five core files plus issues and plans alone, use repo-docs-simple.
allowed-tools: Read, Glob, Grep, Bash, Write, Edit, AskUserQuestion
metadata:
  category: documentation
---

# Large repository documentation setup

This skill sets up everything `repo-docs-simple` sets up, then a `docs/` tree with a folder for each remaining kind of knowledge. The structure comes from a repository where agents write most of the code across many parallel sessions. Each folder answers one question that kept landing in the wrong document: why a choice was made, what a screen looks like now, what a design phase settled, what the product cannot do yet, why a security alert was dismissed, how a user does something, and what one investigation found.

## How this skill builds on repo-docs-simple

Read `../repo-docs-simple/SKILL.md` first and follow its workflow from start to finish. Its references are in `../repo-docs-simple/references/` and its templates in `../repo-docs-simple/assets/`. This file adds to each of its steps and overrides it in the places listed under "Overrides to repo-docs-simple". The paths are relative to this skill's directory, and both skills ship in the same plugin. If `../repo-docs-simple/SKILL.md` is missing, tell the user and stop.

The simple skill's sourcing rule applies to every folder here without change. Every command, path and claim traces to something read in the repository, and a gap is marked TODO. That rule matters most for decisions: a decision file written from a one-line migration note has a decision and no known reason, and the reason stays a TODO until someone supplies it.

## The folders

| Folder | Holds | Edited when | Reference |
|---|---|---|---|
| `docs/README.md` | The map: every folder, what it holds and when it is edited | When a folder is added or its purpose changes | `references/docs-map-and-small-folders.md` |
| `docs/decisions/` | Why a choice was made, the paths turned down and what it costs. The one folder where history belongs | When a decision is made or reversed | `references/decisions.md` |
| `docs/design/` | The current design: each platform's design language and one document per surface that needs one | In the pull request that changes what the document describes | `references/design.md` |
| `docs/design-phases/` | What a design phase settled before its build, kept as written | Once, by the design phase | `references/design.md` |
| `docs/features/` | Things the product should do and cannot, one file each, open and closed | As a request is raised, planned and closed | `references/features.md` |
| `docs/advisories/` | One file per dependency carrying a known advisory the repo has decided not to remediate | Before the alert is dismissed, and when its condition for revisiting arrives | `references/advisories.md` |
| `docs/help/` | User-facing help in four kinds, written to a style guide, each page reviewed by a second session | When a surface a page names changes | `references/help.md` |
| `docs/reports/` | Dated investigations that belong to no plan | Once, when the investigation is written | `references/reports.md` |
| `docs/reference/` | How one subsystem behaves now, in full | Whenever the code it describes moves | `references/docs-map-and-small-folders.md` |
| `docs/runbooks/` | A procedure someone follows without the context in front of them | When the procedure changes | `references/docs-map-and-small-folders.md` |
| `docs/external/` | Notes on software the project depends on and does not own | When that software ships a change | `references/docs-map-and-small-folders.md` |
| `docs/archive/` | Documents nobody maintains, kept because something cites them | Never | `references/docs-map-and-small-folders.md` |
| `DEPENDENCIES.md` | How dependencies are pinned, when they move and what proves an update | When the policy changes | `references/docs-map-and-small-folders.md` |

`docs/issues/` and `docs/plans/` come from the simple skill (`../repo-docs-simple/references/issues-and-plans.md`).

## Telling the folders apart

Route a piece of content by the question its reader brings.

| The reader's question | Home |
|---|---|
| Where does this change go, and what may this component depend on? | `ARCHITECTURE.md` |
| How does this one subsystem work, in full? | `docs/reference/<subsystem>.md` |
| Why is it like this, and what was turned down? | `docs/decisions/<decision>.md` |
| What does this screen look like now, and what rules does it follow? | `docs/design/` |
| What did the design phase settle before the build? | `docs/design-phases/<record>.md` |
| What is going wrong with what the product already has? | `docs/issues/open/` |
| What should the product do that it cannot do yet? | `docs/features/open/` |
| What will be built, in what order, and what proves it done? | `docs/plans/<slug>.md` |
| What did one run of a plan do? | `docs/plans/reports/<slug>/` |
| What did one investigation outside any plan find? | `docs/reports/` |
| Why is this security alert dismissed, and when should it be rechecked? | `docs/advisories/<package>.md` |
| How does a user of the product do something? | `docs/help/` |
| What does someone do when a system is down, or when setting up a host by hand? | `docs/runbooks/` |
| How does a tool the project does not own behave? | `docs/external/` |

The folders split into two groups by how they treat time.

- **Current state.** `ARCHITECTURE.md`, `reference/`, `design/`, `help/`, `runbooks/` and `DEPENDENCIES.md` say what is true now. They change in the same pull request as the code they describe. They carry no history and link to `decisions/` for reasons.
- **Records.** `decisions/`, `design-phases/`, `reports/`, `plans/reports/` and `archive/` keep what they recorded. A record is dated by its filename or its git history. A record is never rewritten to match what was built later. A decision is the one exception: a reversed decision gains a "Superseded" line at its top.

The pairs that get confused most often:

- `ARCHITECTURE.md` and `reference/`. `ARCHITECTURE.md` is the map, read to find where a change goes. A `reference/` document covers one subsystem in full, read once the component is found. An `ARCHITECTURE.md` entry that outgrows a paragraph becomes a `reference/` document and the entry links to it.
- `reference/` and `decisions/`. A `reference/` document answers "what does this do", and every statement in it is still true of the code. A decision answers "why is it like this", and may describe paths not taken.
- `design/` and `design-phases/`. `design/` is the app as it looks now. A design-phase record is what one phase settled before its build. When the build lands, `design/` is updated to match what was built and the record stays as written.
- `issues/`, `features/` and `plans/`. An issue is the product doing the wrong thing with what it already has, seen going wrong. A feature request is something it should do and does not, while nobody is building it. A plan is a feature that has been scoped into phases. When a feature request gets a plan, the request links to the plan and closes.
- `advisories/` and `decisions/`. An advisory is a standing exception to one security alert on one dependency, with a fixed shape and a condition that expires it. Anything broader is a decision.
- `reports/` and `plans/reports/`. A plan's report records one run of that plan. A report under `docs/reports/` is an investigation that belongs to no plan: a regression hunt, a flake triage, a feasibility study, a dependency pass.
- `help/` and everything else. Help is written for a person using the product who has not read the source. A help page that names a file under the source tree belongs in `ARCHITECTURE.md` or `reference/` instead.

## Which folders this repo warrants

The simple skill's rule holds here: propose a folder only when the repo has something real for it. A folder whose `README.md` says what belongs there is worth creating on the first day when the kind of work it holds is already happening, because the README tells the next writer where the first file goes. A folder for work the repo never does is a stub.

| Folder | Warranted when | Skip when |
|---|---|---|
| `decisions/` | Any repo this skill is used on. Signals: an `adr/` folder, "we chose X because" paragraphs in other docs, history the triage step pulled out of `CLAUDE.md` | Never skipped. Start it with the decisions the triage finds |
| `design/` | The product has a UI with rules the code cannot show: tokens, wording, hard-to-reach states, platform conventions | No UI, or a UI fully described by a component library's own docs |
| `design-phases/` | UI work is designed in a phase of its own before it is built, by a separate session or person | Design happens inline with the build |
| `features/` | Requests accumulate that nobody is building yet, or agents keep finding gaps while working | A tracker holds requests and the user wants it to stay authoritative |
| `advisories/` | Dependency alerts are reviewed (Dependabot, `npm audit`, GHSA) and some get dismissed | No dependency alerting is set up |
| `help/` | The product has users who are not its developers, and help ships in the product or on a site | A library or internal tool whose README is the help |
| `reports/` | Investigations get written up: flake triage, audits, feasibility studies, regression attribution | Nothing is investigated outside a plan |
| `reference/` | `ARCHITECTURE.md` entries have outgrown a paragraph, or subsystems have behaviour a reader needs before changing them | The codemap is enough |
| `runbooks/` | See `../repo-docs-simple/references/elsewhere.md` | |
| `external/` | The code depends on behaviour of software it does not own, and that behaviour is not in the other software's docs | The project uses its dependencies as documented |
| `archive/` | A document is retired while something still cites it | Never create it empty |
| `DEPENDENCIES.md` | Pins span several ecosystems, or there is a policy on when versions move and what proves an update | One lockfile and a bot that updates it |

When the user asked for "everything", still say which folders the repo has nothing for and ask before creating them. `help/` and `advisories/` depend on facts outside the repo (whether the product has end users, whether alerts are dismissed), so ask about those rather than concluding from absence.

## Overrides to repo-docs-simple

- **Decision records go in `docs/decisions/<decision>.md`**, named in kebab case after the decision, with no number. This replaces every mention of `docs/adr/NNNN-*.md` in the simple skill's `SKILL.md`, `references/elsewhere.md`, `references/architecture-md.md` and `references/claude-md.md`. Numbered filenames collide when two worktrees each add one, for the same reason issue files carry no number. If the repo already has a numbered `docs/adr/`, keep it and ask before renaming, because links from outside the repo point at those paths.
- **A past decision that still constrains today's work** is stated in `ARCHITECTURE.md` (or `CLAUDE.md` when repo-wide) as a one-line rule with a link to its decision file. The reasons live only in the decision file.
- **An issue is a bug someone saw go wrong.** A check a run could not do, a measurement nobody took, or a fault found only by reading the code goes in the pull request body or the run's report. Something the product should do and does not goes in `docs/features/`. Write this into `docs/issues/README.md` alongside the simple skill's guidance.
- **Findings that outlive a run** go to `docs/reference/` for behaviour and `docs/decisions/` for reasons. The simple skill's `issues-and-plans.md` names these destinations generically.
- **A plan's design phase** writes its output to `docs/design-phases/`, and the phase that builds the surface updates `docs/design/`. Add this to `docs/plans/README.md` when both folders exist.
- **Where the five core documents sit.** Keep them where the repo already has them. In a new setup the simple skill puts them at the root. They may sit in `docs/` beside the folders that link to them if the user prefers. `CLAUDE.md` stays at the root in every case.

## Additions to each step of the workflow

### Additions to the survey

Look for evidence of each folder's kind of work, in the repo and in its existing docs:

- **Decisions**: `docs/adr/`, `doc/architecture/decisions/`, `decisions/`; paragraphs in `ARCHITECTURE.md`, `CLAUDE.md` or a README that explain why something was chosen or what it replaced.
- **Design**: design tokens, a component catalogue (Storybook, a debug catalogue screen, render scripts), SVG or PNG mockups, copy or strings files, links to design tools, one app per platform (`android/`, `ios/`, a desktop shell).
- **Design phases**: plans with a phase that produces a design ahead of the build; design documents that describe a surface before its code exists.
- **Features**: `TODO.md`, `ROADMAP.md`, `ideas.md`, wishlists, "future work" sections, a tracker label for enhancements.
- **Advisories**: `.github/dependabot.yml`, audit steps in CI, `overrides`/`resolutions` blocks in package manifests with no comment explaining them. If `gh` is authenticated, `gh api repos/{owner}/{repo}/dependabot/alerts --paginate -q '.[] | select(.state=="dismissed") | [.number, .dependency.package.name, .dismissed_reason, .dismissed_comment] | @tsv'` lists dismissed alerts. Each one with no written reason is a candidate file.
- **Help**: `docs/help/`, a docs site config (`mkdocs.yml`, `docusaurus.config.*`, `astro.config.*` with Starlight, `book.toml`), code that loads help pages into the product, user-facing how-to sections in a README.
- **Reports**: files with a date in the name, or named "investigation", "audit", "spike", "feasibility", "postmortem", "triage", loose in `docs/` or the root.
- **Reference**: long per-subsystem sections in `ARCHITECTURE.md`, module READMEs that describe behaviour across several directories.
- **External**: notes on a third-party CLI, API or protocol and how it behaves in practice.
- **Archive candidates**: documents describing an approach the code no longer takes, still linked from elsewhere.

### Additions to deciding which files

Decide the folders with the table under "Which folders this repo warrants", after the simple skill decides the five files and issues and plans.

### Additions to the triage

Classify every existing document under `docs/`, not only `CLAUDE.md`, against "Telling the folders apart". The usual moves:

- A "we chose X because" paragraph in `ARCHITECTURE.md`, `reference/` or `design/` moves to a decision file. The current rule stays where it was, in one line with a link.
- A design document edited after its build, with notes on what changed, splits in two: the current design goes to `design/`, and the version as first settled goes to `design-phases/` if its original text can be recovered from git history. Ask before splitting.
- Entries in a TODO or wishlist file become one file each under `features/open/`. A bug list becomes one file each under `issues/open/`.
- Dated investigation notes move to `reports/`.
- User-facing how-to material in a README or `ARCHITECTURE.md` is a candidate for `help/`. Moving it is part of the help pipeline, so propose it there.
- A document about a superseded approach that something still links to moves to `archive/`, with a line in `archive/README.md` saying why it is kept.

Moving a file changes its path. Find inbound links with `grep -rn '<old-name>' --include='*.md' .` and in code comments, and update them in the same change.

### Additions to the proposal

Add to the proposal the simple skill describes:

- which folders you will create, which you are skipping, and why
- what existing content moves into each folder, file by file
- which decision files you will write from history found in the triage, and which of them have no recorded reason
- whether to set up the help pipeline (`STYLE.md`, `REVIEW.md`, briefs). It is the largest single piece, so ask about it separately

### Additions to the writing

Read each folder's reference as you write it. Write each folder's `README.md` in the repo's voice: what belongs there, what does not and where that goes instead, how a file is named, and how it moves through. Copy the templates from `assets/` and adapt only the parts that name this repo's components and platforms:

- `assets/decision-template.md` for a decision
- `assets/feature-template.md` for `docs/features/TEMPLATE.md`
- `assets/advisory-template.md` for an advisory
- `assets/design-phase-record-template.md` for a design-phase record
- `assets/investigation-report-template.md` for a report under `docs/reports/`
- `assets/help-style-template.md`, `assets/help-review-template.md` and `assets/help-brief-template.md` for `docs/help/STYLE.md`, `docs/help/REVIEW.md` and `docs/help/briefs/TEMPLATE.md`
- `assets/docs-readme-template.md` for `docs/README.md`

Write `docs/README.md` last, so it lists only folders that exist. Do not write help pages, design documents or reference documents beyond what the existing docs already hold. Each needs checking against the running product or reading the code in depth, which is its own piece of work. Say so in the report.

### The CLAUDE.md lines

The folders do nothing unless agents know to use them. Add one short section to the root `CLAUDE.md` that names a folder for each kind of thing an agent is likely to write, and links the map. Include a line only for folders that exist:

```markdown
## Writing docs and raising work

- Anything longer than a sentence goes in `docs/`. A decision and its reasons go in
  `docs/decisions/`, a component's shape in ARCHITECTURE.md, a bug you are leaving in
  `docs/issues/open/`, and something the product cannot do yet in `docs/features/open/`.
- Copy the folder's `TEMPLATE.md` and keep every heading. Name the file in kebab case,
  with no number.
- Work bigger than one session gets a plan in phases ([docs/plans/README.md](docs/plans/README.md)).
- [docs/README.md](docs/README.md) says which folder anything else goes in.
```

Use plain links. An `@`-import of `docs/README.md` loads the map into every session.

### Additions to the report

Add to the simple skill's report:

- the folders created and the folders skipped, with the reason for each
- every file moved, old path to new path, and the inbound links updated
- every decision file whose reasons are a TODO
- the pieces left for later work: help pages, design documents, `reference/` documents, a help linter
