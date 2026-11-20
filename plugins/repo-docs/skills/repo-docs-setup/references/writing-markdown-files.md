# How to Write Markdown Files

These rules make a markdown file easy to navigate with `grep` and bounded reads, which is how an agent reads it. They help a person skimming the file too. Every file this skill writes follows them. Existing files are brought up to them only when the user opts in, as described in "Bringing existing files up to these rules" below.

## Headings

- Use ATX headings (`## Name`), one per line, with no skipped levels and no deeper than `###`. `grep -n '^#' file` then returns a complete outline with line numbers.
- Make each heading unique and put the searchable terms in it, such as the function, flag, or concept name. Repeated headings like "Overview", "Notes" or "Details" return many hits that can't be told apart.
- Don't number headings. Numbers go stale when sections move, and renumbering breaks references. A number that identifies something is fine: `### Phase 2` in a plan, `# 0007. Use Postgres` in an ADR. The rule covers outline numbering like `## 3.2 Setup`.
- Avoid `#` comment lines inside code fences. They match a `^#` grep and look like headings.

## Table of contents

- Keep it a flat list at the top, within the first 100 lines.
- Give each entry the exact heading text plus a short note on what the section answers. The note lets a reader choose a section without opening it.
- Leave out line numbers. They go stale on every edit, for the same reason as the brittle values in `SKILL.md`. Either generate the TOC with a script, or replace it with a one-line note giving the command (`grep -n '^#' file`).
- For files with many sections, a TOC with only the `##` level is enough. A `CLAUDE.md` under 200 lines doesn't need one.

## Section bodies

- Start each section with its conclusion or rule, then the detail. A partial read of the first few lines then still gives the answer.
- Make each section stand alone. Name the subject again instead of writing "as above" or "this". A `grep -A 20` hit lands mid-file with no earlier context.
- Keep sections to roughly 50 to 100 lines. A match plus a bounded read then covers the whole section.
- Refer to other sections by file path and heading text, not by line number.

## Tables and lists

- Use one row per item, with the lookup key in the first column. Grep returns whole lines, so each row has to make sense alone.
- Avoid multi-line cells and wide tables. Use a `key: value` bullet list when the values are long.
- Hard-wrapped paragraphs return as fragments when grep matches one line. Putting a sentence or a paragraph on one line returns more complete matches. The trade-off is harder diffs and less readable source.

## Splitting files

When a file passes a few hundred lines, split it by topic into descriptively named files and keep the index small. This is also Anthropic's stated preference for a tree of files over one large one, and it removes most of the need for a TOC.

## Where the repo's own conventions win

The repo's tooling overrides these rules where the two conflict.

- A markdown linter or formatter config (`.markdownlint*`, `.remarkrc*`, Prettier's `proseWrap`) decides heading style, line wrapping and list markers. Follow it.
- A docs site generator (MkDocs, Docusaurus, Sphinx, VitePress) may require frontmatter, a heading level, or a file layout. Its requirements come first.
- If the repo consistently hard-wraps prose, match it in files you write there. Mixed wrapping within one file is harder to read than either style.

## Bringing existing files up to these rules

The sweep is optional, and the user decides its scope. By default this skill applies the rules only to files it creates and to the sections it writes in files it edits. A repo with a lot of existing markdown can take a large diff from a full sweep, so ask before starting one.

### Files the sweep leaves alone

- Dated records: ADRs, run reports under `docs/plans/reports/`, files in `docs/issues/closed/`, `CHANGELOG.md`. Each one records a moment and isn't updated afterwards.
- Generated files: anything a tool writes, such as a release-please changelog or generated API docs. The next run of the tool overwrites the change. Fix the generator's template instead, if the user wants it fixed at all.
- Vendored and third-party content: `node_modules/`, `vendor/`, git submodules.
- Templates another tool parses (`.github/ISSUE_TEMPLATE/`, PR templates) keep whatever structure that tool requires.

### Inventory before changing anything

List the candidates with their sizes, then read each outline:

```bash
git ls-files '*.md' | xargs wc -l | sort -n
grep -n '^#' path/to/file.md
```

Show the user each file with the problems found in it: skipped heading levels, headings deeper than `###`, numbered headings, duplicate or generic headings, no TOC in a long file, length past a few hundred lines. Let them pick the files. Put first the files agents read most: every `CLAUDE.md`, the five core documents, and anything under `docs/` that `CLAUDE.md` links to.

### Order of changes in a sweep

Make the mechanical changes first and the prose changes last. Mechanical changes are easy to review, and prose changes need a careful read.

- Fix heading levels: convert setext headings to ATX, close skipped levels, and fold anything deeper than `###` into its parent section.
- Remove outline numbers from headings.
- Rename generic and duplicate headings. A rename changes the heading's anchor. Grep the repo for links to the old anchor (`file.md#old-anchor` and `](#old-anchor)`) and update them in the same change. Links from outside the repo (wikis, tracker comments, bookmarks) can't be found this way, so tell the user which anchors changed.
- Add a TOC to long files, or replace a stale one.
- Split files past a few hundred lines. Update every inbound link in the repo. Keep the original path as a short index pointing at the new files, so outside links still land somewhere useful.
- Rewrite section openings to lead with the rule, and replace "as above" and "this" with the subject's name. These change prose, so do them only in the files the user picked.

Don't reflow hard-wrapped paragraphs unless the user asks for it. A reflow touches every line, and the real changes get lost in the diff.

### The sweep restructures and keeps meaning intact

A sweep changes where content sits and how it's headed. It does not change what the content says. Stale numbers, historical narration and misplaced sections found during a sweep are triage findings: route them through the placement table in `SKILL.md` and list them for the user, separately from the formatting changes.

Suggest the user commit the sweep separately from any content moves, so a reviewer can tell restructuring apart from edits to what the documents say. For a large sweep, one commit per directory keeps each diff reviewable.
