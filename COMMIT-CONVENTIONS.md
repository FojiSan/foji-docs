# Commit Message Conventions

This project follows conventional commit format with UPPERCASE types for consistency and automated tooling support.

## Format Structure

```
<TYPE>[optional scope]: <description>

[optional body]

[optional footer]
```

## Commit Types

**FEAT** - New features or functionality
- `FEAT[windows]: Add Docker Desktop installation tutorial`
- `FEAT: Add package manager comparison guide`

**DOCS** - Documentation changes only
- `DOCS[windows]: Update Scoop advanced configuration`
- `DOCS: Fix broken links in README`

**FIX** - Bug fixes and corrections
- `FIX[scoop]: Correct aria2 configuration command`
- `FIX: Remove duplicate installation steps`

**REFACTOR** - Code restructuring without feature changes
- `REFACTOR: Split combined tutorial into separate files`
- `REFACTOR[docs]: Reorganize folder structure`

**STYLE** - Formatting and style changes
- `STYLE: Apply consistent markdown formatting`
- `STYLE[windows]: Update writing style to prescriptive language`

**CHORE** - Maintenance tasks and tooling
- `CHORE: Add gitignore for IDE settings`
- `CHORE: Update license file`

## Scope Guidelines

- Use platform names: `windows`, `macos`, `linux`
- Use tool names: `scoop`, `chocolatey`, `git`
- Use category names: `docs`, `config`, `setup`
- Omit scope for project-wide changes

## Footer Format

Always include attribution footer for Claude Code generated commits:

```
🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
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

### Bug Fix
```
FIX[scoop]: Correct PowerShell execution policy command

Fix incorrect execution policy scope in installation tutorial.
Change from RemoteSigned to CurrentUser scope for non-admin environments.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```