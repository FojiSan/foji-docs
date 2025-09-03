# Installing Chocolatey Package Manager

Guide to install Chocolatey on Windows with focus on non-admin user installations.

## What is Chocolatey?

Chocolatey is a Windows package manager with the largest software repository. Can be installed system-wide (admin) or user-level (non-admin).

## Prerequisites

- Windows 10/11
- PowerShell 5.1+
- Internet connection

## Non-Admin Installation (Recommended)

**Open PowerShell as regular user:**

```powershell
# Set installation directory to user profile
$env:ChocolateyInstall = "$env:USERPROFILE\chocolatey"

# Install Chocolatey for current user only
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

## Verify Installation

```powershell
# Check Chocolatey version
choco --version

# List installed packages
choco list
```

**If command not found, add to PATH:**

```powershell
$userPath = [Environment]::GetEnvironmentVariable('PATH', 'User')
$chocoPath = "$env:USERPROFILE\chocolatey\bin"
[Environment]::SetEnvironmentVariable('PATH', "$userPath;$chocoPath", 'User')

# Restart PowerShell and try again
```

## Admin Installation (If Available)

**Run PowerShell as Administrator:**

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

## Basic Configuration

**Enable global confirmation to avoid prompts:**

```powershell
choco feature enable -n allowGlobalConfirmation
```

**Test installation:**

```powershell
# Install a simple package
choco install curl

# Verify installation
curl --version
```

## Package Installation Modes

**User mode (non-admin installs):**
```powershell
choco install packagename --user
```

**Force user directory:**
```powershell
choco install packagename --force-x86 --user --install-directory="$env:USERPROFILE\Tools"
```

## Troubleshooting

**Execution policy error:**
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
```

**PATH issues:**
- Restart PowerShell after installation
- Manually add chocolatey\bin to user PATH if needed

**User installation not working:**
- Some packages require admin rights regardless
- Use portable versions instead

**Corporate proxy:**
```powershell
choco config set proxy http://proxy:port
choco config set proxyUser username
choco config set proxyPassword password
```

## Common Commands

```powershell
# Search packages
choco search nodejs

# Install package
choco install nodejs

# Update package
choco upgrade nodejs

# Remove package
choco uninstall nodejs

# List outdated packages
choco outdated
```

## Next Steps

- [Essential Chocolatey packages](chocolatey-essential-packages.md)
- [Compare with other package managers](package-managers-comparison.md)

## References

- [Official Chocolatey Documentation](https://docs.chocolatey.org/)
- [Chocolatey Community Repository](https://community.chocolatey.org/packages)