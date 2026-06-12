---
name: init-project
description: >
  Initialize a Vibcoding (AI-driven development) environment for any project.
  This skill sets up the documentation and specification system that an AI agent
  needs during development. It asks the user about their project (language,
  framework, architecture), then creates the project documentation skeleton
  including agent behavior rules (CLAUDE.md), project specification (README.md),
  development roadmap (ROADMAP.md), and
  AI memory system (memory/). Use this when the user says "init project",
  "initialize project", "scaffold", "set up project", "Phase 1", or when
  starting a new project from scratch.
compatibility:
  - Write
  - Edit
  - AskUserQuestion
  - Bash
  - Read
---

# init-project: Initialize Vibcoding Project Environment

## Skill Structure

```
init-project/
├── SKILL.md                    # This file - AI instructions
├── scripts/
│   └── init-memory.sh          # Create AI memory directory and files
├── references/
│   ├── claude-template.md      # CLAUDE.md template with placeholders
│   ├── readme-template.md      # README.md template with placeholders
│   ├── roadmap-template.md     # ROADMAP.md template with placeholders
│   └── vibecoding-template.md  # VIBECODING.md template
└── assets/
    └── gitignore_template      # Universal .gitignore template
```

**How to use bundled files:**
- Read references/*.md to get document templates, replace placeholders ({{PLACEHOLDER}}), then Write to project
- Run scripts/init-memory.sh via Bash to create memory system
- Copy assets/gitignore_template content when creating .gitignore

## Overview

This skill sets up the documentation and specification infrastructure that an AI
agent relies on when developing a project in Vibcoding mode (AI-driven
development with human oversight). It does NOT create any code or install any
dependencies -- those are handled by later phases.

## Step 1: Interview the User

Start by asking the user about their project. Use AskUserQuestion to gather the
following information. Group related questions together (2-3 per AskUserQuestion
call) rather than asking one at a time.

### Required questions

| Question | Purpose |
|---|---|
| Project name | Used for directory name and all docs |
| Project description (one line) | Used in README and documentation |
| Programming language(s) | Primary language for the project |
| Tech stack / framework | e.g. FastAPI + React, Spring Boot + Vue, etc. |
| Project type | CLI tool, Web app, Library, Mobile app, etc. |

### Optional questions (if user has clear preferences)

| Question | Default |
|---|---|
| Git strategy | Local only / GitHub / GitLab |
| Development mode | AI-driven (Vibcoding) / Traditional |
| Use emoji in code/docs | No |

If the user is unsure about any optional question, use the default and proceed.

Record all answers in memory/user-preferences.md.

## Step 2: Create Core Documents

Read the template files from references/, replace {{PLACEHOLDER}} values with
user's answers, then Write the result to the project root.

### 2a. CLAUDE.md (AI Behavior Rules)

Read `references/claude-template.md`, replace placeholders, Write to `<project-dir>/CLAUDE.md`.

This is the most important file -- it is automatically loaded into the AI's
context at the start of every session. It governs all subsequent AI behavior.

**Placeholder mapping:**

| Placeholder | Source |
|---|---|
| {{PROJECT_NAME}} | User's project name |
| {{PROJECT_DESCRIPTION}} | User's description |
| {{LANGUAGE}} | Programming language(s) |
| {{TECH_STACK}} | Full tech stack |
| {{PROJECT_TYPE}} | CLI / Web / Library / etc. |
| {{CODE_STANDARDS}} | Language-specific conventions (see mapping table below) |
| {{TEST_STANDARDS}} | Testing framework recommendations |
| {{PROJECT_ROOT}} | Project directory name |

**Language to code standards mapping:**

| Language | Code Standards | Test Standards |
|---|---|---|
| Python | Google-style docstring, PEP 484 type annotations | pytest, coverage > 80% |
| TypeScript / JavaScript | ESLint + Prettier, strict mode | Vitest / Jest, coverage > 80% |
| Java | Javadoc, Spring Boot conventions | JUnit 5, coverage > 80% |
| Go | gofmt, standard project layout | go test, coverage > 80% |
| Rust | rustfmt, clippy linting | cargo test |
| C / C++ | clang-format, CMake conventions | Google Test / CTest |
| Ruby | Rubocop, YARD docs | RSpec |
| PHP | PSR-12, PHPDoc | PHPUnit |
| Swift | SwiftLint, SwiftDoc | XCTest |
| Kotlin | ktlint, Dokka | JUnit 5 |
| Other / Unknown | Follow community standard conventions | Standard testing framework for the language |

### 2b. README.md (Project Entry)

Read `references/readme-template.md`, replace placeholders, Write to `<project-dir>/README.md`.

Note: Do NOT use emoji unless the user explicitly requested it.

### 2c. ROADMAP.md (Development Roadmap)

Read `references/roadmap-template.md`, replace placeholders. If the user has
described their vision for phases, customize the phases. Otherwise use the
default template. Write to `<project-dir>/ROADMAP.md`.

### 2d. .gitignore

Copy `assets/gitignore_template` content to `<project-dir>/.gitignore`.
Use Write to create the file.

## Step 3: Create AI Memory System

Run the memory initialization script:

```bash
bash scripts/init-memory.sh <project-dir> <project-name> "<tech-stack>"
```

This creates:
- `memory/MEMORY.md` -- Index file
- `memory/user-preferences.md` -- User preferences with YAML frontmatter
- `memory/decisions.md` -- Architecture decisions with YAML frontmatter

## Step 4: Initialize Git (Optional)

If the user wants version control:

```bash
cd <project-dir>
git init
git add -A
git commit -m "Initial project setup: documentation and specifications"
```

If they provided a remote URL:

```bash
git remote add origin <url>
git branch -M main
git push -u origin main
```

## What This Skill Does NOT Do

This skill intentionally does NOT:
- Create any source code files
- Install any dependencies or tools
- Set up build configurations
- Create framework-specific files
- Configure IDEs or editors

Those are handled in subsequent development phases. This skill only creates the
specification and documentation layer that an AI agent needs to work effectively.

## Important Rules

1. Do NOT use emoji unless the user explicitly requests it
2. Read reference template files instead of hardcoding templates in SKILL.md
3. Record user preferences in memory/user-preferences.md
4. Record key decisions in memory/decisions.md with dates
5. Use TaskCreate to track progress during execution
6. The CLAUDE.md file is critical -- it governs all subsequent AI behavior
7. **Vibcoding configuration placement**: All vibcoding-related configurations, including but not limited to Claude command whitelists, hooks, skills, subagent definitions, and other AI agent tooling, MUST be placed inside the `.claude/` directory at the project root. Global configuration (e.g., `%USERPROFILE%\.claude\` on Windows or `~/.claude/` on Unix) should ONLY be used when the user explicitly requests it. This ensures per-project isolation and portability.
