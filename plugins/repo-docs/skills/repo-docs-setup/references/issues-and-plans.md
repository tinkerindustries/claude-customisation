# Issues and plans as files in the repository

The five core documents describe a repository as it is. Two kinds of knowledge don't fit that shape: a problem nobody is fixing yet, and work that hasn't been built yet. Both are usually held in a tracker — GitHub Issues, Linear, Jira — and both become expensive there once agents do the work, because an agent reaching a tracker needs a network call, a token, and a tool that may not be configured, and gets back something it cannot grep.

Keeping them as markdown in `docs/` makes them ordinary repository content: readable with `Glob` and `Grep`, editable in the same commit as the fix, reviewable in the same pull request, and branched with the code.

This reference covers both structures. They are independent — a repo can warrant one, both or neither.

## When this is warranted

Propose it when agents do a meaningful share of the work in the repo, and especially when more than one session works on the same codebase. The signals:

- A `CLAUDE.md` that already tells agents to write findings down somewhere
- Work that spans sessions — a feature too large for one context window, handed between runs
- Multiple worktrees or parallel sessions on the same repo
- An existing `docs/` folder with hand-written planning or issue notes accumulating in it
- The user describing problems that "get lost" between sessions

Don't propose it for a repo where a human does all the work and already has a tracker they like. Duplicating a tracker into markdown gets you two sources of truth and no ceremony for keeping them in step.

When the repo has a tracker *and* agents, ask which is authoritative. A common working split is that the tracker holds anything a user or stakeholder reported, and `docs/issues/` holds what agents find while working — but that is a decision for the user, not a default to apply silently.

## `docs/issues/`

```
docs/issues/
├── README.md          What belongs here, and how a file moves through
├── TEMPLATE.md        Copied to start a new issue
├── open/              One file per unresolved problem
├── closed/            The same files, after they're resolved
└── triage/            One dated report per triage pass
```

**One file per problem**, named after the problem in kebab case: `permission-dialog-loses-focus.md`. No numbers in filenames — two worktrees working in parallel will pick the same one, and the conflict surfaces as a confusing merge rather than an error.

**Closing moves the file**, `open/` to `closed/`, keeping the filename so links to it survive. A closed issue keeps its history and stays greppable; the location is the status. Fill in the resolution section as part of the same commit that fixes the problem.

**What belongs here** is a problem in this project's own behaviour that is not being fixed right now. Something being fixed in the next few minutes doesn't need a file. A problem in someone else's software belongs here only if it changes how this code has to be written.

**Triage is a pass, not a state.** A triage report is dated and records what that pass closed and how it ranked what stayed open. The issues it looked at carry a `Triaged` line, so the next pass can see what has already been weighed rather than re-deriving it. This only earns its keep once the open set is too large to read in one sitting; below that, skip `triage/` entirely.

The template is `assets/issue-template.md`. Every heading in it is required — writing "Unknown" under one is meaningful, because a missing heading and an unanswered question are indistinguishable otherwise.

## `docs/plans/`

```
docs/plans/
├── README.md          What a plan is, and what it is not
├── <slug>.md          One plan per feature
├── memory/<slug>.md   The facts every session on that feature shares
├── reports/<slug>/    What each run actually did
└── archive/           Plans whose phases have all landed
```

**Each feature gets a slug**, and the slug is the join key: the plan is `<slug>.md`, its shared facts are `memory/<slug>.md`, its reports are `reports/<slug>/`. Anything that needs to find the rest of a feature's material can derive the paths.

**A plan says what will be built, in what order, and what proves each step done.** It is written before the work and left as written afterwards. This is the part people get wrong — the instinct is to update the plan to match what was built, which destroys the record of what was intended and leaves a document that is neither a plan nor a description. What was actually built belongs in `reference/` or `ARCHITECTURE.md`; why it came out differently belongs in a decision record.

**A feature memory holds what every session working on the feature needs**: the schema it introduces, the invariants that hold across phases, and the files that matter with a sentence on why each one does. No code, no progress tracking, and small enough to load without crowding out the work. One writer — the session orchestrating the feature — with the sessions doing the phases suggesting additions in their reports. Two writers produce contradictions that every later session then has to resolve.

**Reports are per run and never updated.** One file per run, named `<yyyy-mm-dd>-<what-it-did>.md`. A report says what was built, what was verified and how, what was left undone, and what the next run needs to know. It is also where mechanical friction gets recorded — flaky tests, slow builds, a lint pass that takes four minutes — which is otherwise never written down anywhere, because it is nobody's feature and nobody's bug.

Findings in a report that outlive the run get moved out: durable behaviour into `reference/` or `ARCHITECTURE.md`, a choice and its reasoning into a decision record, a problem the run couldn't fix into `docs/issues/open/`. The report itself stays as the dated record.

**A plan whose phases have all landed moves to `archive/`**, and its memory to `archive/memory/`. Reports stay where they are. What's left in `plans/` is work that is planned, in flight or not started — which is what makes the folder listing worth reading.

Templates: `assets/plan-template.md`, `assets/memory-template.md`, `assets/report-template.md`.

## Phases and dependencies

The phase table is what lets separate sessions work on one feature without colliding. Each phase needs three things stated: what it depends on, what proves it done, and where the neighbouring phase's work starts.

The last of those is the one that gets skipped, and it's the one that prevents two agents building the same thing from opposite ends. "Out of scope for this phase" is not padding — it's the boundary that a second session reads to know what it must not touch.

Dependencies stated as phase numbers let an orchestrator work out what can run in parallel. A plan with no dependency column reduces to a sequential list, and the whole structure has bought nothing.

## Where this sits relative to the five documents

| Content | Home |
|---|---|
| A problem in this project not being fixed right now | `docs/issues/open/` |
| What that problem turned out to be, once fixed | The same file, in `docs/issues/closed/` |
| What will be built, in what order, and what proves it | `docs/plans/<slug>.md` |
| Facts every session on a feature needs | `docs/plans/memory/<slug>.md` |
| What one run actually did and what it left undone | `docs/plans/reports/<slug>/<date>-<what>.md` |
| What the feature does now that it's built | `ARCHITECTURE.md`, or a `reference/` document |
| Why it came out differently from the plan | A decision record |
| A procedure followed under pressure | `docs/runbooks/` |

The tense test settles most of it. A plan is future, a report is past, an issue is present-but-wrong, and the five core documents are present-and-correct.

## Wiring it into `CLAUDE.md`

The structure does nothing unless agents know to use it, and that costs a handful of lines in the always-loaded file. Keep it to the rule and the path, and let the folder's own `README.md` carry the rest:

```markdown
## Writing things down

A problem you are not fixing right now gets a file in `docs/issues/open/`, copied from
`docs/issues/TEMPLATE.md`. Name it after the problem, not the fix. Closing it means
moving it to `docs/issues/closed/` under the same name and filling in its resolution.

Work larger than a single session gets a plan — read `docs/plans/README.md` first.
```

Two things to resist. Don't inline the templates into `CLAUDE.md`; point at the file and let it be read when it's needed. Don't list the open issues anywhere — the folder listing is the list, and any copy of it is wrong within a week.

Add a `docs/README.md` mapping every folder to what it holds and when it gets edited, once `docs/` has more than about four things in it. Without it, the next person to write a document picks a folder by guessing, and the structure erodes from there.
