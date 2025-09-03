# Contributing Guide

Personal tutorial collection with community contributions welcome. Help improve tutorials for developers working in restricted environments.

## How to Contribute

### 🐛 Report Issues

Found broken or outdated content:

1. **Check existing issues** first
2. **Create detailed issue** including:
   - Tutorial name and section
   - Operating system and versions
   - Expected vs actual results
   - Error messages or screenshots

### 💡 Suggest Tutorials

Request new tutorials:

1. **Search existing docs** and planned issues
2. **Create issue** with "tutorial request" label
3. **Include details**:
   - Target tools/technologies
   - Use case and benefits
   - Specific challenges encountered

### 🔧 Submit Improvements

#### Quick Fixes
Fix typos, broken links, or small errors:
1. Fork repository
2. Make changes
3. Submit pull request with clear description

#### New Tutorials or Major Updates
Discuss before investing significant time:
1. **Create issue** to discuss tutorial idea
2. **Wait for feedback** on project fit
3. **Follow tutorial format** (detailed below)

## Tutorial Guidelines

### Structure
Each tutorial should follow this format:

```markdown
# Tutorial Title

Brief description of what this tutorial accomplishes.

## Prerequisites
- Requirement 1 (with version if important)
- Requirement 2
- Basic knowledge needed

## Overview
Quick summary of the steps involved.

## Step 1: First Action
Detailed instructions...

## Step 2: Next Action
More instructions...

## Troubleshooting
Common issues and solutions.

## References
- Links to official documentation
- Related tutorials
- Useful resources
```

### Writing Style
- **Clear and direct** - Use prescriptive language and imperatives
- **Copy-pasteable commands** - Include exact code blocks
- **Test on clean systems** - Verify all steps work
- **Explain rationale** - Brief context for each step
- **Add screenshots** for GUI-heavy procedures

### File Organization
- Use lowercase with hyphens: `setup-nodejs-environment.md`
- Place in appropriate OS folder under `/docs/`
- Update tutorial index file

## Quality Standards

### Before Submitting
- [ ] Tutorial works on a clean system
- [ ] All commands are tested
- [ ] Links are working
- [ ] Spelling and grammar checked
- [ ] Follows the standard format

### Commit Messages
Use conventional format with UPPERCASE types:
```
FEAT[windows]: Add Docker Desktop tutorial
FIX[scoop]: Update Node.js installation steps  
DOCS: Fix broken links in Git workflow guide
```

## Project Scope

Primary focus areas:
- **Windows development** - Non-admin installations preferred
- **Scientific computing** - Python, R, data analysis tools
- **Engineering workflows** - Practical, working solutions
- **Restricted environments** - Corporate/academic settings
- **Quality over quantity** - Well-tested, comprehensive guides

## Recognition

Contributors acknowledged in:
- Tutorial credits (substantial contributions)
- CHANGELOG.md updates
- README acknowledgments

## Questions?

Get help:
- Open discussion issue
- Review existing tutorials for examples  
- Ask questions in pull requests

Thanks for improving these tutorials!