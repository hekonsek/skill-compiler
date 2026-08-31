---
name: skill-compiler
description: Compile an agent-optimized SKILL.md from source material in a src directory, including ADRs, documentation, instructions, source code, snippets, schemas, and data format descriptions. Use when asked to "compile a skill", create a skill from source content, convert human-friendly guidance into an Agent Skills SKILL.md, or distill examples and project knowledge into reusable agent instructions.
---

# Skill compiler

Compile source material into a concise, portable `SKILL.md` that follows the [Agent Skills specification](https://skill.md) and is optimized for any compatible agent to use after activation.

## Input

Treat source material as authoritative when it includes:

- Architecture Decision Records, design notes, and technical rationale
- Human-facing instructions, runbooks, checklists, and policies
- Source code, tests, examples, snippets, and configuration
- Data format descriptions, schemas, fixtures, and API contracts
- Existing README files, or product documentation

Ignore generated dependencies, build outputs, lockfile noise, vendored code, caches, binary blobs, and unrelated project metadata unless the source material explicitly depends on them.

When sources conflict, prefer in this order:

1. Explicit user instructions for the current compilation task
2. ADRs and decision records
3. Tests, examples, schemas, and executable behavior
4. README files and general documentation
5. Inferred conventions from code structure

If a conflict materially changes the compiled skill, either encode the higher-priority rule or ask the user to resolve it. Do not silently merge incompatible instructions.

When source code is the main input, derive instructions from observable behavior, public APIs, tests, CLI commands, configuration, and error handling. Avoid documenting private implementation details unless agents need them to perform the skill.

When ADRs are the main input, preserve accepted decisions, constraints, consequences, and rejected alternatives only when they guide future agent behavior.

When schemas or data formats are the main input, include field semantics, required/optional rules, validation behavior, examples, and compatibility constraints that agents need to produce or edit valid data.


## Output

Produce an agent-agnostic skill. Follow the Agent Skills specification at <https://skill.md>. Treat specification compliance and cross-agent portability as required output properties, not optional style preferences. When an environment-specific dependency is essential, state it explicitly and isolate it from the portable workflow; use the specification's `compatibility` field when appropriate.

Prefer flat `SKILL.md` files over multi-file skills unless the complexity of the source material justifies it.

The compiled skill should be:

- Portable across skill-compatible agents
- Self-contained enough to be useful after activation
- Specific enough to change agent behavior
- Short enough to avoid wasting context
- Grounded in the source material rather than generic best practices
- Clear about required files, commands, validation, and expected outputs

## Workflow

1. Inventory the source directory before writing. Identify file types, likely primary documents, code examples, and repeated themes.
2. Read the most relevant source files first. For large sources, search for headings, decision statements, examples, constraints, "must", "should", "when", "avoid", "workflow", and "validation".
3. Extract the skill's intended capability, activation contexts, required workflow, constraints, validation steps, and reusable patterns.
4. Distill human-friendly prose into direct agent instructions. Preserve operational rules and domain-specific details; remove background narrative, duplicated rationale, and generic advice an agent already knows.
5. Infer a skill name only when the source clearly supports it. Otherwise use the current directory name. Normalize the frontmatter `name` to lowercase letters, digits, and hyphens.
6. Write or update `SKILL.md` at the skill root unless the user requests a different output path.
7. Validate the compiled file against the current Agent Skills specification at <https://skill.md> and reread it for usefulness as standalone, agent-agnostic instructions.
