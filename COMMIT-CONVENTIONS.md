# Commit Message Conventions

This project follows conventional commit format with UPPERCASE types for consistency and automated tooling support.

## Format Structure

```
<TYPE>[optional scope]: <description>

[optional body]

[optional footer]
```

## Commit Types

**FEAT** - New content addition (new tutorials, sections, or information)
- `FEAT[windows]: Add Docker Desktop installation tutorial`
- `FEAT: Add package manager comparison guide`

**DOCS** - Project-level documentation (README, LICENSE, CONTRIBUTING, etc.)
- `DOCS: Update main README with project overview`
- `DOCS: Add MIT license file`

**FIX** - Corrections to existing content (writing style, accuracy, clarity)
- `FIX[scoop]: Correct aria2 configuration command`
- `FIX: Correct writing style to prescriptive language`

**REFACTOR** - Restructuring content without changing information
- `REFACTOR: Split combined tutorial into separate files`
- `REFACTOR[docs]: Reorganize folder structure`

**STYLE** - Formatting changes only (markdown, code style, indentation)
- `STYLE: Apply consistent markdown formatting`
- `STYLE: Fix code block indentation`

**CHORE** - Maintenance tasks and tooling
- `CHORE: Add gitignore for IDE settings`
- `CHORE: Update license file`

**CONFIG** - Configuration changes (settings, build files, etc.)
- `CONFIG: Add git commit conventions to Claude settings`
- `CONFIG: Update project build configuration`

## Type Usage Guidelines

**Use FEAT when:**
- Adding new tutorials inside `./docs` folder
- Adding new sections or content to existing tutorials
- Introducing new information or procedures

**Use DOCS when:**
- Modifying project-level files (README, LICENSE, CONTRIBUTING)
- Changes to repository documentation structure
- Meta-documentation about the project itself

**Use FIX when:**
- Correcting writing style or tone
- Fixing factual errors or outdated information
- Improving clarity or accuracy of existing content
- Grammar, spelling, or language corrections

**Use REFACTOR when:**
- Splitting large documents into smaller files
- Combining multiple documents
- Reorganizing file/folder structure
- Moving content between files (information stays the same)

**Use STYLE when:**
- Markdown formatting changes only
- Code syntax highlighting fixes
- Indentation or whitespace corrections
- No content changes, only presentation

## Scope Guidelines

- Use platform names: `windows`, `macos`, `linux`
- Use tool names: `scoop`, `chocolatey`, `git`
- Use category names: `docs`, `config`, `setup`
- Omit scope for project-wide changes

## Footer Format

Footer is not required if the committer is the only author. Add footer information when additional contributors are involved:

**Co-authors:**
```
Co-Authored-By: Jane Doe <jane@example.com>
Co-Authored-By: John Smith <john@example.com>
```

**AI-generated content:**
```
🤖 Generated with [Claude Code](https://claude.ai/code)
Co-Authored-By: Claude <noreply@anthropic.com>
```

**Reviewers/Proofreaders:**
```
Reviewed-By: Jane Doe <jane@example.com>
```

**Combined example:**
```
FEAT[windows]: Add Docker installation tutorial

Complete guide for Docker Desktop setup on Windows 11.

Co-Authored-By: John Smith <john@example.com>
🤖 Generated with [Claude Code](https://claude.ai/code)
Co-Authored-By: Claude <noreply@anthropic.com>
Reviewed-By: Jane Doe <jane@example.com>
```

## Examples

### Feature Addition
```
FEAT[windows]: Add WSL2 development environment setup

Complete tutorial covering WSL2 installation, Ubuntu setup, and 
integration with Windows development tools. Includes troubleshooting 
section for common installation issues.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

### Documentation Update
```
DOCS: Update README with new tutorial structure

Restructure main README to focus on Windows development tutorials
for scientists and engineers. Add quick start section and improve
navigation between tutorial categories.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

### Content Correction
```
FIX[scoop]: Correct PowerShell execution policy command

Fix incorrect execution policy scope in installation tutorial.
Change from RemoteSigned to CurrentUser scope for non-admin environments.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

### Refactor Example
```
REFACTOR: Split package manager tutorial into separate files

Split single comprehensive package manager guide into individual
tutorials for Scoop, Chocolatey, and winget. Information content
remains unchanged, only file organization restructured.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```