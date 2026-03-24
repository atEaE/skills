---
name: hands-on-plan-creator
description: A skill that creates step-by-step hands-on building plans for learning purposes. Use this skill when the user wants to build something from scratch to deepen their understanding — phrases like "○○を作りたい", "○○を一から作ってみたい", "○○を△言語で実装してみたい", "build X from scratch", "implement my own X", "recreate X for learning", or "○○を○○で置き換えてみたい". This is NOT for production implementation planning (that's plan mode) — this is for educational, hands-on projects where the user learns by building. Trigger this skill even if the user doesn't say "hands-on" explicitly, as long as the intent is clearly about learning through building.
---

# Hands-on Plan Creator

Create concrete, actionable building plans for people who want to learn by making things. The output is a set of markdown files that guide the learner step-by-step through building a project from scratch.

The key difference from plan mode: plan mode designs an implementation strategy for shipping software. This skill designs a **learning journey** — each step teaches something, builds on the previous one, and results in working code the learner wrote themselves.

## When This Skill Activates

Someone says they want to build, recreate, or implement something — typically for learning:

- "Gitを一から作ってみたい"
- "Dockerを自分で実装してみたい"
- "RustでHTTPサーバーを作りたい"
- "SQLiteを一から作って仕組みを理解したい"
- "build my own Redis in Go"
- "I want to understand how React works by building a mini version"

## Process

### 1. Understand What the Learner Wants to Build

Before generating anything, clarify:

- **What to build**: The target system or tool (e.g., "Git", "HTTP server", "key-value store")
- **Language/tech**: What language or technology they want to use. If not specified, ask — the choice affects every step.
- **Scope**: How deep do they want to go? A minimal working version, or something more complete? If unclear, default to a minimal but functional version and note where they could go deeper.
- **Prior knowledge**: Gauge what they already know. Are they learning the language too, or just the domain? This shapes how much background each step needs.

If the user's initial message already answers these clearly, skip the questions and proceed.

### 2. Research the Domain

Before writing the plan, understand the system being built:

- Break the target system into its core concepts and components
- Identify which features are essential for a minimal working version vs. nice-to-have
- Determine the natural learning order — what needs to be understood before what
- Think about what will give the learner the most "aha moments" early on

### 3. Design the Step Sequence

Each step corresponds to a **feature** the learner will implement. Each step also corresponds to a **feature branch** in git, so steps should be self-contained units of work that result in something demonstrable.

Guiding principles for step design:

- **Early reward**: The first step should produce something visible or testable as quickly as possible. A learner who sees output on screen in step 1 stays motivated.
- **Build on previous steps**: Each step should extend what was built before, not replace it. The learner should feel their codebase growing organically.
- **One concept per step**: Don't cram multiple new ideas into one step. If a feature requires understanding two new concepts, consider splitting it.
- **Testable milestones**: Each step should end with a way to verify it works — a command to run, output to expect, or behavior to observe.

### 4. Generate Output Files

Generate the following files in the current working directory:

#### ROADMAP.md

The roadmap is the learner's home base — a high-level overview with progress tracking.

Structure:

```markdown
# [Project Name] - Hands-on Building Roadmap

## Overview
[1-2 sentences: what we're building and why it's a valuable learning exercise]

## Tech Stack
- Language: [language]
- [any other relevant tools/libraries]

## Prerequisites
- [What the learner should already know]

## Steps

- [ ] **STEP 1: [title]** - [one-line summary of what gets built]
- [ ] **STEP 2: [title]** - [one-line summary]
- [ ] **STEP 3: [title]** - [one-line summary]
...

## Branch Strategy
Each step corresponds to a feature branch:
- STEP 1 → `step-1/[short-description]`
- STEP 2 → `step-2/[short-description]`
...
```

#### STEP_n.md (one per step)

Each step file is a self-contained guide for implementing one feature.

Structure:

```markdown
# STEP n: [Title]

## Goal
[What the learner will build in this step and what they'll understand by the end]

## Branch
`step-n/[short-description]`

## Background
[Explain the concepts needed for this step. Not a textbook — focus on what's directly relevant to the implementation. Use diagrams (ASCII art) or examples where they help clarify. If the learner already knows the language well, keep this brief; if they're also learning the language, include relevant language concepts here.]

## Implementation

### n.1 [First sub-task]
[Concrete instructions: what file to create/edit, what code to write, and — importantly — *why* this code works the way it does. Include code snippets where they clarify intent, but don't provide complete copy-paste solutions unless the code is boilerplate. The learner should be writing the code themselves.]

### n.2 [Next sub-task]
...

## Verify
[How to confirm this step works. Specific commands to run, expected output, or behavior to check. Make this concrete — "run `./mygit hash-object test.txt` and you should see a 40-character hex string".]
```

### Writing Guidelines

- **Explain the "why"**: Don't just say "implement X". Explain why X is needed and how it fits into the bigger picture. This is what separates a learning plan from a task list.
- **Use the learner's language**: If they asked in Japanese, write in Japanese. If in English, write in English. Match their communication style.
- **Be concrete**: "Create a file called `store.go` in the `internal/` directory" is better than "create a storage module". Specific file names, specific commands, specific expected outputs.
- **Don't over-scaffold**: Give enough guidance that the learner isn't stuck, but leave enough room that they're actually thinking and coding. Strike a balance — more guidance for beginners, less for experienced developers learning a new domain.
- **Keep steps roughly even in size**: If one step would take 3 hours and another 15 minutes, consider rebalancing. The learner should feel steady progress.
