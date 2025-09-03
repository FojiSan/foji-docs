# Advanced Scoop Configuration and Usage

Guide to advanced Scoop usage for developers, researchers, and engineers who need more than basic package installation.

## Prerequisites

- Scoop already installed (see [Scoop Installation](scoop-installation.md))
- Basic familiarity with PowerShell
- Understanding of development environments

## Advanced Configuration

### Bucket Management

**Add essential buckets for development:**

```powershell
# Add commonly needed buckets
scoop bucket add extras        # GUI apps, fonts, other tools
scoop bucket add versions      # Multiple versions of packages
scoop bucket add nonportable   # Traditional installers
scoop bucket add java          # Java development tools
scoop bucket add php           # PHP development tools
scoop bucket add games         # Optional: development-related games/tools

# Verify added buckets
scoop bucket list
```

**Add specialized buckets:**

```powershell
# Scientific computing
scoop bucket add r-bucket https://github.com/cderv/r-bucket.git

# Data science tools
scoop bucket add data-science https://github.com/Ash258/scoop-data-science.git

# Graphics and multimedia
scoop bucket add nerd-fonts https://github.com/matthewjberger/scoop-nerd-fonts.git
```

### Fast Downloads with aria2

**Enable aria2 for significantly faster package downloads:**

```powershell
# Install aria2 download manager
scoop install aria2

# Configure aria2 for optimal performance
scoop config aria2-max-connection-per-server 16
scoop config aria2-split 16
scoop config aria2-min-split-size 4M
scoop config aria2-max-concurrent-downloads 8

# Verify aria2 configuration
scoop config
```

**aria2 Configuration Options:**

```powershell
# Maximum connections per server (default: 5, recommended: 8-16)
scoop config aria2-max-connection-per-server 16

# Split downloads into segments (default: 5, recommended: 8-16)
scoop config aria2-split 16

# Minimum size for splitting (default: 1M, recommended: 4M-8M)
scoop config aria2-min-split-size 4M

# Maximum concurrent downloads (default: 3, recommended: 4-8)
scoop config aria2-max-concurrent-downloads 8

# Enable or disable aria2
scoop config aria2-enabled true
```

**Corporate Network Considerations:**

```powershell
# If behind firewall, reduce connections to avoid blocking
scoop config aria2-max-connection-per-server 4
scoop config aria2-split 4

# Disable aria2 if causing network issues
scoop config aria2-enabled false
```

### Custom Installation Directory

**Configure Scoop for specific project organization:**

```powershell
# Set custom Scoop directory (before installation)
$env:SCOOP = 'D:\Development\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')

# Set custom global app directory (optional)
$env:SCOOP_GLOBAL = 'D:\Development\ScoopGlobal'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'User')
```

### Configuration File Customization

**Create custom Scoop configuration:**

```powershell
# Open Scoop config file
notepad "$env:USERPROFILE\.config\scoop\config.json"
```

**Example advanced configuration:**

```json
{
    "proxy": "http://proxy.company.com:8080",
    "default_architecture": "64bit",
    "debug": false,
    "force_update": false,
    "show_manifest": false,
    "shim": "kiennq",
    "root_path": "D:\\Development\\Scoop",
    "global_path": "D:\\Development\\ScoopGlobal",
    "cache_path": "D:\\Development\\ScoopCache",
    "gh_token": "your_github_token_here",
    "virustotal_api_key": "your_vt_key_here"
}
```

## Version Management

### Multiple Language Versions

**Python version management:**

```powershell
# Install multiple Python versions
scoop install python          # Latest Python 3.x
scoop install python27        # Python 2.7 (if needed)
scoop install python39        # Specific Python 3.9
scoop install python310       # Specific Python 3.10

# Switch between versions
scoop reset python39          # Use Python 3.9
scoop reset python            # Use latest Python

# Verify current version
python --version
```

**Node.js version management:**

```powershell
# Install multiple Node.js versions
scoop install nodejs          # Latest LTS
scoop install nodejs-lts      # Explicit LTS
scoop install nodejs16        # Node.js 16.x
scoop install nodejs18        # Node.js 18.x

# Switch versions
scoop reset nodejs16
node --version
```

### Development Environment Switching

**Create project-specific environments:**

```powershell
# Project A: Data Science with Python 3.9
scoop reset python39
pip install numpy pandas matplotlib jupyter

# Project B: Web Development with Node.js 18  
scoop reset nodejs18
npm install -g typescript @angular/cli

# Project C: Scientific Computing with R
scoop reset r
# R packages install to user library automatically
```

## Typical Developer Workflows

### Scientific Computing Setup

**Complete Python data science environment:**

```powershell
# Core tools
scoop install git python vscode

# Scientific Python distribution (alternative to manual pip installs)
scoop install anaconda3

# Additional scientific tools
scoop install r julia
scoop install pandoc          # Document conversion
scoop install graphviz        # Graph visualization
scoop install ffmpeg          # Media processing

# Jupyter extensions and tools
pip install jupyterlab
jupyter lab --generate-config
```

**R development environment:**

```powershell
# Install R and RStudio
scoop install r rstudio

# Install Rtools for package compilation
scoop install rtools

# Optional: R package development tools
scoop install pandoc
scoop install miktex          # For PDF output
```

### Web Development Setup

**Full-stack JavaScript environment:**

```powershell
# Core JavaScript tools
scoop install nodejs yarn git vscode

# Global development tools
npm install -g typescript
npm install -g @angular/cli @vue/cli create-react-app
npm install -g eslint prettier nodemon

# Database tools
scoop install postgresql mongodb
scoop install redis           # Caching/session store

# API testing and development
scoop install postman insomnia
```

### System Utilities and Tools

**Developer productivity tools:**

```powershell
# Terminal and shell enhancements
scoop install windows-terminal
scoop install starship         # Cross-shell prompt
scoop install ripgrep          # Better grep
scoop install fd               # Better find
scoop install bat              # Better cat with syntax highlighting
scoop install fzf              # Fuzzy finder

# File management
scoop install 7zip
scoop install everything       # Fast file search
scoop install treesizefree     # Disk usage analyzer

# Network and system tools
scoop install curl wget
scoop install jq               # JSON processor
scoop install yq               # YAML processor
scoop install sqlite           # Lightweight database
```

## Project-Specific Configurations

### Portable Development Environment

**Create a portable development setup:**

```powershell
# Install to specific directory for portability
$env:SCOOP = 'D:\PortableDev\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')

# Install minimal development stack
scoop install git python nodejs vscode-portable

# Create project structure
New-Item -ItemType Directory -Path 'D:\PortableDev\Projects' -Force
New-Item -ItemType Directory -Path 'D:\PortableDev\Scripts' -Force
```

**Create batch script for easy setup:**

```batch
@echo off
:: portable-dev-env.bat
set SCOOP=D:\PortableDev\Scoop
set PATH=%SCOOP%\shims;%SCOOP%\apps\git\current\bin;%PATH%

echo Portable Development Environment Ready!
echo Git: 
git --version
echo Python: 
python --version
echo Node.js: 
node --version

powershell.exe
```

### Research Project Environment

**Reproducible research setup:**

```powershell
# Document current environment
scoop export > my-research-environment.json

# Install research-specific tools
scoop install python r julia
scoop install latex miktex     # Academic document preparation
scoop install pandoc          # Format conversion
scoop install zotero           # Reference management

# Create research directory structure
New-Item -ItemType Directory -Path "$env:USERPROFILE\Research\Project2024" -Force
New-Item -ItemType Directory -Path "$env:USERPROFILE\Research\Project2024\data" -Force
New-Item -ItemType Directory -Path "$env:USERPROFILE\Research\Project2024\scripts" -Force
New-Item -ItemType Directory -Path "$env:USERPROFILE\Research\Project2024\outputs" -Force
```

## Maintenance and Optimization

### Regular Maintenance Tasks

**Weekly maintenance routine:**

```powershell
# Update all packages
scoop update *

# Clean package cache
scoop cache rm *

# Check for issues
scoop checkup

# Remove old versions
scoop cleanup *
```

**Monthly deep cleaning:**

```powershell
# Remove all cached downloads
scoop cache rm *

# Remove old versions of all apps
scoop cleanup * --global

# Reset apps that might have issues
scoop list | ForEach-Object { scoop reset $_.Name }

# Check for broken shortcuts or shims
scoop checkup
```

### Backup and Migration

**Export current setup:**

```powershell
# Export installed packages
scoop export > my-scoop-setup.json

# Backup Scoop directory (optional)
robocopy "$env:USERPROFILE\scoop" "D:\Backup\scoop" /MIR
```

**Restore on new machine:**

```powershell
# Install Scoop first, then:
scoop import my-scoop-setup.json
```

### Troubleshooting Advanced Issues

**Fix common Scoop problems:**

```powershell
# Reset all shims (if PATH issues)
scoop reset *

# Reinstall Git if Scoop updates fail
scoop uninstall git
scoop install git

# Clear corrupted cache
Remove-Item "$env:USERPROFILE\scoop\cache\*" -Recurse -Force
scoop cache rm *

# Fix permissions issues
scoop checkup
# Follow recommended fixes
```

**Proxy configuration for corporate networks:**

```powershell
# Set proxy in Scoop config
scoop config proxy http://proxy.company.com:8080

# Set proxy with authentication
scoop config proxy http://username:password@proxy.company.com:8080

# Remove proxy configuration
scoop config rm proxy
```

## Advanced Use Cases

### Multi-User Shared Development

**Install commonly used tools globally (requires admin):**

```powershell
# Run PowerShell as Administrator
sudo scoop install git python nodejs --global

# Regular users can then access these tools
# while maintaining personal Scoop installations
```

### Custom Package Creation

**Create your own package manifest:**

```powershell
# Create custom bucket for your packages
New-Item -ItemType Directory -Path "$env:USERPROFILE\my-scoop-bucket" -Force

# Add your custom bucket
scoop bucket add my-bucket "$env:USERPROFILE\my-scoop-bucket"
```

**Example custom manifest (save as `my-tool.json`):**

```json
{
    "version": "1.0.0",
    "description": "My custom development tool",
    "homepage": "https://example.com",
    "license": "MIT",
    "url": "https://example.com/releases/my-tool-1.0.0.zip",
    "hash": "sha256:...",
    "bin": "my-tool.exe",
    "checkver": "github",
    "autoupdate": {
        "url": "https://example.com/releases/my-tool-$version.zip"
    }
}
```

## Performance Tips

### Speed Up Scoop Operations

**aria2 should be configured (see Advanced Configuration section above). Here are additional optimizations:**

```powershell
# Check if aria2 is properly configured
scoop config | Select-String "aria2"

# If not configured, set optimal values
scoop config aria2-max-connection-per-server 16
scoop config aria2-split 16
scoop config aria2-min-split-size 4M
```

**Reduce disk usage:**

```powershell
# Set cache retention policy
scoop config cache_enabled false  # Disable cache entirely
# OR
scoop config cache_enabled true
scoop cache rm *                   # Clean cache regularly
```

## Conclusion

Advanced Scoop usage transforms it from a simple package manager into a powerful development environment management system. Key benefits:

- **Version management** for multiple language runtimes
- **Portable environments** for consistent development setups
- **Project isolation** through environment switching
- **Reproducible setups** through export/import functionality
- **Corporate-friendly** proxy and security configurations

These advanced features make Scoop particularly valuable for researchers, scientists, and engineers who need flexible, non-admin development environments on Windows systems.

## References

- [Scoop Wiki](https://github.com/ScoopInstaller/Scoop/wiki)
- [Available Buckets](https://github.com/ScoopInstaller/Scoop/blob/master/buckets.json)
- [Creating Custom Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)