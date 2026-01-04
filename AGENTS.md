# AGENTS.md - OpenCode Skills Development Guide

## Overview

This document provides comprehensive guidelines for developing skills in the OpenCode Skills repository. Skills are reusable instruction sets that extend AI agent capabilities with specialized knowledge, workflows, and tool integrations.

## Build, Lint, and Test Commands

### Core Development Commands

#### Skill Validation
```bash
# Validate a single skill structure and frontmatter
python skills/skill-creator/scripts/quick_validate.py skills/<skill-name>

# Example: Validate the web-ui-ux skill
python skills/skill-creator/scripts/quick_validate.py skills/web-ui-ux
```

#### Skill Packaging
```bash
# Package a skill into a distributable .skill file
python skills/skill-creator/scripts/package_skill.py skills/<skill-name>

# Package to specific output directory
python skills/skill-creator/scripts/package_skill.py skills/<skill-name> ./dist

# Example: Package the icon-generator skill
python skills/skill-creator/scripts/package_skill.py skills/icon-generator
```

#### Skill Creation
```bash
# Initialize a new skill with proper structure
python skills/skill-creator/scripts/init_skill.py <skill-name> --path skills

# Example: Create a new skill called "api-testing"
python skills/skill-creator/scripts/init_skill.py api-testing --path skills
```

#### Icon Generation (Specialized)
```bash
# Generate icons for web and Unreal Engine
python skills/icon-generator/scripts/generate_icons.py <input-svg> <output-dir>

# Example: Generate icons from logo.svg
python skills/icon-generator/scripts/generate_icons.py assets/logo.svg ./output-icons
```

### Dependency Management

#### JavaScript/TypeScript (Bun)
```bash
# Install dependencies
bun install

# Add a new dependency
bun add <package-name>

# Development server (if configured)
bun run dev

# Build project
bun run build
```

#### Python Dependencies
```bash
# Install Python requirements (if requirements.txt exists)
pip install -r requirements.txt

# Or install specific packages
pip install pyyaml pathlib
```

### Testing Strategy

Since this project focuses on skill development rather than traditional software testing:

1. **Manual Validation**: Use `quick_validate.py` for structural validation
2. **Integration Testing**: Test skills by loading them in OpenCode environment
3. **Content Validation**: Review SKILL.md files for clarity and completeness
4. **Packaging Tests**: Verify `.skill` files can be created and distributed

## Code Style Guidelines

### General Principles

- **Clarity First**: Write for readability and maintainability
- **Consistency**: Follow established patterns within the codebase
- **Documentation**: Include docstrings, comments, and clear variable names
- **Error Handling**: Implement proper error handling with meaningful messages

### Python Code Style

#### File Structure
```python
#!/usr/bin/env python3
"""
Module docstring describing purpose and usage.
"""

import sys
from pathlib import Path
from typing import Optional, Dict, List

# Constants in UPPER_CASE
DEFAULT_TIMEOUT = 30
MAX_RETRIES = 3

class SkillValidator:
    """Class for validating skill structures."""

    def __init__(self, skill_path: str):
        """Initialize validator with skill path.

        Args:
            skill_path: Path to the skill directory
        """
        self.skill_path = Path(skill_path)

    def validate(self) -> tuple[bool, str]:
        """Validate the skill structure and content.

        Returns:
            Tuple of (is_valid, error_message)
        """
        try:
            return self._validate_structure()
        except Exception as e:
            return False, f"Validation failed: {e}"

    def _validate_structure(self) -> tuple[bool, str]:
        """Validate basic skill directory structure."""
        if not self.skill_path.exists():
            return False, f"Skill path does not exist: {self.skill_path}"

        skill_md = self.skill_path / "SKILL.md"
        if not skill_md.exists():
            return False, "SKILL.md file is required"

        return True, "Skill structure is valid"
```

#### Key Conventions
- **Encoding**: Always use UTF-8 for file operations
- **Type Hints**: Include type annotations for function parameters and return values
- **Docstrings**: Use Google-style docstrings
- **Error Messages**: Provide clear, actionable error messages
- **Imports**: Group imports (standard library, third-party, local)
- **Constants**: Use UPPER_CASE for constants
- **Private Methods**: Prefix with underscore for internal methods

#### Naming Conventions
- **Functions/Variables**: `snake_case`
- **Classes**: `PascalCase`
- **Constants**: `UPPER_CASE`
- **Files**: `snake_case.py` or `kebab-case.py` depending on context

### Markdown Style

#### SKILL.md Structure
```markdown
---
name: skill-name
description: Action-oriented description of what the skill does and when to use it. Include specific triggers and use cases.
license: MIT
metadata:
  category: development
  complexity: intermediate
  audience: developers
---

# Skill Title

Brief introduction to the skill's purpose.

## When to Use This Skill

- Specific scenario 1
- Specific scenario 2
- File types or contexts that trigger usage

## What I Do

- Accomplishment 1
- Accomplishment 2
- Accomplishment 3

## Instructions

### Step 1: Preparation
Detailed instructions for the first step.

### Step 2: Execution
Detailed instructions for the second step.

## Examples

### Basic Usage
```bash
# Example command or code
command --option value
```

### Advanced Usage
```python
# Example Python code
from skill_module import SkillClass

skill = SkillClass()
result = skill.process(data)
```

## File References

- `scripts/helper.py` - Utility functions
- `references/api-docs.md` - API documentation
- `assets/template.json` - Configuration template

## Troubleshooting

### Common Issues

**Issue: Skill not loading**
- Verify SKILL.md starts with `---`
- Check frontmatter YAML syntax
- Ensure name matches directory name

**Issue: Validation errors**
- Run `quick_validate.py` for detailed error messages
- Check description length (max 1024 characters)
- Verify naming conventions (hyphen-case only)
```

#### Markdown Conventions
- **Headers**: Use `#` for main title, `##` for sections, `###` for subsections
- **Code Blocks**: Use appropriate language tags for syntax highlighting
- **Lists**: Use `-` for bullets, `1.` for numbered lists
- **Links**: Use reference-style links when possible
- **Emphasis**: Use `**bold**` for emphasis, `*italic*` for secondary emphasis

### YAML Frontmatter Style

#### Required Fields
```yaml
---
name: skill-name
description: Clear, action-oriented description under 1024 characters
---
```

#### Optional Fields
```yaml
---
name: skill-name
description: Description of what the skill does and when to use it
license: MIT
metadata:
  category: development
  complexity: intermediate
  audience: developers
  prerequisites: git, python3
  tags: automation, tooling, productivity
---
```

#### Validation Rules
- **Name**: `^[a-z0-9]+(-[a-z0-9]+)*$` (hyphen-case, 1-64 characters)
- **Description**: 1-1024 characters, no angle brackets `<>` or `>`
- **Metadata**: Key-value pairs only, strings preferred

### File Organization

#### Skill Directory Structure
```
skill-name/
├── SKILL.md              # Required: Main skill file
├── scripts/              # Optional: Executable automation
│   ├── helper.py
│   └── setup.sh
├── references/           # Optional: Documentation loaded on-demand
│   ├── api-specs.md
│   └── examples.md
└── assets/               # Optional: Files used in outputs
    ├── template.json
    └── logo.png
```

#### Root Directory Structure
```
opencode-skills/
├── skills/               # All skill directories
├── assets/               # Project branding
├── .github/              # CI/CD and instructions
├── package.json          # Bun dependencies
├── bun.lock             # Lockfile
├── README.md            # Project documentation
└── AGENTS.md            # This file
```

## Error Handling Patterns

### Python Error Handling
```python
def safe_file_operation(file_path: str) -> Optional[str]:
    """Safely read a file with proper error handling."""
    try:
        path = Path(file_path)
        if not path.exists():
            logger.error(f"File not found: {file_path}")
            return None

        content = path.read_text(encoding='utf-8')
        return content

    except PermissionError:
        logger.error(f"Permission denied reading: {file_path}")
        return None
    except UnicodeDecodeError:
        logger.error(f"Invalid UTF-8 encoding in: {file_path}")
        return None
    except Exception as e:
        logger.error(f"Unexpected error reading {file_path}: {e}")
        return None
```

### Validation Error Messages
```python
def validate_skill_name(name: str) -> tuple[bool, str]:
    """Validate skill name format."""
    if not name:
        return False, "Skill name cannot be empty"

    if not re.match(r'^[a-z0-9-]+$', name):
        return False, f"Invalid name '{name}': use lowercase letters, digits, and hyphens only"

    if name.startswith('-') or name.endswith('-'):
        return False, f"Invalid name '{name}': cannot start or end with hyphen"

    if '--' in name:
        return False, f"Invalid name '{name}': cannot contain consecutive hyphens"

    if len(name) > 64:
        return False, f"Name too long ({len(name)} chars): maximum 64 characters"

    return True, "Name is valid"
```

## Import and Dependency Management

### Python Imports
```python
# Standard library imports
import sys
import re
from pathlib import Path
from typing import Dict, List, Optional

# Third-party imports
import yaml

# Local imports (relative)
from .validators import validate_name
from ..utils.helpers import safe_read
```

### JavaScript/TypeScript Imports
```typescript
// ES6 imports
import { readFile, writeFile } from 'fs/promises';
import { z } from 'zod';

// Relative imports
import { validateSkill } from './validators.js';
import { SkillConfig } from '../types.js';
```

## Logging and Debugging

### Python Logging
```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

def process_skill(skill_path: str) -> bool:
    """Process a skill with proper logging."""
    logger.info(f"Processing skill: {skill_path}")

    try:
        # Processing logic
        logger.debug("Validation step completed")
        logger.info("Skill processed successfully")
        return True

    except Exception as e:
        logger.error(f"Failed to process skill {skill_path}: {e}")
        return False
```

## Testing and Validation

### Unit Testing Pattern
```python
import unittest
from unittest.mock import patch, MagicMock

class TestSkillValidator(unittest.TestCase):
    """Test cases for skill validation."""

    def setUp(self):
        """Set up test fixtures."""
        self.validator = SkillValidator("test-skill")

    def test_valid_skill_passes(self):
        """Test that valid skills pass validation."""
        with patch('pathlib.Path.exists', return_value=True):
            is_valid, message = self.validator.validate()
            self.assertTrue(is_valid)
            self.assertEqual(message, "Skill structure is valid")

    def test_missing_skill_md_fails(self):
        """Test that missing SKILL.md causes failure."""
        with patch('pathlib.Path.exists', return_value=False):
            is_valid, message = self.validator.validate()
            self.assertFalse(is_valid)
            self.assertIn("SKILL.md", message)

    def tearDown(self):
        """Clean up test fixtures."""
        pass

if __name__ == '__main__':
    unittest.main()
```

## Cursor Rules Integration

No specific Cursor rules (.cursor/rules/ or .cursorrules) found in this project.

## Copilot Instructions

### Copilot Instructions (.github/copilot-instructions.md)

**Big Picture Architecture:**
- Skills located in `skills/<skill-name>/` with mandatory `SKILL.md` and optional resource folders (`scripts/`, `references/`, `assets/`)
- OpenCode ecosystem loads skills on-demand via SKILL.md frontmatter (name/description)
- Progressive disclosure: Keep SKILL.md lean (<500 lines), move detailed docs to `references/`, automation to `scripts/`
- Runtime uses Bun for JS/TS tasks, Python for automation

**Critical Developer Workflows:**
- Initialize: `python skills/skill-creator/scripts/init_skill.py <skill-name> --path skills`
- Validate: `python skills/skill-creator/scripts/quick_validate.py skills/<skill-name>`
- Package: `python skills/skill-creator/scripts/package_skill.py skills/<skill-name>`
- Testing: Execute scripts in `scripts/` directly, verify SKILL.md instructions are actionable

**Project-Specific Conventions:**
- Naming: Skill names must be `hyphen-case` (lowercase alphanumeric + hyphens)
- Frontmatter: SKILL.md must start with YAML containing `name` and `description`
- Descriptions: Action-oriented with specific triggers/contexts (e.g., "Use when...")
- Unreal Engine: Follow UE C++ standards and UMG best practices for UE-related skills
- Memory Bank: Respect memory-bank system if present in environment

**Taming Copilot Rules:**
- Be concise: Avoid verbose explanations, prefer code examples and direct instructions
- Actionable: Every instruction leads to clear outcome
- No fluff: Focus on project's domain, not AI itself
- Context awareness: Reference AGENTS.md and skill-creator/SKILL.md for latest standards

**Key Files:**
- skills/AGENTS.md: Global development guidelines
- skills/skill-creator/SKILL.md: Detailed skill anatomy and design patterns
- skills/skill-creator/scripts/init_skill.py: Template for new skills

## Additional Resources

- [OhMyOpenCode Plugin Documentation](https://opencode.ai/docs/plugins)
- [Zod Schema Documentation](https://zod.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Bun Runtime Documentation](https://bun.sh/docs)
- [Python Style Guide (PEP 8)](https://peps.python.org/pep-0008/)
- [Google Python Docstring Style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)

---

This guide should be updated as the project evolves. Last updated: January 2026</content>
<parameter name="filePath">AGENTS.md