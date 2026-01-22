# Troubleshooting Guide

This guide helps resolve common issues when working with this repository.

## Git Issues

### "git add ." Says Files Don't Exist

**Problem**: When running `git add .`, you get errors saying files or directories don't exist, especially when VS Code or the project is on a USB drive.

**Common Causes**:

1. **USB Drive File System Issues**
   - USB drives often use FAT32 or ExFAT file systems which have limitations
   - Case sensitivity issues between operating systems
   - Symbolic links may not work properly on USB drives
   - File path length limitations on certain file systems

2. **VS Code Workspace Path Issues**
   - VS Code creates temporary files and folders that may not sync properly on USB drives
   - `.vscode` folder permissions may be incorrect
   - Workspace file paths may contain characters incompatible with the USB drive file system

3. **Git Configuration Issues**
   - Line ending differences (CRLF vs LF)
   - File permission tracking issues on non-Unix file systems

**Solutions**:

#### Solution 1: Check Your Current Directory
```bash
# Verify you're in the correct repository directory
pwd

# Verify it's a git repository
git status

# List all files to confirm they exist
ls -la
```

#### Solution 2: Check Git Status First
```bash
# See what files Git recognizes
git status

# See what files exist in your working directory
ls -la

# Add files individually if "git add ." fails
git add filename.ext
```

#### Solution 3: Fix File Path Issues
```bash
# If on Windows with USB drive, try using forward slashes
git add ./

# Or add files by explicit paths
git add README.md
git add src/
```

#### Solution 4: Configure Git for USB Drive Usage
```bash
# Disable file mode tracking (permissions)
git config core.fileMode false

# Set appropriate line ending handling
# For Windows:
git config core.autocrlf true

# For Mac/Linux:
git config core.autocrlf input

# Refresh the repository index
git rm -r --cached .
git add .
```

#### Solution 5: Copy Project to Local Drive
The most reliable solution when working with USB drives:

1. **Copy the entire project folder** from the USB drive to your local hard drive
   - Windows: `C:\Users\YourUsername\Projects\slam-website`
   - Mac: `~/Projects/slam-website`
   - Linux: `~/projects/slam-website`

2. **Work from the local copy** for development

3. **Backup to USB drive periodically** or use Git to push to a remote repository

4. **Push to GitHub** regularly instead of relying on USB drive backups:
   ```bash
   git push origin main
   ```

#### Solution 6: Check for Hidden Characters or Spaces
```bash
# Check if there are any unusual characters in filenames
ls -lab

# Rename files with problematic characters
# Before: "my file.txt"
# After: "my_file.txt"
mv "my file.txt" my_file.txt
```

#### Solution 7: Verify .gitignore Isn't Excluding Everything
```bash
# View your .gitignore file
cat .gitignore

# Check what files are being ignored
git status --ignored

# If too many files are ignored, review and update .gitignore
```

### Best Practices for USB Drive Development

❌ **Not Recommended**:
- Running VS Code directly from USB drive
- Keeping your only copy of code on a USB drive
- Using USB drives with FAT32 file system for development

✅ **Recommended**:
- Work from local hard drive
- Use Git with remote repository (GitHub, GitLab, etc.)
- Use USB drive only for backups
- Use cloud storage or Git hosting for primary backup
- Use portable SSD with modern file system instead of USB thumb drive

### Still Having Issues?

If none of the above solutions work:

1. **Check file permissions**:
   ```bash
   ls -la
   # Ensure you have read/write permissions
   ```

2. **Verify Git installation**:
   ```bash
   git --version
   which git
   ```

3. **Try cloning the repository fresh**:
   ```bash
   cd ~/Projects
   git clone <repository-url>
   cd slam-website
   ```

4. **Check for corrupted Git repository**:
   ```bash
   git fsck
   ```

## Android/Kotlin Development Issues

### Build Issues

If you encounter build issues:

```bash
# Clean build
./gradlew clean build

# Or on Windows
gradlew.bat clean build
```

### Dependency Issues

If you have dependency resolution issues:

```bash
# Refresh dependencies
./gradlew --refresh-dependencies
```

## Need More Help?

If you continue to experience issues:

1. Check existing [GitHub Issues](https://github.com/Rahooty/slam-website/issues)
2. Create a new issue with:
   - Your operating system
   - Git version (`git --version`)
   - Exact error message
   - Steps to reproduce
   - Whether you're working from USB drive or local drive
