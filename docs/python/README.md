# Python Virtual Environment and Dependency Management

Comprehensive guide to modern Python project management tools, comparing traditional approaches with cutting-edge alternatives for optimal development workflows.

## Overview

This section covers virtual environment, dependency, and packaging management for Python projects, with focus on performance, reproducibility, and cross-platform compatibility.

## Document Structure

### [Conda Virtual Environments](./conda-virtual-environments.md)
**Complete overview of conda for virtual environment management**
- How conda works for virtual environments
- Python project workflows with conda
- Performance limitations and corporate environment challenges
- When to keep using conda vs migrate to modern tools

### [Poetry vs uv Comparison](./poetry-vs-uv-comparison.md) 
**Objective analysis of modern Python tools with performance benchmarks**
- Feature comparison matrix between Poetry and uv
- Real-world performance metrics (8-15x speed improvements)
- Use case recommendations based on project requirements
- Integration analysis for CI/CD and development workflows

### [Known Issues and Gotchas](./known-issues-and-gotchas.md)
**Real-world problems, bugs, and workarounds for Poetry and uv**
- Common dependency resolution failures
- Corporate environment challenges (proxies, certificates)
- Platform-specific issues and compatibility problems
- Mitigation strategies and practical workarounds

### [Python Project Setup Guide](./python-project-setup-guide.md)
**Cross-platform best practices for Python project management**
- Tool selection framework based on project type
- Recommended workflows for different scenarios
- Corporate environment setup and migration strategies
- CI/CD integration and Docker optimization

## Quick Decision Guide

### For New Projects

**Application Development:** Use **uv** for fast development iteration
- 8-15x faster than conda
- Excellent pip compatibility
- Minimal configuration overhead

**Library Development:** Use **Poetry** for complete lifecycle management
- Integrated build and publishing workflow
- Comprehensive dependency group management
- Strong ecosystem adoption

**Data Science:** Use **hybrid approach** (uv + conda for system dependencies)
- uv for Python packages and development speed
- conda for CUDA, scientific computing, and multi-language dependencies

**Corporate Legacy:** Gradual migration from **conda** to modern tools
- Keep conda for established workflows
- Introduce uv for new projects
- Evaluate team constraints and IT policies

### Performance Comparison

| Operation | conda | Poetry | uv | Winner |
|-----------|--------|--------|-----|---------|
| Environment creation | 2-5 min | 45-120s | 8-15s | 🏆 uv |
| Dependency resolution | 3-8 min | 10-25s | 1-3s | 🏆 uv |
| Package installation | 1-3 min | 15-35s | 2-5s | 🏆 uv |
| CI/CD build time | 4-8 min | 2-4 min | 15-30s | 🏆 uv |

## Target Audience

**Scientists and Engineers:** New to software development, need practical solutions for computational work in corporate/academic environments with potential admin restrictions.

**Development Teams:** Looking to optimize Python project workflows, reduce CI/CD costs, and improve developer productivity while maintaining reproducibility.

**Corporate Environments:** Teams migrating from legacy conda workflows to modern tools, dealing with proxy/firewall restrictions, and air-gapped deployment requirements.

## Getting Started

1. **Read the decision guide** in [Python Project Setup Guide](./python-project-setup-guide.md)
2. **Understand limitations** in [Known Issues and Gotchas](./known-issues-and-gotchas.md)  
3. **Compare tools objectively** in [Poetry vs uv Comparison](./poetry-vs-uv-comparison.md)
4. **Learn conda context** in [Conda Virtual Environments](./conda-virtual-environments.md)

## Related Documentation

- [Windows Package Managers](../windows/package-managers-comparison.md) - Foundation tools for Python development on Windows
- [Scoop Advanced Usage](../windows/scoop-advanced-usage.md) - Installing Python tools without admin rights
- [Windows Development Setup](../windows/) - Platform-specific development environment configuration