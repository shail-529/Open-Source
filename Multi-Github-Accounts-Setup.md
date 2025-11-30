# Setting Up Multiple GitHub Accounts on the Same Laptop Using SSH

This guide will help you configure multiple GitHub accounts (e.g., personal and work) on the same laptop using SSH keys. You'll be able to push and pull from different repositories using different GitHub accounts seamlessly.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Understanding the Concept](#understanding-the-concept)
3. [Step-by-Step Setup](#step-by-step-setup)
4. [Practical Examples](#practical-examples)
5. [Troubleshooting](#troubleshooting)
6. [Quick Reference](#quick-reference)

---

## Prerequisites

- Git installed on your system
- Access to multiple GitHub accounts
- Basic understanding of terminal/command prompt
- Windows PowerShell or WSL (Windows Subsystem for Linux)

---

## Understanding the Concept

When using multiple GitHub accounts, the SSH key is what GitHub uses to identify you. Here's the key concept:

- **One SSH key per GitHub account** - Each GitHub account needs its own unique SSH key
- **SSH Config file** - Acts as a router, telling Git which SSH key to use for which GitHub account
- **Host aliases** - Custom names (like `github-personal`, `github-work`) that help Git identify which account to use
- **Remote URL** - The repository URL that includes the host alias

---

## Step-by-Step Setup

### Step 1: Generate SSH Keys for Each Account

You need to create a separate SSH key for each GitHub account.

#### For Account 1 (e.g., Personal - Shaileshukla529)

```powershell
ssh-keygen -t ed25519 -C "shailesh529" -f C:\Users\shail\.ssh\id_ed25519
```

- `-t ed25519`: Type of encryption (modern and secure)
- `-C "shailesh529"`: Comment to identify the key (use your GitHub username)
- `-f C:\Users\shail\.ssh\id_ed25519`: File path where the key will be saved

When prompted:
- Enter a strong passphrase (recommended for security)
- Confirm the passphrase

#### For Account 2 (e.g., Work - shail-529)

```powershell
ssh-keygen -t ed25519 -C "shail-529" -f C:\Users\shail\.ssh\id_ed25519_shail
```

> **Note**: The file name is different (`id_ed25519_shail`) to distinguish it from the first key.

---

### Step 2: View Your Public Keys

You need to add these public keys to their respective GitHub accounts.

```powershell
# View public key for Account 1
cat C:\Users\shail\.ssh\id_ed25519.pub

# View public key for Account 2
cat C:\Users\shail\.ssh\id_ed25519_shail.pub
```

Copy each public key output (starts with `ssh-ed25519 AAAA...`).

---

### Step 3: Add SSH Keys to GitHub Accounts

For each GitHub account:

1. Log into the GitHub account
2. Go to **Settings** → **SSH and GPG keys**
3. Click **New SSH key**
4. Give it a descriptive title (e.g., "Windows Laptop - Personal")
5. Paste the corresponding public key
6. Click **Add SSH key**

---

### Step 4: Create SSH Config File

The SSH config file tells your system which SSH key to use for which GitHub account.

**Location**: `C:\Users\shail\.ssh\config` (Windows) or `~/.ssh/config` (Linux/Mac)

**Create or edit the config file**:

```powershell
notepad C:\Users\shail\.ssh\config
```

**Add the following content**:

```
# Personal GitHub Account (Shaileshukla529)
Host github-shailesh529
    HostName github.com
    User git
    IdentityFile C:\Users\shail\.ssh\id_ed25519
    IdentitiesOnly yes

# Work GitHub Account (shail-529)
Host github-shail
    HostName github.com
    User git
    IdentityFile C:\Users\shail\.ssh\id_ed25519_shail
    IdentitiesOnly yes
```

**Explanation**:
- `Host github-shailesh529` - Custom alias for your personal account
- `HostName github.com` - Actual GitHub server
- `User git` - Always use `git` as the user for GitHub
- `IdentityFile` - Path to the SSH private key for this account
- `IdentitiesOnly yes` - Forces SSH to only use this specific key

---

### Step 5: Add SSH Keys to SSH Agent

The SSH agent manages your SSH keys and passphrases.

```powershell
# Start the SSH agent (if not running)
# This is usually automatic on Windows

# Add the first key (Personal)
ssh-add C:\Users\shail\.ssh\id_ed25519

# Add the second key (Work)
ssh-add C:\Users\shail\.ssh\id_ed25519_shail

# List all added keys to verify
ssh-add -l
```

**Expected output**:
```
256 SHA256:ewcMQH+3... shailesh529 (ED25519)
256 SHA256:XTHDpUGX... shail-529 (ED25519)
```

> **Tip**: If you restart your computer, you may need to re-add the keys to the SSH agent.

---

### Step 6: Test SSH Connections

Verify that each SSH key works with its respective GitHub account.

```powershell
# Test Personal Account
ssh -T git@github-shailesh529

# Test Work Account
ssh -T git@github-shail
```

**Expected successful output**:
```
Hi Shaileshukla529! You've successfully authenticated, but GitHub does not provide shell access.
```

or

```
Hi shail-529! You've successfully authenticated, but GitHub does not provide shell access.
```

> **Note**: If you see "Permission denied (publickey)", double-check that you've added the correct public key to GitHub.

---

## Practical Examples

### Example 1: Using Personal Account in a Folder

Let's say you have a folder `C:\Projects\PersonalProjects` and you want to use your personal GitHub account.

#### Initialize a new repository

```powershell
cd C:\Projects\PersonalProjects
git init

# Configure user details for this repository
git config user.name "Shaileshukla529"
git config user.email "your-personal-email@example.com"

# Create a README file
echo "# My Personal Project" > README.md
git add README.md
git commit -m "Initial commit"
```

#### Connect to GitHub using Personal Account

```powershell
# Set the remote URL using the personal host alias
git remote add origin git@github-shailesh529:Shaileshukla529/your-repo-name.git

# Push to GitHub
git push -u origin main
```

**Key Point**: Notice `git@github-shailesh529` - this matches the `Host` in your SSH config, which uses your personal SSH key.

---

### Example 2: Using Work Account in a Different Folder

Now let's say you have a folder `C:\Projects\WorkProjects` for work-related repositories.

#### Clone an existing work repository

```powershell
cd C:\Projects\WorkProjects

# Clone using the work host alias
git clone git@github-shail:shail-529/work-repo-name.git

cd work-repo-name

# Configure user details for this repository
git config user.name "shail-529"
git config user.email "your-work-email@company.com"
```

#### Or initialize a new repository

```powershell
cd C:\Projects\WorkProjects
git init

# Configure user details
git config user.name "shail-529"
git config user.email "your-work-email@company.com"

# Create files and commit
echo "# Work Project" > README.md
git add README.md
git commit -m "Initial commit"

# Set the remote URL using the work host alias
git remote add origin git@github-shail:shail-529/your-repo-name.git

# Push to GitHub
git push -u origin main
```

**Key Point**: Notice `git@github-shail` - this matches the `Host` for your work account in SSH config.

---

### Example 3: Switching Between Accounts in the Same Repository

If you accidentally set the wrong remote URL, you can change it:

```powershell
# Check current remote URL
git remote -v

# Change to personal account
git remote set-url origin git@github-shailesh529:Shaileshukla529/repo-name.git

# OR change to work account
git remote set-url origin git@github-shail:shail-529/repo-name.git
```

---

## Troubleshooting

### Issue 1: "Permission denied (publickey)"

**Cause**: SSH key not added to GitHub or SSH agent not using the correct key.

**Solution**:
1. Verify the public key is added to the correct GitHub account
2. Test the connection: `ssh -T git@github-shailesh529`
3. Make sure the SSH key is added to SSH agent: `ssh-add -l`

---

### Issue 2: "Could not resolve hostname github-personal"

**Cause**: SSH config file doesn't exist or has incorrect host alias.

**Solution**:
1. Verify the config file exists: `cat C:\Users\shail\.ssh\config`
2. Make sure the `Host` name in config matches what you're using in the remote URL
3. Example: If config says `Host github-work`, use `git@github-work:username/repo.git`

---

### Issue 3: Wrong account being used

**Cause**: Git is using the wrong SSH key or you haven't configured user details locally.

**Solution**:
1. Check which key is being used: `ssh -vT git@github-shailesh529`
2. Verify remote URL uses the correct host alias: `git remote -v`
3. Update local git config:
   ```powershell
   git config user.name "YourUsername"
   git config user.email "your-email@example.com"
   ```

---

### Issue 4: Need to re-enter passphrase every time

**Cause**: SSH keys not added to SSH agent.

**Solution**:
```powershell
# Add keys to SSH agent
ssh-add C:\Users\shail\.ssh\id_ed25519
ssh-add C:\Users\shail\.ssh\id_ed25519_shail
```

---

### Issue 5: SSH Agent not remembering keys after restart

**Solution**: You can create a startup script or use Windows OpenSSH service:

```powershell
# Configure Git to use Windows OpenSSH
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```

---

## Quick Reference

### Remote URL Format

```
git@<HOST_ALIAS>:<USERNAME>/<REPO_NAME>.git
```

**Examples**:
- Personal: `git@github-shailesh529:Shaileshukla529/my-repo.git`
- Work: `git@github-shail:shail-529/my-repo.git`

---

### Common Commands

```powershell
# List SSH keys in agent
ssh-add -l

# Remove all keys from agent
ssh-add -D

# Add specific key
ssh-add C:\Users\shail\.ssh\id_ed25519_shail

# Test SSH connection
ssh -T git@github-shailesh529

# Check remote URL
git remote -v

# Change remote URL
git remote set-url origin git@github-shail:shail-529/repo.git

# Configure local repository user
git config user.name "YourName"
git config user.email "your-email@example.com"

# View local repository config
git config --list --local
```

---

### SSH Config Template

```
# Account 1
Host github-account1
    HostName github.com
    User git
    IdentityFile C:\Users\YourUsername\.ssh\id_ed25519_account1
    IdentitiesOnly yes

# Account 2
Host github-account2
    HostName github.com
    User git
    IdentityFile C:\Users\YourUsername\.ssh\id_ed25519_account2
    IdentitiesOnly yes
```

---

## Summary

The key to managing multiple GitHub accounts is:

1. **Different SSH keys** for each account (`id_ed25519`, `id_ed25519_shail`)
2. **SSH config file** that maps host aliases to specific SSH keys
3. **Remote URLs** that use the host aliases (`git@github-work`, `git@github-personal`)
4. **Local git config** (name and email) set per repository

Once set up correctly, you can work on different projects using different GitHub accounts from the same laptop without any conflicts!

---

## Additional Tips

- **Use descriptive host aliases**: Instead of `github-personal`, you could use `github-myname` or `github-company`
- **Keep SSH keys secure**: Never share your private keys (files without `.pub` extension)
- **Use passphrases**: Always protect your SSH keys with strong passphrases
- **Backup your keys**: Store encrypted backups of your SSH keys in a secure location
- **Global vs Local config**: Use `git config --global` for default settings, but override per repository with `git config --local` (without `--global`) when needed

---

**Last Updated**: November 2025
