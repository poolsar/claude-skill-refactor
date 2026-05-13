# claude-skill-refactor — Claude Code context

## What this repo is

A **Claude Code skill** — a structured set of instructions that Claude Code loads on demand
to guide systematic code refactoring. The skill is not a runnable program; it is Markdown
that Claude reads and follows.

## How Claude Code skills work

Skills use a three-level loading system:

1. **Metadata** (`name` + `description` in SKILL.md frontmatter) — always in context.
   This is the triggering mechanism: Claude reads the description and decides whether to
   consult the skill. Keep it accurate and specific.

2. **SKILL.md body** — loaded into context when the skill triggers. Contains the full
   workflow and the principles index. Should stay under ~500 lines.

3. **Reference files** (`references/*.md`) — loaded on demand as needed during execution.
   Unlimited size. SKILL.md points to them with explicit instructions about when to read each.

The skill lives in `~/.claude/skills/refactor/` on the user's machine. Claude Code scans
that directory at startup and makes the skill available.

## File layout

```
SKILL.md                          — main skill: triggers, workflow, principles index
references/
  solid.md                        — SOLID principles in full detail (SRP, OCP, LSP, ISP, DIP)
  clean_code.md                   — Clean Code principles in full detail (7 principle groups)
  clean_architecture.md           — Component cohesion/coupling + Dependency Rule
                                    (SOLID section cross-references solid.md, not duplicated)
  safe_refactoring.md             — Safe Refactoring Mode: characterization test workflow
                                    based on Feathers' Working Effectively with Legacy Code
CLAUDE.md                         — this file
```

## Skill workflow (summary)

The skill follows a two-phase workflow:

**Phase 1 — Research:** Claude reads the codebase, iterates every principle and
sub-principle, and writes `REFACTORING.md` in the target project root with numbered
recommendations. No source files are touched.

**Phase 2 — Implement:** User approves specific recommendations. Claude implements
only those. If `.refactor-safe-mode` exists in the project root, Safe Refactoring Mode
is active and Claude follows `references/safe_refactoring.md` for each change.

## Installing locally

Copy (or symlink) this directory to `~/.claude/skills/refactor/`. Claude Code picks it
up automatically on next startup — no config changes needed.

```powershell
# Windows — copy
Copy-Item -Recurse ".\refactor" "$env:USERPROFILE\.claude\skills\refactor"

# Or clone directly into the skills directory
git clone https://github.com/poolsar/claude-skill-refactor "$env:USERPROFILE\.claude\skills\refactor"
```

## Development conventions

- **Don't duplicate content between files.** If a principle is detailed in a reference
  file, SKILL.md gets only a short summary and a pointer. The reference file is the
  single source of truth for that content.
- **Cross-reference with relative paths** (`references/solid.md`, not absolute paths).
- **Keep SKILL.md under ~500 lines.** If it grows beyond that, move detail into a new
  reference file and add a pointer.
- **Principle entries in SKILL.md follow a fixed shape:**
  - One-line summary quote
  - Short description of what to check for
  - Any important caveat
  - Pointer to reference file (if one exists)
- **Reference files are comprehensive.** Each sub-principle gets its own named section,
  a "Check for:" list, and an "Apply by:" or caveat note. The goal is that Claude can
  act on each sub-principle independently without needing additional context.
- **safe_refactoring.md is self-contained.** It must be readable as a standalone
  step-by-step guide without assuming the reader has SKILL.md in context.

## Testing the skill

Open any software project in Claude Code and say:

> "refactor this project" or "do a DRY pass on the codebase"

Claude should load the skill, read the codebase, and produce a `REFACTORING.md` file
with numbered recommendations — without modifying any source file.

To test Safe Refactoring Mode:
1. Create an empty `.refactor-safe-mode` file in the project root.
2. Tell Claude to implement a recommendation.
3. Claude should follow the characterization-test workflow from `references/safe_refactoring.md`
   before touching any source file.

## Planned extensions

- Language-specific reference files (e.g., `references/python.md`, `references/typescript.md`)
  for idioms and conventions that go beyond language-agnostic principles.
- A `references/naming.md` deep-dive on naming strategies across different code elements.
