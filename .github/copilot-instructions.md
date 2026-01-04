# Copilot Instructions for OpenCode Skills

You are an expert developer working on the OpenCode Skills repository. This project focuses on creating modular, self-contained "Skills" that extend AI capabilities with specialized knowledge and workflows.

## Big Picture Architecture
- **Skills**: Located in `skills/<skill-name>/`. Each skill is a directory containing a mandatory `SKILL.md` and optional resource folders (`scripts/`, `references/`, `assets/`).
- **OpenCode Ecosystem**: Skills are loaded on-demand by agents. The `SKILL.md` frontmatter (name and description) is the primary discovery mechanism.
- **Progressive Disclosure**: Keep `SKILL.md` lean (<500 lines). Move detailed documentation to `references/`, automation to `scripts/`, and templates to `assets/`.
- **Runtime**: The project uses **Bun** (see `bun.lock`) for JavaScript/TypeScript tasks, though skills themselves often use Python for automation.

## Critical Developer Workflows
- **Initialize Skill**: `python skills/skill-creator/scripts/init_skill.py <skill-name> --path skills`
- **Validate Skill**: `python skills/skill-creator/scripts/quick_validate.py skills/<skill-name>`
- **Package Skill**: `python skills/skill-creator/scripts/package_skill.py skills/<skill-name>`
- **Testing**: Test scripts in `scripts/` by running them directly. Ensure `SKILL.md` instructions are actionable and clear.

## Project-Specific Conventions
- **Naming**: Skill names must be `hyphen-case` (lowercase alphanumeric + hyphens).
- **Frontmatter**: `SKILL.md` must start with YAML frontmatter containing `name` and `description`.
- **Descriptions**: Must be action-oriented and include specific triggers/contexts (e.g., "Use when...").
- **Unreal Engine**: When developing UE-related skills (like `icon-generator` or `web-ui-ux`), follow UE C++ standards and UMG best practices.
- **Memory Bank**: Respect the memory-bank system (e.g., `activeContext.md`, `progress.md`) if present in the environment to maintain state across sessions.

## Taming Copilot Rules
- **Be Concise**: Avoid verbose explanations. Prefer code examples and direct instructions.
- **Actionable**: Every instruction should lead to a clear outcome.
- **No Fluff**: Do not include "how-to" guides for the AI itself; focus on the project's domain.
- **Context Awareness**: Always check `AGENTS.md` and `skills/skill-creator/SKILL.md` for the latest standards.

## Key Files to Reference
- [skills/AGENTS.md](../skills/AGENTS.md): Global development guidelines.
- [skills/skill-creator/SKILL.md](../skills/skill-creator/SKILL.md): Detailed guide on skill anatomy and design patterns.
- [skills/skill-creator/scripts/init_skill.py](../skills/skill-creator/scripts/init_skill.py): Template for new skills.
