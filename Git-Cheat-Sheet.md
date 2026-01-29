# Git Cheat Sheet & Workflow Guide

## SSH Key Information

**Private key location:** `~/.ssh/id_rsa`
- This file is stored locally on your system and should never be shared
- Keep it secure and backed up if needed
- The public key has been added to your GitHub account

## Essential Git Commands

### Checking Status
```bash
git status              # See what files have changed
git log                 # View commit history
git log --oneline       # View condensed commit history
```

### Staging & Committing
```bash
git add <file>          # Stage a specific file
git add .               # Stage all changes
git commit -m "message"  # Commit staged changes with a message
git commit -am "message" # Stage and commit all changes (for tracked files)
```

### Branching
```bash
git branch              # List all branches
git branch <name>       # Create a new branch
git checkout <name>     # Switch to a branch
git checkout -b <name>   # Create and switch to a new branch
git branch -d <name>     # Delete a branch (must be merged first)
git branch -D <name>     # Force delete a branch
```

### Remote Operations
```bash
git push                # Push commits to remote
git pull                # Pull changes from remote
git fetch               # Fetch changes without merging
```

### Viewing Changes
```bash
git diff                # Show unstaged changes
git diff --staged       # Show staged changes
git diff HEAD~1         # Compare with last commit
```

### Undoing Changes
```bash
git checkout -- <file>  # Discard changes to a file
git reset HEAD <file>   # Unstage a file
git reset --soft HEAD~1 # Undo last commit, keep changes staged
git reset --hard HEAD~1 # Undo last commit, discard changes
```

## Recommended Workflow

### Feature Branch Workflow
1. **Create a new branch** for each feature or bugfix:
   ```bash
   git checkout -b feature/my-new-feature
   ```

2. **Make changes** and commit them regularly:
   ```bash
   git add .
   git commit -m "feat: implement new feature"
   ```

3. **Push your branch** to GitHub:
   ```bash
   git push -u origin feature/my-new-feature
   ```

4. **Create a Pull Request** on GitHub for code review

5. **After review and merge**, delete the branch:
   ```bash
   git checkout main
   git pull
   git branch -d feature/my-new-feature
   ```

### Commit Message Convention
Use conventional commit format for clear, meaningful messages:

- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, etc.)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

Examples:
```bash
git commit -m "feat: add user authentication"
git commit -m "fix: resolve memory leak in data processing"
git commit -m "docs: update README with installation instructions"
```

## Daily Git Workflow

### Starting Work
```bash
# Make sure you're on main and up to date
git checkout main
git pull

# Create a feature branch
git checkout -b feature/my-work
```

### During Work
```bash
# Check what you've changed
git status

# Stage and commit your work
git add .
git commit -m "feat: implement functionality"

# Save work often!
```

### Finishing Work
```bash
# Push your branch
git push -u origin feature/my-work

# Create PR on GitHub
# After review and merge, switch back to main
git checkout main
git pull
```

## Troubleshooting

### Fixing a bad commit message
```bash
git commit --amend -m "new message"
```

### Undo the last commit but keep changes
```bash
git reset --soft HEAD~1
```

### See what changed between commits
```bash
git diff <commit1> <commit2>
```

## SSH Agent (for convenience)

If you get tired of entering your SSH key password, you can add it to the SSH agent:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

The SSH agent will remember your key for the duration of your session.