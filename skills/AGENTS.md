# AGENTS.md - OpenCode Skills Development Guide

## Overview

This document provides guidelines for developing skills in the OpenCode ecosystem. Skills are reusable instruction sets defined in `SKILL.md` files that agents can load on-demand to accomplish specific tasks.

## Skill Structure & Discovery

### File Organization
Skills are placed in specific directories for automatic discovery:

```
.opencode/skill/<skill-name>/SKILL.md          # Project-local (highest priority)
~/.config/opencode/skill/<skill-name>/SKILL.md # Global config
.claude/skills/<skill-name>/SKILL.md           # Claude-compatible project
~/.claude/skills/<skill-name>/SKILL.md         # Claude-compatible global
```

### SKILL.md Format
Each skill file must start with YAML frontmatter:

```yaml
---
name: skill-name
description: What this skill does and when to use it (1-1024 characters)
license: MIT  # optional
compatibility: opencode  # optional
metadata:  # optional key-value pairs
  audience: developers
  workflow: coding
---

## What I do
- Accomplishment 1
- Accomplishment 2
- Accomplishment 3

## When to use me
Describe when agents should use this skill.
Ask clarifying questions if needed.

## Instructions
Step-by-step guidance in Markdown format.

Reference supporting files: `scripts/helper.py` or `docs/api.md`
```

## Naming & Validation Rules

### Name Requirements
- **Format**: `^[a-z0-9]+(-[a-z0-9]+)*$` (lowercase alphanumeric with single hyphens)
- **Length**: 1-64 characters
- **Uniqueness**: Must be unique across all discovery locations
- **Matching**: Directory name must exactly match `name` field

### Description Guidelines
- **Length**: 1-1024 characters
- **Content**: Specific enough for agents to choose correctly
- **Action-oriented**: Start with verbs describing what the skill accomplishes

## Usage & Permissions

### Accessing Skills
Agents use the `skill` tool to load skills on-demand:

```
skill({ name: "code-review" })
```

### Permission Control
Configure access in `opencode.json`:

```json
{
  "permission": {
    "skill": {
      "code-review": "allow",
      "internal-*": "deny",
      "experimental-*": "ask",
      "*": "allow"
    }
  }
}
```

**Permission Types:**
- `allow`: Load immediately
- `deny`: Hidden from agents
- `ask`: Prompt user for approval
- `*`: Wildcard patterns supported

### Per-Agent Permissions
Override global permissions for specific agents:

```json
{
  "agent": {
    "build": {
      "permission": {
        "skill": {
          "testing-*": "allow"
        }
      }
    }
  }
}
```

## Skill Content Guidelines

### Structure Best Practices
- **Clear Purpose**: Start with what the skill accomplishes
- **Usage Context**: Explain when and why to use it
- **Step-by-Step**: Provide detailed, actionable instructions
- **File References**: Use relative paths for supporting materials
- **Error Handling**: Include guidance for common issues

### Content Quality
- **Specific**: Avoid vague instructions
- **Comprehensive**: Cover all necessary steps
- **Contextual**: Include relevant background information
- **Testable**: Instructions should be verifiable
- **Maintainable**: Keep content current and accurate

## Development Workflow

### Creating Skills
1. Choose appropriate discovery location
2. Create directory matching skill name
3. Write SKILL.md with proper frontmatter
4. Add supporting files if needed
5. Test skill loading and functionality

### Testing Skills
- Restart OpenCode after changes
- Check skill appears in `skill` tool
- Verify permissions work correctly
- Test skill execution with different agents
- Validate file path resolution

### Skill Maintenance
- Keep descriptions accurate and current
- Update instructions as workflows evolve
- Review and update permissions regularly
- Archive deprecated skills appropriately

## Advanced Features

### Supporting Files
Organize additional resources:

```
skill-name/
├── SKILL.md              # Required
├── scripts/              # Executable helpers
│   └── automation.sh
├── templates/            # Reusable templates
│   └── config-template.md
├── docs/                 # Documentation
│   └── usage-examples.md
└── assets/               # Static resources
    └── diagrams/
```

### Metadata Usage
Leverage metadata for organization:

```yaml
metadata:
  category: development
  complexity: intermediate
  prerequisites: git, npm
  tags: testing, quality, automation
```

### Cross-Skill References
Skills can reference other skills:

```
See also: code-review, testing-setup, deployment
```

## Common Patterns

### Code Review Skill
```yaml
---
name: code-review
description: Comprehensive code review following security, performance, and quality standards
license: MIT
metadata:
  category: quality-assurance
  audience: developers
---

## What I do
- Security vulnerability scanning
- Performance optimization review
- Code quality assessment
- Best practices verification
- Testing coverage evaluation

## When to use me
Use for pull request reviews or code quality checks.
Provide file paths or commit ranges to review.

## Review Process
1. Security: Check for injection, XSS, authentication issues
2. Performance: Identify bottlenecks and memory leaks
3. Quality: Verify naming, documentation, structure
4. Testing: Ensure adequate coverage and proper mocking
5. Standards: Confirm framework conventions followed

## Checklist
- [ ] Security issues addressed
- [ ] Performance optimized
- [ ] Code style consistent
- [ ] Tests comprehensive
- [ ] Documentation updated
```

### Project Setup Skill
```yaml
---
name: project-setup
description: Initialize new projects with proper structure, dependencies, and configuration
metadata:
  category: project-management
  audience: developers
---

## What I do
- Create project directory structure
- Initialize package management
- Set up basic configuration files
- Install essential dependencies
- Configure development tools

## When to use me
Use when starting new projects or onboarding team members.
Specify project type (web, api, library) and technology stack.

## Setup Steps
1. Create directory structure
2. Initialize git repository
3. Set up package.json or equivalent
4. Install core dependencies
5. Configure linting and formatting
6. Set up testing framework
7. Create basic documentation

## Technology-Specific Guidance
- **Node.js**: npm init, install dev dependencies
- **Python**: poetry init, requirements.txt
- **Go**: go mod init, basic module structure
```

## Troubleshooting

### Skills Not Appearing
- Verify SKILL.md starts with `---`
- Check frontmatter has `name` and `description`
- Ensure directory name matches `name` field
- Confirm file is named exactly `SKILL.md` (all caps)
- Restart OpenCode after adding/modifying

### Permission Issues
- Check `opencode.json` for permission settings
- Verify pattern matching (wildcards, exact names)
- Test with different agents
- Look for conflicting permission rules

### Path Resolution Problems
- Use relative paths from skill directory
- Verify supporting files exist
- Check permissions on referenced files
- Test with absolute paths if needed

### Loading Errors
- Validate YAML frontmatter syntax
- Check for special characters in names
- Ensure descriptions are within length limits
- Verify metadata format (string-to-string map)

## Best Practices

### Content Organization
- Keep skills focused on single responsibilities
- Use clear, actionable language
- Include examples and templates
- Update regularly based on feedback

### Permission Management
- Start restrictive, expand as needed
- Use patterns for related skill groups
- Document permission rationales
- Audit permissions periodically

### Skill Discovery
- Place project skills in `.opencode/skill/`
- Use global skills for personal workflows
- Consider team-shared skill repositories
- Archive unused skills rather than delete

### Collaboration
- Share effective skills with team
- Document skill dependencies and prerequisites
- Version control skill improvements
- Gather feedback for skill refinement

---

## Cursor Rules Integration

No specific Cursor rules (.cursor/rules/ or .cursorrules) found in this project.

## Copilot Instructions

No Copilot instructions (.github/copilot-instructions.md) found in this project.

## Additional Resources

- [OhMyOpenCode Plugin Documentation](https://opencode.ai/docs/plugins)
- [Zod Schema Documentation](https://zod.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Bun Runtime Documentation](https://bun.sh/docs)</content>
<parameter name="filePath">AGENTS.md