# Installing Scoop Package Manager

Install Scoop on Windows without administrator privileges.

## What is Scoop?

Command-line package manager for Windows that installs programs to user directory. Designed for developers who need development tools without admin rights.

## Prerequisites

- Windows 10/11
- PowerShell 5.1+
- Internet connection
- Regular user account (no admin required)

## Installation

Open PowerShell and execute:

```powershell
# Set execution policy for current user
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Install Scoop
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

## Verify Installation

```powershell
# Check if Scoop is working
scoop --version

# See where Scoop is installed
scoop prefix scoop
```

**Expected output:**
```
Current Scoop version:
v0.3.1 - Released at 2023-XX-XX

C:\Users\{username}\scoop\apps\scoop\current
```

## Alternative Methods

### Custom Installation Directory

```powershell
# Install to different location (optional)
$env:SCOOP = 'D:\Tools\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')

# Then install normally
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

### Behind Corporate Proxy

```powershell
# Configure proxy first
[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

# Then install
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

## Basic Setup

**Add useful package repositories:**

```powershell
scoop bucket add extras
scoop bucket add versions
```

**Test installation with a simple package:**

```powershell
scoop install curl
curl --version
```

## Troubleshooting

**PowerShell execution policy error:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

**Installation directory permissions:**
- Scoop installs to `%USERPROFILE%\scoop` by default
- No admin rights needed for user directory

**Network issues:**
- Check internet connection
- Try using different DNS (8.8.8.8)
- Contact IT if corporate firewall blocks installation

## Next Steps

- [Install development tools with Scoop](scoop-essential-packages.md)
- [Compare with other package managers](package-managers-comparison.md)

## References

- [Official Scoop Documentation](https://scoop.sh/)
- [Scoop GitHub Repository](https://github.com/ScoopInstaller/Scoop)