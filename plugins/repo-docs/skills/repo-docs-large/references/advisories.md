# Accepted advisories in docs/advisories/

`docs/advisories/` holds one file per dependency that carries a known security advisory the repository has decided not to remediate. Each file stands behind a dismissed alert (a Dependabot alert on GitHub, or the equivalent in the repo's scanner). The file is written before the alert is dismissed.

## What belongs in advisories/

An advisory reaches this folder only when nothing proportionate can be done about it. Either no patched release exists, or the fix sits behind an upgrade that is its own piece of work. And the vulnerable code is not reachable with input an attacker controls.

An advisory with an ordinary upgrade path does not belong here. Take the upgrade.

## Naming an advisory file

Name the file after the package, in kebab case: `minimatch.md`, `golang-x-crypto.md`. One file covers every advisory against that package, across every version the tree carries. The title names the package and the versions: `# minimatch 3.1.5, 5.1.9, 9.0.9 and 10.2.6`.

## What an advisory file has to answer

`assets/advisory-template.md` carries these headings. Every one is required. Write "Unknown" under a heading that cannot be answered.

- **The advisories.** Every GHSA or CVE id, what each one is, the vulnerable range, and whether a patched version exists.
- **Where it comes from.** The dependency path, whether it is a dev dependency, and whether a second path reaches it. The package manager's `why` command (`pnpm why`, `npm explain`, `cargo tree -i`, `go mod why`) starts the answer, and a grep of the installed tree finishes it.
- **What the caller does with it.** The function that reaches the vulnerable code, and the input it is handed. This decides the rest of the file: an advisory about a malicious archive matters as much as the archives being opened are untrusted.
- **Remedies.** Each remedy considered, and why it does or does not work. Include the rejected ones and what rejecting each costs.
- **Decision.** What was decided, with the exact text for the alert's dismissal comment. That text quotes the file's path.
- **What would change this.** The conditions that make the file wrong: a patched release, a new caller, the input becoming untrusted.

## Dismissing the alert

Writing the file does not dismiss the alert. The dismissal happens on GitHub or in the scanner, and it is the repository owner's to make. Write the reason text in the file and leave the dismissal to the owner. This skill never dismisses an alert.

## Rereading an advisory file

A file here rests on a dependency tree that keeps moving. Its "one path in" claim was true of the tree on the day it was written. Before trusting one, rerun the `why` command and the grep it describes, and check its "What would change this" list against the repo as it is now.

A dependency pass that touches the package rereads its file. When a condition on that list arrives, the decision is open again: take the upgrade if one now exists, or rewrite the file against the current tree.

## The folder's README

Write `docs/advisories/README.md` with: what belongs here (the conditions above), the list of questions each file answers, who dismisses the alert, and the rereading rule. Add one paragraph separating it from `docs/decisions/`: an advisory is a standing exception to one alert on one dependency, with a fixed shape and an expiry condition.
