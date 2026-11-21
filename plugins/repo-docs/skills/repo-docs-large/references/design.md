# The current design and design-phase records

Two folders hold design. `docs/design/` is what the product looks like now and how a person uses it. `docs/design-phases/` is what each design phase settled before its build, kept as it was written. The split keeps each document honest. A record edited after its build is neither the intent nor the current state, and a current design document carrying critic rounds and rejected alternatives buries the rules a builder needs.

## The flow between plans, design phases and the design

1. A plan for UI work gets a design phase ahead of the phase that builds the surface (`docs/plans/<slug>.md`).
2. The design phase writes its record to `docs/design-phases/<record>.md`. Until the build lands, the record is the intent.
3. The build phase builds the surface and updates `docs/design/` to match what it built, in the same pull request.
4. Where the build departs from the record, the reason goes in `docs/decisions/`. The record stays as written.

Add the first three steps to `docs/plans/README.md` when both folders exist.

## docs/design/: the current design

A statement in `docs/design/` describes the product as it is. When the product changes, the document changes with it, in the same pull request.

The code is the authority on behaviour. A design document says what the code cannot show:

- the rules a screen follows (ordering, grouping, when something is drawn and when it is not)
- the states that are hard to reach (empty, failed, offline, refused, a slow peer)
- the words, as a table of what each state says
- where the rule lives in code, so a reader can check it
- links to the reasons in `decisions/` and to the behaviour underneath in `reference/`

A design document carries no history: no critic rounds, no alternatives turned down, no account of what changed. Those go in `decisions/` and in the design-phase record.

When the repo has a catalogue that renders screens from the code (Storybook, a debug catalogue screen, a render script), a design document points at the catalogue entry. A screenshot or mockup pasted into `design/` goes stale on the next change to the screen.

### Hubs per platform

A product with more than one platform (desktop, Android, iPhone, web) gets one hub document per platform holding its design language, plus a folder of surface documents beside it. The hub holds the principles, the foundations (type, space, colour, shape, motion, icons), the component kit and how a new screen is built from it, and an index of the folder's surface documents.

Rules that two or more platforms share (the words a page says, which group an item sorts into) go in one shared document. Each platform's own document for that surface links to the shared one. Write the shared rule once and point at the code that enforces it, such as a shared module or a set of port fixtures.

### The design README

`docs/design/README.md` opens with the rules above in a few sentences: describes the product as it is, changes in the same pull request, carries no history. Then it indexes the documents in tables of `Document | What it covers`, grouped by platform and then by shared surfaces.

## docs/design-phases/: frozen records

Each document is the output of one design phase: what the phase settled before the build, written against the code of its day. A record is kept as it was written and is not edited once its phase closes. It keeps the critic rounds, the contradictions it found and the alternatives it turned down, because those are what the phase did.

### The banner every record opens with

```markdown
> This is a design-phase record, kept as it was written. It settled a design before the build,
> against the code of its day. [The current design](../design/README.md) says what the product
> looks like now, [the reference](../reference/) how it works, and [the decisions](../decisions/) why.
```

The banner stops a reader, or an agent landing on the file from a grep, treating the record as current.

### What a record contains

`assets/design-phase-record-template.md` carries the shape:

- the banner
- one paragraph on what the record settles, and which plan and phase it was settled ahead of
- the vocabulary it uses, when the feature has a memory file that defines terms
- one section per surface or screen, with every state
- **What was not designed**: the boundary, so the build phase knows what it must not invent
- **Decided here, because the plan left it open**: choices the phase made that the plan did not specify
- the contradictions it found with other documents or the code

Mockups (SVG or PNG) sit beside the record, named after it. Scripts that draw them go in a `lib/` folder in `design-phases/`.

### The design-phases README

`docs/design-phases/README.md` states the freezing rule and the banner's three links, then indexes the records in a table of `Phase | Document | Settles`. The Phase column links the plan and names the phase. A record asked for directly, with no plan, says so in that column.

## Departures from a record

A departure's reason goes in `docs/decisions/`, in a file that quotes the record's section and says how the product differs and why. Avoid a separate departures log inside `design-phases/`. A log becomes a second home for decisions, and a reader searching `decisions/` misses it.

## When to set these folders up

`design/` is warranted when the product has a UI whose rules the code cannot show. `design-phases/` is warranted only when design happens in a phase of its own before the build. A repo where design happens inline with the build gets `design/` alone, and its design reasons go straight into `decisions/`.

Do not write design documents from screenshots or from reading components. A design document states rules and states that a person has settled. Set up the folder, its README and the hub structure, move existing design content in, and list the surfaces that have no document as follow-up work.
