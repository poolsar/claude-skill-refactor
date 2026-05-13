# refactor — Claude Code Skill

A Claude Code skill for systematic, principle-driven code refactoring. Its central mission
is **fighting complexity** — making code easier to read, understand, and maintain for both
human developers and Claude Code itself.

## How it works

The skill follows a safe, two-phase workflow:

**Phase 1 — Research (no code changes)**
Claude reads the codebase, applies every refactoring principle to it, and writes a
`REFACTORING.md` file in your project root with numbered, prioritised recommendations.
Nothing in your source code is touched.

**Phase 2 — Implement (only what you approve)**
You review `REFACTORING.md` and tell Claude which recommendations to implement — by number,
by principle, or all of them. Claude implements only those, then deletes `REFACTORING.md`.

## Installation

Clone directly into your Claude Code skills directory:

```powershell
# Windows
git clone https://github.com/poolsar/claude-skill-refactor "$env:USERPROFILE\.claude\skills\refactor"
```

```bash
# macOS / Linux
git clone https://github.com/poolsar/claude-skill-refactor ~/.claude/skills/refactor
```

Restart Claude Code. The skill is picked up automatically — no config changes needed.

## Usage

Say any of the following in Claude Code:

```
refactor this project
clean up the code
do a DRY pass on the codebase
apply best practices
this module is getting hard to follow — can you review it?
```

Claude will research the code and produce `REFACTORING.md`. Then tell it what to implement:

```
implement R1, R3, and R5
implement all DRY recommendations
implement everything
skip the SOLID ones, do the rest
```

## Safe Refactoring Mode

For large codebases or code with sparse test coverage, the skill recommends enabling
**Safe Refactoring Mode** — a disciplined workflow based on Michael Feathers'
*Working Effectively with Legacy Code*.

In safe mode, before changing any file Claude:
1. Identifies the exact scope of the change
2. Writes *characterization tests* that pin the current behaviour of the code being changed
3. Confirms the tests are green on the original code
4. Makes the refactoring
5. Confirms the tests are still green — proof that behaviour was preserved

**Enable it permanently for a project:**
```
enable safe refactoring mode
```
Claude creates a `.refactor-safe-mode` marker file in your project root. Every future
refactoring session in that project will use safe mode automatically — across conversations.

**Disable it:**
```
disable safe refactoring mode
```
Or just delete `.refactor-safe-mode` manually.

**Skip for one session only:**
```
implement without safe mode
```

## Principles applied

| Principle | Scope |
|-----------|-------|
| **DRY** — Don't Repeat Yourself | Duplicated logic, constants, data structures |
| **KISS** — Keep It Simple, Stupid | Unnecessary complexity, clever code, over-abstraction |
| **YAGNI** — You Ain't Gonna Need It | Dead code, speculative generality, unused extension points |
| **SOLID** | Class and module design (SRP, OCP, LSP, ISP, DIP) |
| **Clean Code** | Names, functions, comments, formatting, error handling, classes |
| **Clean Architecture** | Component cohesion/coupling, dependency direction, layering |

Every principle has detailed sub-principles in the `references/` directory. Claude iterates
each one against the code and reports only findings where the benefit clearly outweighs
the cost. Over-engineering is explicitly guarded against.

## File structure

```
SKILL.md                       — skill entry point: workflow + principles index
references/
  solid.md                     — SOLID principles in full detail
  clean_code.md                — Clean Code principles in full detail
  clean_architecture.md        — Component and architecture principles
  safe_refactoring.md          — Safe Refactoring Mode step-by-step workflow
```
