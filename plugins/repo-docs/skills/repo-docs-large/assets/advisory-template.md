# <package> <every affected version the tree carries>

## The advisories

| Id | What it is | Vulnerable range | Patched version |
|---|---|---|---|
| <GHSA-xxxx-xxxx-xxxx> | <one line> | <range> | <version, or none> |

## Where it comes from

The dependency path from this repo's manifest to the package, whether it is a dev dependency,
and whether any second path reaches it. Give the command that shows the path (`pnpm why`,
`npm explain`, `cargo tree -i`, `go mod why`) and the grep that confirms nothing else does.

## What the caller does with it

The function that reaches the vulnerable code, and the input it is handed. Say where that input
comes from and whether an attacker can control it.

## Remedies

- <A remedy: upgrade, override, replace, vendor a patch.> <Why it does or does not work, and
  what rejecting it costs.>

## Decision

What was decided.

Dismissal reason text, for the repository owner to paste when dismissing the alert:

> <Reason, quoting this file's path: docs/advisories/<package>.md>

## What would change this

- <A condition that makes this file wrong: a patched release, a new caller, the input becoming
  untrusted.>
