# <Project> documentation

Start at [../README.md](../README.md) for the shape of the project.

## What each folder is for

<One row per folder that exists. Delete the rest.>

| Folder | Holds | Edited when |
|---|---|---|
| [decisions/](decisions/) | Why a choice was made, the paths turned down and what it costs | When a decision is made or reversed |
| [design/](design/README.md) | The current design: each platform's design language and the surfaces that have a document | In the pull request that changes what the document describes |
| [design-phases/](design-phases/README.md) | What each design phase settled before its build, kept as written | Once, by the design phase. Never after it closes |
| [help/](help/STYLE.md) | The user-facing help, written to `help/STYLE.md` | When a surface a page names changes |
| [plans/](plans/README.md) | Plans for work larger than one session | While the work is being planned |
| [plans/memory/](plans/memory/) | One small file per feature with the facts every session on it shares | By the session orchestrating that feature |
| [plans/reports/](plans/reports/) | What each run of a plan did | Never. Each run writes a new one |
| [plans/archive/](plans/archive/) | Plans whose phases have all landed, with their memories | Never, once a plan is moved here |
| [reference/](reference/) | How a subsystem behaves now | Whenever the code it describes moves |
| [issues/](issues/README.md) | Known problems, one file each, open and closed | As a problem is found, worked and closed |
| [features/](features/README.md) | Things the product should do and cannot, one file each, open and closed | As a request is raised, planned and closed |
| [advisories/](advisories/README.md) | One file per dependency with a known advisory the repo has decided not to remediate | Before the alert is dismissed, and when its condition for revisiting arrives |
| [reports/](reports/README.md) | Dated investigations that belong to no plan | Once, when the investigation is written |
| [runbooks/](runbooks/) | Procedures followed without the context in front of you | When the procedure changes |
| [external/](external/) | Notes on software the project depends on and does not own | When that software ships a change that matters |
| [archive/](archive/README.md) | Documents no longer maintained, kept because something cites them | Never |

## The documents beside the folders

| Document | Covers |
|---|---|
| [ARCHITECTURE.md](<path>) | The codemap, how the pieces relate, and the invariants between them |
| [TESTING.md](<path>) | What each test layer is for and when to run what |
| [RELEASE.md](<path>) | Versioning and cutting a release |
| [HOSTING.md](<path>) | Where the product runs, how it is deployed and how to roll it back |
| [DEPENDENCIES.md](<path>) | How dependencies are pinned, when they move and what proves an update |

## Choosing between architecture and reference

`ARCHITECTURE.md` is the map: what each component is for, what it may depend on, and what must
hold across all of them. It is read to find where a change goes. A `reference/` document is one
subsystem in full, read once the component is found.

## Choosing between reference and decisions

A `reference/` document answers "what does this do". Everything in it is still true of the code.
A `decisions/` document answers "why is it like this". It may describe a path not taken, and it is
the only place in `docs/` where history belongs.
