# Python Project Setup: Best Practices Guide

Cross-platform approach for Python virtual environment, dependency, and packaging management optimized for modern development workflows.

## Executive Summary

**Recommended approach for most teams:**
- **New application projects:** uv for development, Poetry for publishing (if needed)
- **Library development:** Poetry for complete lifecycle management
- **Legacy projects:** Gradual migration from conda to modern tools
- **Corporate environments:** Hybrid approach based on constraints

## Tool Selection Framework

### Decision Matrix

| Project Type | Primary Tool | Secondary Tool | Rationale |
|--------------|-------------|----------------|-----------|
| **Web applications** | uv | Poetry (publishing) | Speed, simplicity, pip compatibility |
| **Data analysis** | uv | Conda (scientific deps) | Performance + scientific computing |
| **Machine learning** | uv | Conda (GPU/CUDA) | Development speed + GPU toolchain |
| **Python libraries** | Poetry | uv (development) | Complete packaging workflow |
| **Corporate legacy** | Conda | uv (new projects) | Gradual migration strategy |
| **Research projects** | uv | Poetry (reproducibility) | Fast iteration + publication needs |

### Performance vs Features Trade-off

```
High Performance ←─────────────────→ Rich Features
uv ──────── Hybrid Approach ──────── Poetry ──────── Conda
```

**Choose based on priority:**
- **Performance critical:** uv → Hybrid → Poetry → Conda
- **Feature complete:** Conda → Poetry → Hybrid → uv
- **Balanced:** Hybrid approach (uv development + Poetry publishing)

*Performance rankings based on verified benchmarks from [Astral's uv documentation](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md), [Streamlit Cloud's production data](https://blog.streamlit.io/python-pip-vs-astral-uv/), and [Anaconda's performance analysis](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/conda-performance.html).*

## Cross-Platform Setup Strategy

### 1. Foundation Tools Installation

**Windows (via Scoop):**
```powershell
# Install Python version manager and tools
scoop install python uv
scoop install poetry  # If needed for publishing

# Verify installations
python --version && uv --version && poetry --version
```

**macOS (via Homebrew):**
```bash
# Install modern Python tools
brew install python@3.11 uv
brew install poetry  # If needed

# Add to PATH if needed
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
```

**Linux (Ubuntu/Debian):**
```bash
# Install via package managers or official installers
curl -LsSf https://astral.sh/uv/install.sh | sh
curl -sSL https://install.python-poetry.org | python3 -

# System packages for build dependencies
sudo apt update && sudo apt install -y build-essential python3-dev
```

*Installation methods verified from [uv's official installation guide](https://docs.astral.sh/uv/getting-started/installation/) and [Poetry's installation documentation](https://python-poetry.org/docs/#installation).*

### 2. Unified Project Structure

**Recommended directory layout:**
```
my-project/
├── .envrc                    # direnv configuration (optional)
├── .gitignore               # Python + IDE ignores
├── .python-version          # Python version specification
├── README.md                # Project documentation
├── pyproject.toml           # Project configuration (PEP 518)
├── uv.lock                  # uv lock file
├── poetry.lock              # Poetry lock file (if using hybrid)
├── requirements.txt         # Generated for deployment
├── requirements-dev.txt     # Generated dev requirements
├── src/                     # Source code
│   └── myproject/
│       ├── __init__.py
│       └── main.py
├── tests/                   # Test code
├── docs/                    # Documentation
└── scripts/                 # Utility scripts
```

### 3. Environment Configuration

**pyproject.toml (universal configuration):**
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-project"
version = "0.1.0"
description = "Project description"
authors = [{name = "Your Name", email = "email@example.com"}]
dependencies = [
    "requests>=2.31.0",
    "pydantic>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=23.0.0",
    "ruff>=0.1.0",
    "mypy>=1.0.0",
]
web = [
    "fastapi>=0.104.0",
    "uvicorn>=0.24.0",
]

[project.scripts]
my-project = "myproject.main:main"

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.black]
line-length = 88
target-version = ['py311']

[tool.mypy]
python_version = "3.11"
warn_return_any = true
strict = true
```

## Recommended Workflows by Project Type

### Application Development (uv-first approach)

**Project initialization:**
```bash
# Create project structure
mkdir my-app && cd my-app
echo "3.11" > .python-version

# Initialize uv environment
uv venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows

# Install dependencies
uv add "fastapi>=0.104.0" "uvicorn>=0.24.0"
uv add "pytest>=7.0.0" "black>=23.0.0" --dev

# Generate requirements files for deployment
uv pip freeze > requirements.txt
```

**Development workflow:**
```bash
# Daily development
uv add requests pydantic           # Add production dependency
uv add pytest-cov --dev          # Add development dependency
uv sync                           # Install all dependencies

# Run application
python -m myapp.main

# Testing
python -m pytest

# Code formatting
black src/ tests/
ruff check src/ tests/
```

### Library Development (Poetry-first approach)

**Project initialization:**
```bash
# Create library project
poetry new my-library
cd my-library

# Configure for development
poetry config virtualenvs.in-project true
poetry install

# Add dependencies
poetry add requests "pydantic>=2.0.0"
poetry add pytest black ruff mypy --group dev
```

**Development and publishing workflow:**
```bash
# Development
poetry run pytest
poetry run black src/ tests/
poetry run mypy src/

# Version management
poetry version patch  # 0.1.0 → 0.1.1

# Build and publish
poetry build
poetry publish  # or poetry publish --repository testpypi
```

### Data Science Projects (Hybrid approach)

**Setup for scientific computing:**
```bash
# Base environment with conda for scientific dependencies
conda create -n datascience python=3.11
conda activate datascience
conda install numpy pandas matplotlib jupyter

# Use uv for additional Python packages
uv venv --system-site-packages  # Use conda packages
source .venv/bin/activate
uv add streamlit plotly scikit-learn

# Development dependencies
uv add jupyter-lab black ruff --dev
```

**Alternative pure-uv approach:**
```bash
# Modern data science stack with uv
uv venv && source .venv/bin/activate
uv add "numpy>=1.24.0" "pandas>=2.0.0" "matplotlib>=3.7.0"
uv add "jupyter>=1.0.0" "scikit-learn>=1.3.0"
uv add "plotly>=5.17.0" "streamlit>=1.28.0"

# Fast iteration for experiments
uv add seaborn           # Add visualization
uv add polars           # Alternative to pandas
```

### Corporate Environment Setup

**Proxy and certificate configuration:**
```bash
# Set environment variables
export HTTPS_PROXY=http://proxy.corp.com:8080
export HTTP_PROXY=http://proxy.corp.com:8080
export SSL_CERT_FILE=/path/to/corporate-ca-bundle.crt

# Configure uv
uv config set global.index-url https://pypi.internal.corp.com/simple/
uv config set global.trusted-host pypi.internal.corp.com

# Configure Poetry (if used)
poetry config repositories.corporate https://pypi.internal.corp.com/simple/
poetry config certificates.corporate.cert /path/to/corporate-ca-bundle.crt
```

**Air-gapped environment setup:**
```bash
# Download packages on connected machine
uv export --format requirements-txt > requirements.txt
pip download -r requirements.txt -d ./wheels

# Transfer wheels to air-gapped environment
uv venv && source .venv/bin/activate
uv pip install --find-links ./wheels -r requirements.txt
```

## Best Practices

### 1. Version Management

**Python version specification:**
```bash
# Use .python-version file for consistency
echo "3.11.6" > .python-version

# Tool compatibility
pyenv install 3.11.6    # pyenv
uv python install 3.11.6  # uv (when available)
```

**Dependency version strategies:**
```toml
# Production applications - pin precisely
dependencies = [
    "fastapi==0.104.1",
    "pydantic==2.5.0",
]

# Libraries - use compatible release
dependencies = [
    "requests>=2.31.0,<3.0.0",
    "pydantic~=2.5.0",  # Allows 2.5.x
]
```

### 2. Lock File Management

**uv lock file workflow:**
```bash
# Generate lock file
uv lock

# Install from lock file (production)
uv sync --frozen

# Update specific package
uv lock --upgrade-package requests

# Update all packages
uv lock --upgrade
```

**Cross-platform considerations:**
```bash
# Generate platform-specific lock files
uv lock --python-platform windows
uv lock --python-platform linux
uv lock --python-platform darwin
```

### 3. CI/CD Integration

**GitHub Actions example:**
```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install uv
        uses: astral-sh/setup-uv@v1
        with:
          version: "latest"
      
      - name: Set up Python
        run: uv python install 3.11
      
      - name: Install dependencies
        run: uv sync --all-extras --dev
      
      - name: Run tests
        run: uv run pytest
      
      - name: Build package
        run: uv build  # when supported
```

**Multi-platform testing:**
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    python-version: ["3.10", "3.11", "3.12"]
```

### 4. Docker Integration

**Optimized Dockerfile:**
```dockerfile
FROM python:3.11-slim

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

# Copy dependency files
COPY pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync --frozen --no-cache

# Copy application code
COPY src/ src/

# Run application
CMD ["uv", "run", "python", "-m", "myapp.main"]
```

**Multi-stage build for smaller images:**
```dockerfile
# Build stage
FROM python:3.11-slim as builder
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-dev

# Runtime stage
FROM python:3.11-slim
COPY --from=builder /.venv /.venv
COPY src/ src/
ENV PATH="/.venv/bin:$PATH"
CMD ["python", "-m", "myapp.main"]
```

## Migration Strategies

### From conda to uv

**1. Assess project dependencies:**
```bash
# Export conda environment
conda env export > environment.yml

# Analyze dependencies
grep -E "^  - " environment.yml | grep -v "pip:"
# Identify conda-specific packages (CUDA, scientific libraries)
```

**2. Gradual migration approach:**
```bash
# Keep conda for system dependencies
conda create -n base python=3.11 numpy pandas matplotlib
conda activate base

# Use uv for application dependencies
uv venv --system-site-packages
source .venv/bin/activate
uv add fastapi uvicorn pytest
```

**3. Full migration (Python-only projects):**
```bash
# Convert conda environment
conda env export --no-builds | grep -E "^  - [^=]+=" > conda-deps.txt

# Install with uv
uv venv && source .venv/bin/activate
# Manually convert conda-deps.txt to uv add commands
```

### From Poetry to uv

**1. Export dependencies:**
```bash
# Export Poetry dependencies
poetry export --format requirements.txt --output requirements.txt
poetry export --format requirements.txt --dev --output requirements-dev.txt

# Initialize uv
uv venv && source .venv/bin/activate
uv pip install -r requirements.txt
uv pip install -r requirements-dev.txt
```

**2. Update project configuration:**
```bash
# Keep Poetry for publishing, use uv for development
poetry config virtualenvs.create false
# uv manages the virtual environment

# Development workflow
uv add package-name  # Fast development
poetry add package-name  # Update pyproject.toml
```

## Troubleshooting Guide

### Common Issues and Solutions

**Environment activation problems:**
```bash
# Issue: Command not found after activation
source .venv/bin/activate
which python  # Should point to .venv/bin/python

# Solution: Verify virtual environment creation
uv venv --verbose
```

**Dependency resolution conflicts:**
```bash
# Issue: Conflicting package versions
uv add package-a package-b
# Error: Cannot resolve versions

# Solution: Use explicit versions
uv add "package-a>=1.0.0,<2.0.0" "package-b>=2.0.0,<3.0.0"
```

**Corporate proxy issues:**
```bash
# Issue: SSL certificate verification failed
# Solution: Configure certificates
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
```

## Conclusion

**Key recommendations:**
1. **Start with uv** for new Python applications - prioritize development speed
2. **Use Poetry** for library development and publishing workflows
3. **Keep conda** for complex scientific computing with system dependencies
4. **Adopt hybrid approaches** to balance performance and functionality
5. **Plan migrations gradually** - evaluate tooling impact on team productivity

This approach maximizes development velocity while maintaining production reliability and team collaboration effectiveness across different project types and organizational constraints.