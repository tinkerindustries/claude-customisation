# The docs/ map and the smaller folders

This reference covers `docs/README.md`, which maps the whole tree, and the folders that need less than a reference of their own: `reference/`, `runbooks/`, `external/`, `archive/` and `DEPENDENCIES.md`.

## docs/README.md: the map

Once `docs/` holds more than a few folders, the next person to write a document picks a folder by guessing unless something tells them. `docs/README.md` is that thing. Seed it from `assets/docs-readme-template.md`.

It holds:

- one line pointing at the root `README.md` for the shape of the project
- a table of every folder: `Folder | Holds | Edited when`. The "Edited when" column carries most of the value, because it separates current-state folders from records at a glance
- a table of the documents at the root of `docs/` (or at the repo root), with what each covers
- one short section per confusable pair, saying how to choose between them: architecture and reference, reference and decisions, and any pair this repo has trouble with

List only folders that exist. The map carries no list of files inside the folders, because each folder's listing is the list and a copy goes stale.

## docs/reference/: one subsystem in full

A `reference/` document describes how one subsystem behaves now, for someone about to use or change it. Every statement in it is still true of the code. It changes in the same pull request as the code it describes.

`ARCHITECTURE.md` is the map, read to find where a change goes. A `reference/` document is read once the component is found. Open each `reference/` document with a line linking back to `ARCHITECTURE.md`, then the plan, design documents and decisions that relate to it.

Name files after the subsystem: `base-folder.md`, `peer-discovery.md`. A module `README.md` beside the code (the simple skill's `elsewhere.md`) still fits a subsystem that lives in one directory. A `reference/` document fits a subsystem that spans several.

The simple skill's staleness rules apply in full. Name modules and functions where a reader needs them to find the code, and leave out anything the code shows faster: signatures, file trees, counts.

## docs/runbooks/

The simple skill's `elsewhere.md` covers runbooks. In this structure a runbook is a procedure someone follows without the context in front of them: under pressure when something has gone wrong, or setting up a host by hand. It changes when the procedure changes. A runbook with images keeps them in a folder of the same name beside it.

## docs/external/: software the project does not own

A note under `external/` records how a tool, CLI, API or protocol the project depends on behaves, where that behaviour shapes how the project's code has to be written and is not in the other software's own docs.

- Open with what was inspected: the exact version, where it was installed, and which source each claim comes from (the binary's own output, its source at a tag, its docs).
- Mark claims taken only from docs, without checking the source or a running binary, as "docs only".
- A problem in someone else's software goes here only if it changes how this project's code is written. Otherwise it goes nowhere.
- The note changes when that software ships a change that matters. A version bump that changes nothing relevant needs no edit.

## docs/archive/: unmaintained but cited

`archive/` holds documents nobody maintains, kept because something still cites them. Nothing in it is edited.

`docs/archive/README.md` lists each document with one paragraph: what it was, why it stopped being current, what replaced it, and what in it is still worth reading. A document that needs correcting has become live again, and moves back to `reference/` or `decisions/`.

Create `archive/` only when the first document is retired. Plans have their own archive at `docs/plans/archive/` (the simple skill's `issues-and-plans.md`).

## DEPENDENCIES.md: how dependencies move

`DEPENDENCIES.md` says how every dependency is pinned, when pins move, and what proves an update. It sits beside `ARCHITECTURE.md` and the other root documents. It is warranted when pins span several ecosystems (a language's packages, a runtime, container images, vendored binaries, CI actions), or when the repo has a policy on update timing.

Sections that earn their place:

- the rules, in a short list at the top
- how each kind of pin is held (lockfile, version file, digest, vendored copy) and where
- sets that have to move together
- when things move: the cadence, and any wait before adopting a new release
- what proves an update: the commands to run
- adding a dependency: what to check first
- overrides and held-back versions, each with its reason or a link to its decision or advisory

Leave out an inventory of current versions. The manifests and lockfiles hold them, and a copy goes stale on the next update. Point at the files instead.
