# Investigation reports in docs/reports/

`docs/reports/` holds dated investigations that belong to no plan: tracing a set of regressions back to the changes that caused them, triaging flaky tests, a feasibility study, a security assessment, a dependency pass, a review of the onboarding flow. A report is written once, when the investigation finishes, and is never updated afterwards.

A plan's run reports go in `docs/plans/reports/<slug>/` (the simple skill's `issues-and-plans.md`). A report here answers a question nobody planned a feature around.

## Naming a report

`<yyyy-mm-dd>-<what-it-investigated>.md`: `2026-09-13-regression-attribution.md`, `2026-09-23-iphone-app-feasibility.md`. The date is the day the investigation finished. Dates belong in this folder because a report is a record of one moment.

An investigation that recurs gets a subfolder named for its kind, with dated files inside: `reports/flake/2026-10-29.md`, `reports/dependencies/2026-10-23.md`, `reports/security/2026-11-02-phase-1-automated-baseline.md`. The listing of the subfolder is the history of that kind of pass.

A report with images or data keeps them in a folder with the report's name, beside it: `2026-11-14-onboarding-review-macos.md` and `2026-11-14-onboarding-review-macos/`.

## What a report contains

`assets/investigation-report-template.md` carries the shape. The parts that make a report useful a month later:

- **Scope, first.** What was read, over what window, at which commit. "The failing tests in the test ledger over the three days to 2026-10-29, read at `616eef49d`." A reader needs this to know whether the report still applies.
- **How it was run.** The commands, scripts or steps, so a later pass can repeat it and compare.
- **The answer, early.** A summary table or a paragraph before the detail. A reader with one question reads the top and stops.
- **Evidence beside each finding.** Every verdict rests on something in the report: a commit, a log line, a measurement. Keep what the instrument showed apart from what it is taken to mean.
- **What was not covered, and what is estimated.** The limits of the evidence, stated as their own section.
- **What it filed.** Links to the issues and features the report opened or updated. Findings that outlive the report move out: behaviour to `reference/`, reasons to `decisions/`, problems to `issues/open/`. The report stays as the dated record.
- **For the next pass**, when the investigation recurs. What the next run should check first, and what this run could not.
- **For the owner to decide**, when the report ends in a choice someone has to make. Options with their costs and a recommendation.

A later pass on the same question writes a new report and links the previous one, under a heading such as "Against the previous review". It never edits the earlier report.

## The folder's README

Write `docs/reports/README.md` with: what belongs here, the naming rule and the subfolders that exist, the never-updated rule, and the boundary with `docs/plans/reports/`.
