# Cross-Machine Setup Guide

This guide helps you use the Scientific Assistant on both local and HPC machines with GitHub sync.

## Prerequisites

- GitHub account
- SSH access to your HPC machine
- Git configured on both machines

## Setup Steps

### 1. Initialize Git Repository (Local Machine)

```bash
cd ~/.config/goose/recipes/scientific-assistant

# Initialize and make first commit
git init
git add .
git commit -m "Initial scientific assistant knowledge base"
```

### 2. Create GitHub Repository

**Option A: Via GitHub Web Interface**
1. Go to https://github.com/new
2. Create a **private** repository (recommended for research notes)
3. Name it: `scientific-assistant` or `e3sm-research-kb`
4. Don't initialize with README (we already have files)
5. Copy the repository URL

**Option B: Via GitHub CLI** (if you have `gh` installed)
```bash
cd ~/.config/goose/recipes/scientific-assistant
gh repo create scientific-assistant --private --source=. --push
```

### 3. Connect Local Repository to GitHub

```bash
cd ~/.config/goose/recipes/scientific-assistant

# Add remote (replace with your GitHub username)
git remote add origin git@github.com:YOUR_USERNAME/scientific-assistant.git

# Or use HTTPS if you prefer:
# git remote add origin https://github.com/YOUR_USERNAME/scientific-assistant.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### 4. Install Goose on HPC

SSH into your HPC machine and follow these steps:

```bash
# Load Python module (adjust for your HPC)
module load python3  # or: module load anaconda3

# Install Goose
pip install --user goose-ai

# Or in a conda/virtual environment (recommended):
conda create -n goose python=3.10
conda activate goose
pip install goose-ai

# Verify installation
goose --version
```

**Add to your HPC `.bashrc` or `.bash_profile`:**
```bash
# For pip --user install
export PATH="$HOME/.local/bin:$PATH"

# For conda environment
# conda activate goose  # Uncomment if using conda
```

### 5. Clone Knowledge Base on HPC

```bash
# On HPC machine
mkdir -p ~/.config/goose/recipes
cd ~/.config/goose/recipes

# Clone your repository
git clone git@github.com:YOUR_USERNAME/scientific-assistant.git
# Or with HTTPS:
# git clone https://github.com/YOUR_USERNAME/scientific-assistant.git

# Make sync script executable
cd scientific-assistant
chmod +x sync-knowledge.sh
```

### 6. Configure Git on HPC (if needed)

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set up SSH key for GitHub (if using SSH)
ssh-keygen -t ed25519 -C "your.email@example.com"
cat ~/.ssh/id_ed25519.pub
# Copy this and add to GitHub: https://github.com/settings/keys
```

## Daily Workflow

### On Local Machine

```bash
# Before starting work - get latest from HPC
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh

# Work with the assistant (use appropriate command for your Goose version)
goose session --recipe scientific-assistant
# OR for older Goose: goose run --recipe scientific-assistant

# After planning experiments - sync to GitHub
./sync-knowledge.sh
```

### On HPC Machine

```bash
# Before working - get latest from local
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh

# Use the assistant (check your Goose version)
goose run --recipe scientific-assistant
# OR for newer Goose: goose session --recipe scientific-assistant

# Or just run your E3SM cases

# After experiments - update knowledge and sync
./sync-knowledge.sh
```

> **Note:** Goose command syntax changed between versions. Older versions use `goose run --recipe` while newer versions support `goose session --recipe`. Both work with the same recipe files.

## Sync Script Usage

The `sync-knowledge.sh` script automatically:
1. Commits any local changes with timestamp
2. Pulls remote changes (from other machine)
3. Pushes local changes to GitHub
4. Handles merge conflicts gracefully

```bash
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh
```

## Best Practices

### 1. Sync Often
- **Before** starting work on either machine
- **After** completing experiments or planning sessions
- **Daily** if actively using both machines

### 2. Use Descriptive Commits
If you prefer manual commits over the script:
```bash
git add .
git commit -m "Documented ELM carbon cycle test - successful run"
git push
```

### 3. Handle Conflicts
If both machines edit the same file:
```bash
# Pull first
git pull

# If conflicts occur, edit the conflicted files
# Look for <<<<<<< HEAD markers
vim knowledge/e3sm_experiments.md

# After resolving
git add .
git commit -m "Resolved sync conflicts"
git push
```

### 4. Keep Recipe Updated
When Goose or E3SM updates, you might want to enhance the recipe:
```bash
# Edit recipe.md with improvements
vim recipe.md
git add recipe.md
git commit -m "Updated recipe with new E3SM features"
git push
```

## Troubleshooting

### Can't Push to GitHub
```bash
# Check remote URL
git remote -v

# If using HTTPS, you may need a personal access token
# Generate at: https://github.com/settings/tokens

# Or switch to SSH
git remote set-url origin git@github.com:YOUR_USERNAME/scientific-assistant.git
```

### Merge Conflicts
```bash
# See what changed
git status
git diff

# Accept their version
git checkout --theirs path/to/file

# Accept our version
git checkout --ours path/to/file

# After resolving
git add .
git commit
```

### HPC Module Issues
Some HPCs have specific requirements:
```bash
# Load required modules (check HPC docs)
module load git
module load python/3.10
module load gcc  # May be needed for some Python packages

# List available modules
module avail python
module avail git
```

### Goose Not Found on HPC
```bash
# Check if in PATH
which goose

# If installed with --user, add to PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Or use full path
~/.local/bin/goose --version
```

## Advanced: Automatic Sync

### Cron Job (HPC)
To automatically sync after each session:
```bash
# Edit crontab
crontab -e

# Add line to sync every hour
0 * * * * cd ~/.config/goose/recipes/scientific-assistant && ./sync-knowledge.sh >> ~/sync.log 2>&1
```

### Git Hooks
To sync automatically after commits:
```bash
cd ~/.config/goose/recipes/scientific-assistant/.git/hooks

# Create post-commit hook
cat > post-commit << 'EOF'
#!/bin/bash
git push
EOF

chmod +x post-commit
```

## Security Note

Since this contains research notes and experiment details:
- ✅ Use a **private** GitHub repository
- ✅ Keep sensitive data (passwords, API keys) out of knowledge base
- ✅ Consider institutional policies about cloud storage
- ✅ Use SSH keys instead of password authentication

## Quick Reference

```bash
# Setup (one-time per machine)
git clone git@github.com:USERNAME/scientific-assistant.git
cd scientific-assistant && chmod +x sync-knowledge.sh

# Daily workflow
./sync-knowledge.sh                          # Before work
goose session --recipe scientific-assistant  # Do work
./sync-knowledge.sh                          # After work

# Check status
git status          # See what changed
git log --oneline   # See recent history
git diff            # See detailed changes
```

## Support

If you encounter issues:
1. Check GitHub repository permissions
2. Verify SSH key setup: `ssh -T git@github.com`
3. Test git operations manually before using script
4. Review HPC documentation for module loading
5. Check Goose installation: `goose --version`

## Goose Version Compatibility

### Recipe Command Differences

The command to use a recipe changed between Goose versions:

**Older Goose (likely on HPC):**
```bash
goose run --recipe scientific-assistant
```

**Newer Goose (likely on local machine):**
```bash
goose session --recipe scientific-assistant
```

Both commands work with the same recipe files - just the CLI syntax changed.

## Goose Version Compatibility

### Recipe Command Differences

The command to use a recipe changed between Goose versions:

**Older Goose (likely on HPC):**
```bash
goose run --recipe scientific-assistant
```
Uses `recipe.yaml` file (YAML format)

**Newer Goose (likely on local machine):**
```bash
goose session --recipe scientific-assistant
```
Uses `recipe.md` file (Markdown format)

Both commands work with the same recipe directory - Goose automatically picks the right format file. This recipe includes both `recipe.yaml` and `recipe.md` for maximum compatibility.

### Check Your Version

```bash
goose --version
```

### Interactive vs Non-Interactive

**`goose run`** (older or for automation):
- Can be used non-interactively with `-i` or `-t` flags
- Supports `--recipe` flag for recipe-based sessions
- Good for scripts and automation

**`goose session`** (newer, recommended for interactive use):
- Better session management
- Built-in session resume/fork/edit
- Named sessions for organization
- Currently recipes may not work directly (check version)

### Recommendation for HPC

If your HPC has an older Goose version, use:
```bash
goose run --recipe scientific-assistant
```

If you install the latest Goose, you can use:
```bash
goose session --recipe scientific-assistant
```

The recipe works identically with both commands!

---

## IMPORTANT: Recipe File Location for Older Goose

### The Problem

Older Goose versions look for `scientific-assistant.yaml` directly in the recipes directory:
```
~/.config/goose/recipes/scientific-assistant.yaml
```

NOT in a subdirectory:
```
~/.config/goose/recipes/scientific-assistant/recipe.yaml  ❌ Older Goose can't find this
```

### The Solution

**On HPC (one-time setup after cloning):**

```bash
cd ~/.config/goose/recipes
cp scientific-assistant/recipe.yaml scientific-assistant.yaml

# Verify it was created
ls -la scientific-assistant.yaml

# Now test - should work!
goose run --recipe scientific-assistant
```

### Final Directory Structure

```
~/.config/goose/recipes/
├── scientific-assistant.yaml           ← For older Goose (HPC) ✅
└── scientific-assistant/               ← Knowledge base directory
    ├── recipe.yaml                     ← Same content as above
    ├── recipe.md                       ← For newer Goose
    ├── knowledge/                      ← All knowledge files
    │   ├── e3sm_experiments.md
    │   ├── compset_usage.md
    │   └── ...
    ├── sync-knowledge.sh
    └── README.md
```

### Why This Structure?

- **Knowledge base**: Organized in subdirectory with git
- **Recipe file**: Copied to parent directory for older Goose to find
- **Sync script**: Automatically maintains both copies (local only)
- **HPC**: Needs manual copy once after initial clone

### After Initial Setup

On local machine, the `sync-knowledge.sh` script automatically maintains both locations.

On HPC, you only need to copy the recipe file ONCE during initial setup. After that, just use `git pull` to update the knowledge base.
