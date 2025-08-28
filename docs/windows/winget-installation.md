# Installing winget Package Manager

Guide to install and set up Microsoft's official Windows Package Manager.

## What is winget?

winget is Microsoft's official command-line package manager for Windows. Built into Windows 11 and available for Windows 10.

## Prerequisites

- Windows 10 (version 1809+) or Windows 11
- Microsoft Store access
- Internet connection

## Windows 11 Installation

**winget is pre-installed on Windows 11.**

**Verify it's working:**

```cmd
winget --version
```

**If not available, update via Microsoft Store:**
1. Open Microsoft Store
2. Search "App Installer"
3. Click "Update"

## Windows 10 Installation

### Method 1: Microsoft Store

**Install App Installer:**
1. Open Microsoft Store
2. Search "App Installer"
3. Click "Install"

**Verify installation:**
```cmd
winget --version
```

### Method 2: Direct Download

**Download installer:**

```powershell
# Download latest winget release
$url = "https://github.com/microsoft/winget-cli/releases/latest/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"
$output = "$env:TEMP\winget-installer.msixbundle"
Invoke-WebRequest -Uri $url -OutFile $output

# Install package
Add-AppxPackage -Path $output
```

### Method 3: Microsoft Official Link

```powershell
# Alternative download source
$url = "https://aka.ms/getwinget"
$output = "$env:TEMP\winget-installer.msixbundle"
Invoke-WebRequest -Uri $url -OutFile $output
Add-AppxPackage -Path $output
```

## Verify Installation

```cmd
# Check version
winget --version

# List installed packages
winget list

# Search for a package
winget search "Visual Studio Code"
```

**Expected output:**
```
v1.4.10173
...
Name               Id                     Version      Available Source
Visual Studio Code Microsoft.VisualStud…  1.74.2       1.74.3    winget
```

## Basic Usage

**Search packages:**
```cmd
winget search nodejs
```

**Install package:**
```cmd
winget install OpenJS.NodeJS
```

**List installed:**
```cmd
winget list
```

**Upgrade packages:**
```cmd
winget upgrade --all
```

## User vs System Installation

**Install for current user only:**
```cmd
winget install Microsoft.VisualStudioCode --scope user
```

**System-wide installation (requires admin):**
```cmd
winget install Microsoft.VisualStudioCode --scope machine
```

## Configuration

**View current settings:**
```cmd
winget settings
```

**Common settings file location:**
```
%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\settings.json
```

## Troubleshooting

**Command not found:**
- Ensure Windows 10 version 1809 or later
- Update Windows and Microsoft Store
- Restart command prompt/PowerShell

**Installation fails:**
- Check if Microsoft Store is blocked
- Try direct download method
- Ensure sufficient disk space

**Package not found:**
- winget repository is smaller than Chocolatey/Scoop
- Try alternative package managers for missing software

**Update issues:**
```cmd
# Reset winget sources
winget source reset --force
```

## Useful Commands

```cmd
# Show package information
winget show Microsoft.VisualStudioCode

# Install specific version
winget install Microsoft.VisualStudioCode --version 1.74.0

# Uninstall package
winget uninstall Microsoft.VisualStudioCode

# Export installed packages
winget export --output packages.json

# Import packages
winget import --import-file packages.json
```

## Next Steps

- [Essential winget packages](winget-essential-packages.md)
- [Compare with other package managers](package-managers-comparison.md)

## References

- [Official winget Documentation](https://docs.microsoft.com/en-us/windows/package-manager/)
- [winget GitHub Repository](https://github.com/microsoft/winget-cli)