# Poetry and uv: Known Issues and Gotchas

Comprehensive guide to common problems, bugs, incompatibilities, and user pain points when using Poetry and uv in real-world Python projects.

## Poetry Known Issues

### Dependency Resolution Problems

**Complex constraint satisfaction failures:**
```bash
# Common failure scenario
poetry add tensorflow==2.11.0 torch==1.13.0 transformers==4.25.0

# Error: Because no versions of torch match >1.13.0,<1.14.0
# and tensorflow 2.11.0 depends on torch>=1.12.0,<1.13.0,
# torch is forbidden.
```

**Resolution:** Manual constraint specification often required
```toml
[tool.poetry.dependencies]
torch = {version = "^1.13.0", source = "pytorch"}
tensorflow = {version = "^2.11.0", markers = "python_version >= '3.8'"}
```

**Git dependency version pinning issues:**
```toml
# This can break reproducibility
my-package = {git = "https://github.com/user/repo.git", branch = "main"}

# Better, but Poetry doesn't always respect it
my-package = {git = "https://github.com/user/repo.git", rev = "abc123"}
```

### Performance and Resource Issues

**Memory exhaustion during resolution:**
```bash
# Large projects often hit memory limits
poetry install
# [Errno 12] Cannot allocate memory
```

**Slow lock file updates:**
```bash
# Adding a single dependency can take minutes
time poetry add requests
# real    2m34.156s  # This should be seconds
```

**Disk space bloat:**
```bash
# Poetry caches grow large over time
du -sh ~/.cache/pypoetry
# 15G  /home/user/.cache/pypoetry

# Multiple virtual environments for similar projects
du -sh .venv/
# 2.1G per project environment
```

### Virtual Environment Management Issues

**Environment location inconsistencies:**
```bash
# Poetry creates environments in different locations
poetry env info --path
# Sometimes: /home/user/.cache/pypoetry/virtualenvs/project-abc123-py3.11
# Other times: ./project/.venv
# Depends on settings and project structure
```

**Python version switching problems:**
```bash
# Changing Python version doesn't always work cleanly
poetry env use python3.12
# Environment still uses python3.11 from old lock file
# Manual environment deletion often required
```

**Windows path length limitations:**
```bash
# Long Windows paths break Poetry installations
poetry install
# FileNotFoundError: [Errno 2] No such file or directory: 
# 'C:\\Users\\username\\very\\long\\project\\path\\.venv\\Scripts\\...'
```

### Corporate Environment Issues

**Proxy and firewall problems:**
```bash
# Corporate proxies break Poetry's network requests
poetry install
# HTTPSConnectionPool: Max retries exceeded with url: /simple/requests/
# SSL certificate verification errors
```

**Workaround required:**
```bash
poetry config repositories.corporate-pypi https://pypi.internal.com/simple/
poetry config http-basic.corporate-pypi username password
poetry source add corporate-pypi https://pypi.internal.com/simple/ --default
```

**Private repository authentication:**
```bash
# Token authentication often fails
poetry config pypi-token.private-repo abc123
poetry add private-package --source private-repo
# 401 Unauthorized errors even with valid tokens
```

### Build and Publishing Problems

**Inconsistent build artifacts:**
```bash
# Same project builds differently on different machines
poetry build
# Windows: wheel includes .pyc files
# Linux: wheel structure differs
# Causes deployment inconsistencies
```

**Poetry.lock vs setup.py conflicts:**
```bash
# Legacy setup.py requirements don't match poetry.lock
poetry install  # Uses poetry.lock
pip install -e .  # Uses setup.py, gets different versions
```

### Integration Issues

**IDE and tooling integration:**
```bash
# VSCode Python extension can't find Poetry virtual environment
# PyCharm sometimes creates duplicate environments
# Jupyter kernels don't automatically use Poetry environment
```

**Docker integration complexity:**
```dockerfile
# Multi-stage builds become complex with Poetry
FROM python:3.11
COPY pyproject.toml poetry.lock ./
RUN pip install poetry
RUN poetry config virtualenvs.create false
RUN poetry install --no-dev
# This often fails in containerized environments
```

## uv Known Issues

### Compatibility and Feature Gaps

**Limited pip feature coverage:**
```bash
# Some pip flags not yet supported
uv install --find-links ./wheels package-name
# Error: unrecognized option '--find-links'

# Editable installs with path dependencies
uv add -e ./local-package
# Sometimes fails with complex local package structures
```

**requirements.txt parsing edge cases:**
```txt
# Complex requirement specifications sometimes fail
package-name @ git+https://github.com/user/repo.git@branch#egg=package-name&subdirectory=sub
# uv may not parse this correctly while pip does
```

**Wheel building limitations:**
```bash
# Packages requiring compilation may fail
uv add mysqlclient
# Error: Failed to build wheel for mysqlclient
# Missing system dependencies not handled gracefully
```

### Platform-Specific Issues

**Windows-specific problems:**
```bash
# Long path issues on Windows
uv venv very-long-project-name-that-exceeds-windows-path-limits
# FileNotFoundError due to path length

# PowerShell execution policy conflicts
uv venv && .venv\Scripts\activate
# Execution of scripts is disabled on this system
```

**macOS ARM64 compatibility:**
```bash
# Some packages don't have ARM64 wheels
uv add numpy  # Older versions
# Falls back to source builds which may fail without proper toolchain
```

**Linux distribution variations:**
```bash
# Different libc versions cause wheel compatibility issues
uv add package-with-compiled-extensions
# Works on Ubuntu 22.04, fails on CentOS 7 with older glibc
```

### Virtual Environment Issues

**Environment activation inconsistencies:**
```bash
# Different shells handle activation differently
source .venv/bin/activate  # bash/zsh
. .venv/bin/activate.fish  # fish
.venv\Scripts\activate.bat  # Windows cmd
.venv\Scripts\Activate.ps1  # PowerShell
# uv doesn't provide unified activation command
```

**Python version management:**
```bash
# uv doesn't manage Python versions
uv venv --python python3.12
# Error: python3.12 not found
# Requires external Python version management (pyenv, etc.)
```

### Lock File and Reproducibility Issues

**Lock file conflicts in teams:**
```yaml
# uv.lock files can diverge between team members
# Platform-specific resolutions cause merge conflicts
# No built-in lock file merge resolution
```

**Cross-platform reproducibility:**
```bash
# Lock file generated on Linux may not work on Windows
uv sync
# Different package versions installed due to platform constraints
```

### Corporate Environment Challenges

**Proxy configuration complexity:**
```bash
# Corporate proxies require multiple environment variables
export HTTPS_PROXY=http://proxy.corp.com:8080
export HTTP_PROXY=http://proxy.corp.com:8080
export NO_PROXY=localhost,127.0.0.1,.corp.com
# uv sometimes ignores these settings
```

**Internal package repositories:**
```bash
# Custom PyPI mirrors not always supported
uv add package --index-url https://pypi.internal.corp.com/simple/
# Authentication with internal repositories can fail
```

## Common Gotchas and Workarounds

### Poetry Gotchas

**1. Version constraint conflicts:**
```toml
# Problem: Overly restrictive constraints
[tool.poetry.dependencies]
requests = "^2.28.0"  # Excludes 2.29.x, 2.30.x, etc.

# Solution: Use compatible release operator carefully
requests = "~2.28.0"  # Allows 2.28.x only
# Or be more permissive
requests = ">=2.28.0,<3.0.0"
```

**2. Development dependency isolation:**
```bash
# Problem: Dev dependencies installed in production
poetry install

# Solution: Use production flag
poetry install --without dev
```

**3. Environment path issues:**
```bash
# Problem: Environment not found
poetry run python script.py
# Error: The virtual environment does not exist

# Solution: Recreate environment
poetry env remove python && poetry install
```

### uv Gotchas

**1. Missing build dependencies:**
```bash
# Problem: Source builds fail silently
uv add package-requiring-rust

# Solution: Install system build tools first
# Linux: apt-get install build-essential
# macOS: xcode-select --install
# Windows: Visual Studio Build Tools
```

**2. Cache corruption:**
```bash
# Problem: Inconsistent installations
uv add numpy
# Gets wrong version or corrupted files

# Solution: Clear cache
uv cache clean
```

**3. Virtual environment location assumptions:**
```bash
# Problem: Scripts assume .venv location
source .venv/bin/activate

# Solution: Check actual environment path
uv venv --show-path
```

## Mitigation Strategies

### Poetry Optimization

**Speed improvements:**
```bash
# Configure faster installer
poetry config installer.max-workers 8
poetry config installer.no-binary false

# Use system trust store for corporate environments
poetry config certificates.corporate-ca.cert /path/to/ca-bundle.crt
```

**Memory management:**
```bash
# Limit memory usage during resolution
export POETRY_SOLVER_MEMORY_LIMIT=2048  # MB

# Use simpler solver
poetry config solver.lazy-wheel false
```

### uv Optimization

**Corporate environment setup:**
```bash
# Configure proxy and certificates
export UV_SYSTEM_CERTS=1
export UV_KEYRING_PROVIDER=subprocess

# Set custom index
export UV_INDEX_URL=https://pypi.internal.corp.com/simple/
```

**Performance tuning:**
```bash
# Parallel downloads
export UV_CONCURRENT_DOWNLOADS=10

# Cache location on fast storage
export UV_CACHE_DIR=/fast/ssd/uv-cache
```

## Decision Framework

### Choose Poetry Despite Issues When:
- Complete project lifecycle management required
- Team already invested in Poetry workflows
- Package publishing to PyPI is primary use case
- Complex dependency groups needed

### Choose uv Despite Issues When:
- Performance is critical priority
- Simple application development focus
- pip-compatible workflows preferred  
- CI/CD optimization is important

### Hybrid Approach:
- Development: uv for speed and iteration
- Publishing: Poetry for complete toolchain
- CI/CD: uv for build performance
- Production: Whatever works reliably

Understanding these limitations helps set appropriate expectations and plan workarounds for successful Python project management with modern tooling.