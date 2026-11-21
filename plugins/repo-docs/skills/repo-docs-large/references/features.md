# Feature requests in docs/features/

`docs/features/` holds one file per thing the product should be able to do and cannot. It mirrors `docs/issues/` from the simple skill: an open request lives in `open/`, and moves to `closed/` when it ships or is turned down. The file keeps its name so links to it survive, and its Outcome section is filled in as it closes.

```
docs/features/
├── README.md       What belongs here, and how a request moves through
├── TEMPLATE.md     Copied to start a new request
├── open/           One file per request nobody is building yet
└── closed/         The same files, once shipped, planned or turned down
```

## What belongs in features/

Something the product should do and does not, while nobody is building it.

- A defect (the product doing the wrong thing with what it already has) goes in `docs/issues/`.
- Work someone is starting in the next few minutes needs no file.
- Once a request is scoped and split into phases it gets a plan under `docs/plans/`. The request links to the plan and closes, with the plan as its Outcome.

The separation from issues matters when agents triage. A bug list with requests mixed in gets ranked as if every entry were broken, and requests sink below real bugs or crowd them out.

## Naming a request

Name the file after the capability, in kebab case: `android-userland-ships-python.md`, `phone-composers-offer-the-suggested-next-message.md`. No numbers, for the same reason as issues. The title states what the product should be able to do, in one line.

## The template

Copy `assets/feature-template.md` to `docs/features/TEMPLATE.md`. Replace the **Area** values with this repo's own components, taken from `ARCHITECTURE.md`'s codemap. Leave the headings alone. Every heading is required, and "Unknown" under one is a real answer: a missing heading and an unanswered question look the same otherwise.

The metadata lines:

- **Status**: open or closed.
- **Size**: small (a session), medium (a few sessions) or large (a plan under `docs/plans/`).
- **Area**: one of the repo's components.
- **Raised**: the date the request was written.
- **Triaged**: the date and band of the last triage pass that looked at it (now, next, someday, won't). Omit it until a pass has looked.

The sections: what is missing, what it should do, why, where it lands, how to do it, what would make it wrong, notes, outcome.

"What would make it wrong" carries the constraints that decide the shape: a platform limit, an invariant it must not break, a cost that rules out an approach. It is the section a later planning session reads first.

## Triage

Triage of features works like triage of issues in the simple skill's `issues-and-plans.md`. The **Triaged** line records the last pass, so the next pass sees what has been weighed. A `triage/` folder with dated reports earns its place only when the open set is too large to read in one sitting.

## The folder's README

Write `docs/features/README.md` with: the open and closed rule, the naming rule, the template rule, what belongs here, and the two boundaries (issues for defects, plans for scoped work).
