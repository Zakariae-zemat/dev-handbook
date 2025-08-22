# Complete Git & GitHub Tutorial

## Table of Contents
1. [Git Basics & Setup](#git-basics--setup)
2. [Repository Management](#repository-management)
3. [Basic Workflow Commands](#basic-workflow-commands)
4. [Branching & Merging](#branching--merging)
5. [Remote Repository Operations](#remote-repository-operations)
6. [GitHub-Specific Operations](#github-specific-operations)
7. [Viewing History & Changes](#viewing-history--changes)
8. [Undoing Changes](#undoing-changes)
9. [Collaboration Workflows](#collaboration-workflows)
10. [Advanced Commands](#advanced-commands)

## Git Basics & Setup

### Initial Configuration
```bash
# Set your identity (required for commits)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name to 'main'
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"  # For VS Code
git config --global core.editor "nano"         # For nano

# View all configurations
git config --list
```

## Repository Management

### Creating and Cloning Repositories
```bash
# Initialize a new repository in current directory
git init

# Initialize with specific branch name
git init -b main

# Clone an existing repository
git clone https://github.com/username/repository.git

# Clone to a specific folder
git clone https://github.com/username/repository.git my-project

# Clone only the latest commit (shallow clone)
git clone --depth 1 https://github.com/username/repository.git
```

## Basic Workflow Commands

### The Core Git Workflow
```bash
# Check repository status
git status

# Add files to staging area
git add filename.txt           # Add specific file
git add .                     # Add all files in current directory
git add -A                    # Add all files in repository
git add *.js                  # Add all JavaScript files

# Remove files from staging area
git reset filename.txt        # Unstage specific file
git reset                     # Unstage all files

# Commit changes
git commit -m "Your commit message"
git commit -m "Add user authentication feature"

# Add and commit in one step (for tracked files only)
git commit -am "Update README file"

# Amend the last commit
git commit --amend -m "New commit message"
git commit --amend --no-edit   # Keep same message, just add staged changes
```

## Branching & Merging

### Branch Operations
```bash
# List all branches
git branch                    # Local branches
git branch -r                # Remote branches
git branch -a                # All branches

# Create new branch
git branch feature-login
git checkout -b feature-login  # Create and switch in one command
git switch -c feature-login   # Modern alternative

# Switch between branches
git checkout main
git switch main               # Modern alternative

# Delete branches
git branch -d feature-login   # Delete merged branch
git branch -D feature-login   # Force delete unmerged branch

# Rename current branch
git branch -m new-branch-name
```

### Merging
```bash
# Merge branch into current branch
git merge feature-login

# Merge with no fast-forward (creates merge commit)
git merge --no-ff feature-login

# Merge with custom message
git merge feature-login -m "Merge feature-login into main"

# Abort merge in case of conflicts
git merge --abort
```

## Remote Repository Operations

### Remote Management
```bash
# List remote repositories
git remote -v

# Add remote repository
git remote add origin https://github.com/username/repository.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repository.git

# Remove remote
git remote remove origin
```

### Push and Pull Operations
```bash
# Push to remote repository
git push origin main
git push origin feature-branch

# Push and set upstream branch
git push -u origin main
git push --set-upstream origin feature-branch

# Push all branches
git push --all origin

# Pull changes from remote
git pull origin main
git pull                      # Pull from tracked branch

# Fetch changes without merging
git fetch origin
git fetch --all              # Fetch from all remotes
```

## GitHub-Specific Operations

### Working with GitHub
```bash
# Push new repository to GitHub
git remote add origin https://github.com/username/repository.git
git push -u origin main

# Clone and navigate
git clone https://github.com/username/repository.git
cd repository

# Fork workflow - add upstream remote
git remote add upstream https://github.com/original-owner/repository.git
git fetch upstream
git checkout main
git merge upstream/main
```

### SSH Setup for GitHub
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add SSH key to ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Test SSH connection
ssh -T git@github.com

# Clone using SSH
git clone git@github.com:username/repository.git
```

## Viewing History & Changes

### Log and History
```bash
# View commit history
git log
git log --oneline            # Compact view
git log --graph              # Visual branch graph
git log --graph --oneline --all  # All branches with graph

# View specific number of commits
git log -5                   # Last 5 commits
git log --since="2 weeks ago"
git log --author="Your Name"

# View changes in commits
git show                     # Show latest commit
git show commit-hash         # Show specific commit
git show HEAD~1             # Show previous commit
```

### Differences
```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
git diff --cached

# Compare branches
git diff main..feature-branch
git diff main...feature-branch  # Show changes since branching

# Compare specific files
git diff filename.txt
git diff main feature-branch filename.txt
```

## Undoing Changes

### Various Ways to Undo
```bash
# Discard unstaged changes
git checkout -- filename.txt    # Specific file
git checkout -- .              # All files
git restore filename.txt       # Modern alternative

# Unstage files
git reset HEAD filename.txt
git restore --staged filename.txt  # Modern alternative

# Undo commits
git reset --soft HEAD~1        # Keep changes staged
git reset --mixed HEAD~1       # Keep changes unstaged (default)
git reset --hard HEAD~1        # Discard changes completely

# Revert commits (safe for shared repositories)
git revert HEAD                # Revert last commit
git revert commit-hash         # Revert specific commit

# Clean untracked files
git clean -f                   # Remove untracked files
git clean -fd                  # Remove untracked files and directories
git clean -n                   # Dry run (preview what will be removed)
```

## Collaboration Workflows

### Pull Request Workflow
```bash
# 1. Create feature branch
git checkout -b feature/user-authentication

# 2. Make changes and commit
git add .
git commit -m "Add user authentication system"

# 3. Push branch to origin
git push -u origin feature/user-authentication

# 4. Create Pull Request on GitHub
# 5. After review and merge, clean up
git checkout main
git pull origin main
git branch -d feature/user-authentication
```

### Keeping Fork Updated
```bash
# Add upstream remote (original repository)
git remote add upstream https://github.com/original/repository.git

# Fetch upstream changes
git fetch upstream

# Merge upstream changes
git checkout main
git merge upstream/main

# Push updated main to your fork
git push origin main
```

## .gitignore - Excluding Files

### What is .gitignore?
The `.gitignore` file tells Git which files and directories to ignore and not track. This is essential for excluding:
- Build artifacts and compiled files
- Dependencies (node_modules, vendor folders)
- Environment variables and secrets
- IDE/editor specific files
- Operating system files
- Log files and temporary files

### Creating and Using .gitignore
```bash
# Create .gitignore file in repository root
touch .gitignore

# Add .gitignore to repository
git add .gitignore
git commit -m "Add .gitignore file"
```

### Common .gitignore Patterns

#### Basic Syntax
```gitignore
# Comments start with #

# Ignore specific file
config.env
secrets.txt

# Ignore all files with specific extension
*.log
*.tmp
*.cache

# Ignore entire directory
node_modules/
build/
dist/

# Ignore files in specific directory
logs/*.log

# Ignore all .txt files in any directory
**/*.txt

# Ignore files only in root directory
/root-file.txt

# Negate pattern (don't ignore)
!important.log
*.log
!important.log  # This file will be tracked despite *.log rule
```

#### Common .gitignore Examples

**Most Common Patterns (Universal)**
```gitignore
# Environment files
.env
.env.local

# Dependencies
node_modules/
vendor/

# Build outputs
build/
dist/
target/

# IDE files
.vscode/
.idea/
*.swp

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Temporary files
*.tmp
*.temp
```

**Quick Language Examples**
```gitignore
# Python
__pycache__/
*.pyc
venv/

# Node.js
node_modules/
npm-debug.log

# Java
*.class
target/

# C/C++
*.o
*.exe
```

**Pro Tip**: Use https://gitignore.io to generate .gitignore files for your specific tech stack!

### Managing .gitignore

#### Check if file is ignored
```bash
# Check if specific file is ignored
git check-ignore filename.txt

# Show which .gitignore rule is causing file to be ignored
git check-ignore -v filename.txt

# List all ignored files
git status --ignored
```

#### Ignore already tracked files
```bash
# Remove file from tracking but keep local copy
git rm --cached filename.txt

# Remove entire directory from tracking
git rm -r --cached directory-name/

# After removing, add to .gitignore and commit
echo "filename.txt" >> .gitignore
git add .gitignore
git commit -m "Stop tracking filename.txt"
```

#### Update .gitignore for already tracked files
```bash
# If you add a file to .gitignore that's already tracked:
# 1. Add the pattern to .gitignore
echo
```bash
# Stash current changes
git stash
git stash push -m "Work in progress on feature X"

# List stashes
git stash list

# Apply stash
git stash apply              # Apply latest stash
git stash apply stash@{2}    # Apply specific stash

# Pop stash (apply and remove)
git stash pop

# Drop stash
git stash drop stash@{1}
git stash clear              # Remove all stashes
```

### Rebasing
```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (edit commit history)
git rebase -i HEAD~3         # Edit last 3 commits

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

### Cherry-picking
```bash
# Apply specific commit to current branch
git cherry-pick commit-hash

# Cherry-pick without committing
git cherry-pick -n commit-hash
```

### Tagging
```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0 release"

# List tags
git tag
git tag -l "v1.*"           # Pattern matching

# Push tags
git push origin v1.0.0      # Push specific tag
git push origin --tags      # Push all tags

# Delete tag
git tag -d v1.0.0           # Delete local tag
git push origin :refs/tags/v1.0.0  # Delete remote tag
```

## Common Workflows Summary

### Daily Workflow
```bash
git status                   # Check current state
git pull origin main         # Get latest changes
git checkout -b feature-xyz  # Create feature branch
# ... make changes ...
git add .                   # Stage changes
git commit -m "Description" # Commit changes
git push -u origin feature-xyz  # Push to remote
# ... create Pull Request on GitHub ...
```

### Before Starting Work
```bash
git checkout main           # Switch to main branch
git pull origin main        # Get latest changes
git checkout -b new-feature # Create new branch
```

### After Feature is Merged
```bash
git checkout main           # Switch to main
git pull origin main        # Get latest changes
git branch -d feature-name  # Delete local feature branch
git remote prune origin     # Clean up remote tracking branches
```

## Pro Tips

1. **Use meaningful commit messages**: Start with a verb, keep under 50 characters for the first line
2. **Commit often**: Small, logical commits are easier to understand and revert
3. **Use branches**: Never work directly on main/master
4. **Pull before push**: Always get latest changes before pushing
5. **Use .gitignore**: Exclude files that shouldn't be tracked (node_modules, .env, etc.)
6. **Review before committing**: Use `git diff --staged` to review changes
7. **Use SSH keys**: More secure and convenient than HTTPS authentication

## Common Git Aliases
Add these to your `~/.gitconfig` file:
```
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    visual = !gitk
    lg = log --oneline --graph --decorate --all
```

This tutorial covers the essential Git and GitHub commands you'll use in your daily development workflow. Practice these commands in a test repository to become comfortable with the Git workflow!
