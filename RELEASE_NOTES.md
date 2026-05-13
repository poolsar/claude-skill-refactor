# Release Notes

## 2026-05-13 — Initial release

### Skill

- Two-phase workflow: research-first (produces `REFACTORING.md` with numbered recommendations),
  then implement only what the user approves.
- Anti-over-engineering guard built into both phases: recommendations are omitted unless the
  benefit clearly outweighs the overhead.
- Safe Refactoring Mode based on Michael Feathers' *Working Effectively with Legacy Code*:
  characterization tests pin current behaviour before any source file is touched; refactoring
  happens inside that safety net. Activated persistently per-project via a `.refactor-safe-mode`
  marker file.
- Complexity assessment in Phase 1 determines whether to recommend Safe Refactoring Mode.

### Principles covered

- **DRY** — Don't Repeat Yourself
- **KISS** — Keep It Simple, Stupid
- **YAGNI** — You Ain't Gonna Need It
- **SOLID** — SRP, OCP, LSP, ISP, DIP (detailed in `references/solid.md`)
- **Clean Code** — names, functions, comments, formatting, objects, error handling, classes
  (detailed in `references/clean_code.md`)
- **Clean Architecture** — component cohesion (REP, CCP, CRP), component coupling (ADP, SDP, SAP),
  Dependency Rule (detailed in `references/clean_architecture.md`)

### Reference files

- `references/solid.md`
- `references/clean_code.md`
- `references/clean_architecture.md`
- `references/safe_refactoring.md`
