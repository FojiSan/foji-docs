# Poetry Setup and Usage Guide

Practical guide to Poetry installation, configuration, and basic usage covering version evolution and current best practices.

## Current Poetry Status

### Version Evolution

**Poetry 1.x (Stable Period)**
- Mature dependency management
- Reliable virtual environment handling
- Predictable behavior across platforms

**Poetry 2.0.x (Buggy Transition)**
- Major architecture changes introduced instabilities
- Frequent crashes and dependency resolution failures
- Python version management issues

**Poetry 2.1.4 (Latest Stable)**
- Resolved major 2.0.x stability issues
- Introduced experimental Python version management
- Performance improvements in dependency resolution
- Recommended for new installations

*Version stability documented in [Poetry's release history](https://github.com/python-poetry/poetry/releases) and community feedback on [Poetry GitHub issues](https://github.com/python-poetry/poetry/issues).*

## Current Installation Problems

### Typical Corporate Setup

**Common configuration:**

Poetry installed in conda base environment:
```bash
conda activate base
pip install poetry
```

**Resulting issues:**
- Poetry tied to conda's Python version
- Project Python requirements conflict with conda base
- Difficult to manage multiple Python versions (for Poetry <= 2.0)

### Pain Points

**1. Python Version Mismatch**

Check conda `base` Python version:
```bash
conda info
```

Output:
```
python version : 3.10.9.final.0
```

Project requirement:
```toml
[tool.poetry.dependencies]
python = "^3.11"
```

Poetry fails to create environment:
```bash
poetry install
```

Error output:
```bash
The currently activated Python version 3.10.9 is not supported by the project (>=3.11,<3.14).
Trying to find and use a compatible version. 

Poetry was unable to find a compatible version. If you have one, you can explicitly use it via the "env use" command.
```

**2. External Python Installation Required**

Install Python versions using external tools:
```bash
conda install python=3.11
poetry env use python3.11
```

Alternatives: pyenv, scoop, or system package managers.

**3. Dependency Resolution Failures**

Common 2.0.x issues:
```bash
poetry add numpy pandas scikit-learn
```

Error output:
```
SolverProblemError: Because no versions of numpy match >1.24.0,<2.0.0
and pandas (2.1.4) depends on numpy>=1.23.2,<2.0.0
and scikit-learn (1.3.2) depends on numpy>=1.17.3,<1.25.0
numpy is forbidden.
Process hangs or crashes
```

**4. Version Instability**
- Poetry 2.0.x versions crash during dependency resolution
- Inconsistent behavior between patch versions
- Lock file corruption issues

## Recommended Installation Procedures

### Current Poetry Upgrade (Conda Base)

**If Poetry installed via pip in conda base:**

Activate conda base environment:
```bash
conda activate base
```

Option 1 - Upgrade existing installation:
```bash
pip install --upgrade poetry
```

Option 2 - Clean reinstall (recommended for 2.0.x users):
```bash
pip uninstall poetry
pip install poetry==2.1.4
```

Verify installation:
```bash
poetry --version
```

### Recommended: pipx Installation

**Benefits:**
- Isolated Poetry installation
- Independent of conda/system Python
- Easy version management
- No environment contamination

**Installation steps:**

**1. Install pipx (system level):**

Linux/macOS system installation:
```bash
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

Or install in conda base if preferred:
```bash
conda activate base
pip install pipx
```

**2. Install Poetry via pipx:**

Install latest stable version:
```bash
pipx install poetry
```

Check Poetry location:
```bash
which poetry
```

Output:
```
/home/user/.local/bin/poetry
```

Verify Poetry version:
```bash
poetry --version
```

Output:
```
Poetry (version 2.1.4)
```

### Windows Users: Scoop Integration

Install pipx through Scoop:
```powershell
scoop install pipx
```

Install Poetry through pipx:
```powershell
pipx install poetry
```

Verify installation:
```powershell
poetry --version
```

*Windows installation follows [Scoop package availability](https://github.com/ScoopInstaller/Main/blob/master/bucket/pipx.json) and [pipx documentation](https://pipx.pypa.io/stable/installation/).*

## Poetry Basic Usage

### Python Version Management (2.1.4 Only)

**Install Python versions:**

Install specific Python version:
```bash
poetry python install 3.11
```

Install multiple versions:
```bash
poetry python install 3.10 3.11 3.12
```

**List available versions:**

Show installed Python versions:
```bash
poetry python list
```

Output:
```bash
Version Implementation Manager Path 

3.13.7  CPython        Poetry  ~/.local/share/pypoetry/python/cpython@3.13.7/bin/python3.13      
3.12.11 CPython        Poetry  ~/.local/share/pypoetry/python/cpython@3.12.11/bin/python3.12    
3.11.13 CPython        Poetry  ~/.local/share/pypoetry/python/cpython@3.11.13/bin/python3.11     
3.10.9  CPython        System  ~/miniconda3/bin/python3.10                                                             
3.9.5   CPython        System  /usr/bin/python3.9                     
```

**Remove unused versions:**

Remove specific version:
```bash
poetry python remove 3.10
```

Clean unused installations:
```bash
poetry python remove --all
```

*Python management commands are experimental features documented in [Poetry CLI reference](https://python-poetry.org/docs/cli/#python).*

### Dependency Management

**Add dependencies:**

Production dependencies:
```bash
poetry add requests pandas numpy
```

Development dependencies:
```bash
poetry add pytest black mypy --group dev
```

Optional dependencies:
```bash
poetry add streamlit --group web
```

Specific version constraints:
```bash
poetry add "django>=4.0,<5.0"
```

**Remove dependencies:**

Remove single package:
```bash
poetry remove requests
```

Remove development dependencies:
```bash
poetry remove pytest --group dev
```

**Update dependencies:**

Update single package:
```bash
poetry update requests
```

Update all packages:
```bash
poetry update
```

Preview changes:
```bash
poetry update --dry-run
```

**Environment synchronization:**

Install all dependencies from lock file:
```bash
poetry install
```

Install only production dependencies:
```bash
poetry install --without dev
```

Install with specific groups:
```bash
poetry install --with test,docs
```

Sync environment to match lock file exactly:
```bash
poetry sync
```

### Project Workflow

**Initialize new project:**

Create project structure:
```bash
poetry new my-project
cd my-project
```

Or initialize in existing directory:
```bash
poetry init
```

**Development workflow:**

Install project in development mode:
```bash
poetry install
```

Activate virtual environment:
```bash
poetry shell
```

Run Python script in environment:
```bash
poetry run python script.py
```

Run tests in environment:
```bash
poetry run pytest
```

**Dependency file management:**

Export requirements for deployment:
```bash
poetry export -f requirements.txt --output requirements.txt
```

Export with development dependencies:
```bash
poetry export -f requirements.txt --output requirements-dev.txt --with dev
```

## Troubleshooting Common Issues

### Poetry Command Not Found

Check installation method:
```bash
which poetry
```

pipx installation - check PATH:
```bash
pipx ensurepath
```

Conda installation - activate base environment:
```bash
conda activate base
```

### Python Version Conflicts

Check current environment Python:
```bash
poetry run python --version
```

List available environments:
```bash
poetry env list
```

Remove problematic environment:
```bash
poetry env remove python3.9
```

Create new environment with specific Python:
```bash
poetry env use python3.11
```

### Dependency Resolution Hangs

Clear Poetry cache:
```bash
poetry cache clear pypi --all
```

Use verbose output for debugging:
```bash
poetry add package-name -vvv
```

Temporary workaround for complex dependencies:
```bash
poetry add package-name --dry-run
```

### Version Downgrade (If 2.1.4 Issues)

Downgrade to stable 1.x version:
```bash
pipx install poetry==1.8.0 --force
```

Or via pip in conda base:
```bash
pip install poetry==1.8.0 --force-reinstall
```

## Migration Checklist

**From conda base pip installation:**

1. Export current project dependencies:
```bash
poetry export
```

2. Install pipx:
```bash
pip install pipx
```

3. Install Poetry via pipx:
```bash
pipx install poetry
```

4. Verify new installation:
```bash
poetry --version
```

5. Test with existing projects:
```bash
poetry install --dry-run
```

6. Remove old installation:
```bash
pip uninstall poetry
```

**From Poetry 2.0.x:**

1. Back up existing lock files:
```bash
cp poetry.lock poetry.lock.backup
```

2. Upgrade to 2.1.4:
```bash
pipx install poetry==2.1.4 --force
```

3. Clear cache:
```bash
poetry cache clear pypi --all
```

4. Regenerate lock file:
```bash
poetry lock --no-update
```

5. Test installation:
```bash
poetry install
```

This approach provides a more reliable Poetry setup with better isolation and easier version management across different projects and Python versions.

## References

- [Poetry Installation Documentation](https://python-poetry.org/docs/#installation)
- [pipx Installation Guide](https://pipx.pypa.io/stable/installation/)
- [Poetry CLI Commands Reference](https://python-poetry.org/docs/cli/)
- [Poetry Managing Environments](https://python-poetry.org/docs/managing-environments/)