# init-project

A Claude Skill that initializes a Vibcoding (AI-driven development) project environment. It does **not** write any source code or install any dependencies — instead, it scaffolds the **documentation and specification layer** that an AI agent needs in subsequent development phases.

## Overview

`init-project` runs a short interview with the user to collect basic project information (name, language, tech stack, project type, etc.), then automatically generates a standardized set of project documents and an AI memory system, including:

- **CLAUDE.md** — AI behavior rules (auto-loaded into context every session; the most critical file)
- **README.md** — Project entry document
- **ROADMAP.md** — Development roadmap
- **.gitignore** — Universal ignore rules
- **memory/** — AI memory system (MEMORY.md, user-preferences.md, decisions.md)

## When It Triggers

Claude invokes this skill automatically when the user expresses any of the following intents:

- "init project" / "initialize project"
- "scaffold" / "set up project"
- "Phase 1"
- Starting a brand-new project from scratch

## Directory Structure

```
init-project/
├── SKILL.md                    # Main instruction file (read by AI)
├── README.md                   # This file (English documentation, default)
├── README_CN.md                # Chinese documentation
├── scripts/
│   └── init-memory.sh          # Creates the AI memory directory and files
├── references/
│   ├── claude-template.md      # CLAUDE.md template (with placeholders)
│   ├── readme-template.md      # README.md template (with placeholders)
│   ├── roadmap-template.md     # ROADMAP.md template (with placeholders)
│   └── vibecoding-template.md  # VIBECODING.md template
└── assets/
    └── gitignore_template      # Universal .gitignore template
```

## Workflow

1. **Interview the user** — Use `AskUserQuestion` to gather the project name, one-line description, language, tech stack, and project type. Optional fields include Git strategy, development mode, and emoji preference.
2. **Generate core documents** — Read templates from `references/`, replace `{{PLACEHOLDER}}` values with user input, then write to the project root.
3. **Create the memory system** — Run `bash scripts/init-memory.sh` to initialize the `memory/` directory.
4. **Optional Git initialization** — If version control is enabled, run `git init` and make the first commit; if a remote URL is provided, also link and push.

## Placeholder Mapping

| Placeholder | Source |
|---|---|
| `{{PROJECT_NAME}}` | User-provided project name |
| `{{PROJECT_DESCRIPTION}}` | One-line description |
| `{{LANGUAGE}}` | Programming language |
| `{{TECH_STACK}}` | Full tech stack |
| `{{PROJECT_TYPE}}` | CLI tool / Web app / Library / Mobile app / etc. |
| `{{CODE_STANDARDS}}` | Code standards mapped by language |
| `{{TEST_STANDARDS}}` | Test standards mapped by language |
| `{{PROJECT_ROOT}}` | Project directory name |

### Language → Standards Mapping (Excerpt)

| Language | Code Standards | Test Standards |
|---|---|---|
| Python | Google-style docstring + PEP 484 type hints | pytest, coverage > 80% |
| TypeScript / JavaScript | ESLint + Prettier, strict mode | Vitest / Jest, coverage > 80% |
| Java | Javadoc + Spring Boot conventions | JUnit 5, coverage > 80% |
| Go | gofmt + standard project layout | go test, coverage > 80% |
| Rust | rustfmt + clippy linting | cargo test |
| Other | Community standard conventions | Standard test framework for the language |

For the complete mapping, see [SKILL.md](file:///c:/Users/YV/.claude/skills/init-project/SKILL.md).

## Required Tools

This skill depends on the following AI tool capabilities:

- **Write** — Create documents
- **Edit** — Adjust content
- **Read** — Load templates
- **AskUserQuestion** — User interview
- **Bash** — Execute scripts and Git commands

## What This Skill Does NOT Do

To keep responsibilities focused, this skill explicitly avoids:

- Creating any source code files
- Installing dependencies or toolchains
- Configuring build systems
- Creating framework-specific files
- Configuring IDEs or editors

These belong to later development phases (Phase 2 and beyond). This skill only scaffolds the **specification and documentation layer** that an AI agent needs to work effectively.

## Important Rules

1. Do **not** use emoji unless the user explicitly requests it
2. Template content lives in `references/`, never hardcoded in the main instruction file
3. User preferences are recorded in `memory/user-preferences.md`
4. Key architectural decisions (with dates) are recorded in `memory/decisions.md`
5. `CLAUDE.md` is the core file — it governs all subsequent AI behavior
6. **Vibcoding configuration placement**: All vibcoding-related configurations (Claude command whitelists, hooks, skills, subagent definitions, etc.) MUST be placed inside the `.claude/` directory at the project root. Global configuration should ONLY be used when the user explicitly requests it.

---

For the Chinese version, see [README_CN.md](file:///c:/Users/YV/.claude/skills/init-project/README_CN.md).
