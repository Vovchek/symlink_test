# Windows Git Symlinks Guide

This project demonstrates how to properly create and maintain symlinks in Git repositories on Windows.

## Prerequisites

1. **Enable Developer Mode** in Windows 11:
   - Go to `Settings > System > For Developers`
   - Turn on `Developer Mode`
   - This allows creating symlinks without administrator privileges

2. **Configure Git** to handle symlinks properly:
   ```powershell
   # For current repository only
   git config core.symlinks true
   
   # Or globally for all repositories
   git config --global core.symlinks true
   ```

## Creating Symlinks

Use PowerShell to create symlinks with **relative paths**:

```powershell
# Create a symbolic link
New-Item -ItemType SymbolicLink -Path "test_folder\README.md" -Target "..\README.md"
```

**Important:** Always use relative paths (e.g., `..\README.md`) instead of absolute paths. Absolute paths won't work across different systems or on GitHub.

## Committing Symlinks

After creating the symlink, add and commit it normally:

```powershell
git add test_folder\README.md
git commit -m "Add symlink to README"
git push
```

Git will store symlinks with mode `120000` (you can verify with `git ls-files -s`).

## Cloning Repositories with Symlinks

When cloning a repository that contains symlinks, use the `-c` flag:

```powershell
git clone -c core.symlinks=true https://github.com/Vovchek/symlink_test.git
```

Or if you've already set `core.symlinks=true` globally, regular clone works:

```powershell
git clone https://github.com/Vovchek/symlink_test.git
```

## Troubleshooting

### Symlinks appear as text files after cloning
- **Cause:** Git cloned without `core.symlinks=true`
- **Solution:** 
  1. Enable developer mode
  2. Set `git config core.symlinks true`
  3. Remove and re-clone the repository with the proper configuration

### "Administrator privilege required" error
- **Cause:** Developer Mode is not enabled
- **Solution:** Enable Developer Mode in Windows Settings

### GitHub shows symlinks as text
- This is normal GitHub behavior - it displays the symlink target as text in the web interface
- Symlinks work correctly when you clone the repository locally

## Verification

Check if a file is stored as a symlink in Git:

```powershell
git ls-files -s test_folder/README.md
```

If the output starts with `120000`, it's a proper symlink.
