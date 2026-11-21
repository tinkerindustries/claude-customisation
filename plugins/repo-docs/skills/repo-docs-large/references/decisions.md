# Decision records in docs/decisions/

`docs/decisions/` holds one file per decision: what was chosen, why, the paths turned down and what the choice costs. It is the one folder under `docs/` where history belongs. Every current-state document (`ARCHITECTURE.md`, `reference/`, `design/`, `CLAUDE.md`) states the rule and links here for the reason.

## Naming a decision file

Name the file after the decision, in kebab case, with no number: `no-formatter.md`, `claude-code-is-the-permission-layer.md`, `peer-link-pins-the-peer-key-in-tls.md`. Two worktrees adding a decision at the same time would pick the same number, and the clash surfaces as a merge conflict or a duplicate.

A name that states the decision is better than a name for its topic. `database-charts-use-vega-lite.md` tells a reader listing the folder what was decided. `charting.md` tells them only where to look.

## The title and first paragraph

The `#` title states the decision as a sentence or a noun phrase: "No formatter over the TypeScript", "Claude Code is the only permission layer". The first paragraph states the decision in the present tense, as the rule it sets. A reader who stops after that paragraph has the rule.

When the decision removed something, the first paragraph says what was removed and what holds now, and the body lists the details under "What was removed" and "What replaced it".

## Headings in a decision file

`assets/decision-template.md` carries the shape. Two headings are required in every file:

- **Why.** The reasons, as a list when there are several. Each reason names what made the alternative worse, with a measurement where one exists.
- **What it costs.** What the choice gives up and who pays. A decision with no cost listed reads as one nobody weighed.

The rest are used when they apply:

- **What was not chosen.** Each alternative and the reason it lost.
- **What was measured.** Numbers that support the decision, with how to rerun the measurement. Numbers are fine here because the file is a dated record.
- **What would reopen it.** The conditions that make the decision wrong. Someone rereading the file in a year needs to know what to recheck. Write this section whenever a condition is known.
- **What was removed** and **What replaced it**, for a removal.

Write "Unknown" under a required heading rather than dropping it. A decision reconstructed from a migration note often has no recorded reason. Its Why section says so and carries a TODO for a person to fill.

## Superseding a decision

A reversed decision keeps its body. Add one blockquote line at the top, above the title:

```markdown
> **Superseded** by [data-explorer-is-the-one-chart-renderer.md](data-explorer-is-the-one-chart-renderer.md).
> What follows describes the decision as it was made.
```

Write the new decision as its own file, and say in its first paragraph which file it supersedes. When part of the old record is still true, such as a measurement, the line says so: "What this record measured is still true."

Edit the old file's body only to fix an error in what it recorded. Its reasons were the reasons at the time.

## Linking a decision from where the rule is read

The rule lives where people read it, and the reason lives in the decision file. The link joins the two:

```markdown
- Don't reformat code you did not come to change
  ([docs/decisions/no-formatter.md](docs/decisions/no-formatter.md)).
```

The same applies in `ARCHITECTURE.md` invariants, `reference/` documents and code comments. A code comment that needs more than a sentence of reasoning links to a decision instead of carrying the reasoning itself.

## Departures from a plan or a design-phase record

When the product is built differently from what a plan or a design-phase record said, the reason is a decision. The plan and the record stay as written, and the decision file names the section it departs from. `references/design.md` covers the design side.

## Moving existing ADRs

A repo with a numbered `docs/adr/` keeps it unless the user agrees to move it. If they agree, rename each file to a slug that states its decision, keep the old number in neither the filename nor the title, and update inbound links in the same change. Links from outside the repo break on any rename, so list the old paths in the report.

## The folder's README

Write `docs/decisions/README.md` with: what a decision file holds, the naming rule, the two required headings, the superseding line, and the rule that this is the one folder where history belongs. Leave out an index of decisions. The folder listing is the index, and the filenames state the decisions.

## What a decision file is not

- A changelog. A change with no choice behind it is git history.
- A design document. What a screen looks like goes in `design/`. The decision file holds why it looks that way.
- A plan. Work not yet built goes in `plans/` or `features/`.
- An advisory. Accepting one security alert on one dependency has its own folder and shape (`references/advisories.md`).
