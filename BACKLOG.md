# Backlog

Planned improvements, not yet implemented. Move items to `RELEASE_NOTES.md` with a
`YYYY-MM-DD` timestamp when done.

---

## Skill structure

- **Extract principles index to a reference file.**
  The principles section in `SKILL.md` (DRY, KISS, YAGNI, SOLID, Clean Code, Clean
  Architecture) is growing and may push the file toward the ~500-line limit. Consider
  moving the full principles index to `references/principles.md` and replacing it in
  `SKILL.md` with a short pointer. Evaluate whether this hurts or helps Claude's ability
  to apply the principles without an extra read step.

## New reference files

- **Language-specific idioms** — `references/python.md`, `references/typescript.md`, etc.
  Conventions and patterns that go beyond language-agnostic principles (e.g., Pythonic
  naming, TypeScript type narrowing hygiene, Go error handling idioms).

- **Naming deep-dive** — `references/naming.md`.
  Expanded treatment of naming strategies: domain vs. solution vocabulary, naming boolean
  variables and functions, naming collections, avoiding noise words, evolving names as
  understanding grows.

## Workflow improvements

- **Incremental mode** — allow the user to scope Phase 1 to a single file or directory
  rather than the whole codebase. Useful for large projects where a full scan is expensive.

- **Recommendation templates** — standardise the format of common recommendation types
  (e.g., "extract duplicated logic", "rename misleading identifier") so output is more
  consistent across runs.
