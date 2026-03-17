# Git — Common Commands & General Workflow

---

## Initialization

```bash
git init                        # Initialize a Git repository in the current folder
git clone <url>                 # Clone a remote repository locally
```

---

## Staging & Commits

```bash
git add <file>                  # Stage a specific file
git add .                       # Stage all modified files
git commit -m "message"         # Create a commit with the staged files
```

> ⚠️ A commit ≠ a branch. A commit is a snapshot of the project state at a given point in time.

---

## Branches

```bash
git branch                      # List branches (shows which one you're on)
git branch <name>               # Create a new branch
git checkout <name>             # Switch to an existing branch
git checkout -b <name>          # Create a branch AND switch to it
git branch -d <name>            # Delete a local branch (commits are preserved)
```

---

## History

```bash
git log                         # Show commit history
git log --oneline --graph       # Condensed, visual graph of branches
```

---

## Fetch, Pull & Push

```bash
git fetch                       # Download remote changes WITHOUT merging them locally
git pull                        # fetch + merge: downloads AND integrates remote changes
git push                        # Push local commits to the remote repository
git push -u origin <branch>     # First push of a branch (links local branch to remote)
```

> 💡 **Fetch vs Pull**
> - `git fetch`: downloads remote updates into `.git/` but doesn't touch your working code. Useful to check what changed before integrating.
> - `git pull`: equivalent to `git fetch` + `git merge`. Directly integrates changes into your current branch.
>
> When working in a team, it's normal to have remote commits that aren't yet local. Running `git fetch` regularly keeps you informed without any risk.

---

## Going Back: `git revert`

Consider this commit history:

```
A - B - C - D        (main branch)
        |
        E - F - G - H  (other branch)
```

To undo commits **without rewriting history** (recommended in a team):

```bash
# Revert a range of commits (e.g. from F to H)
git revert --no-commit F^..H
git commit -m "Revert commits F to H"

# Revert only the last commit
git revert HEAD

# Revert the last N commits (e.g. last 3)
git revert HEAD~3..HEAD
git commit -m "Revert last 3 commits"
```

> ✅ `git revert` creates a **new commit** that undoes the changes. History stays intact — this is the clean, team-friendly approach.
>
> ❌ Avoid `git push --force` in a team: it rewrites the remote history and can overwrite your colleagues' work.

---

## Merge & Cleanup

```bash
git checkout main               # Switch to the target branch
git merge <branch-name>         # Merge the branch into main
git branch -d <branch-name>     # Delete the local branch (commits are preserved in main)
```

---

## Typical Team Workflow Summary

```bash
git fetch                       # Check for remote updates
git pull                        # Integrate remote changes
git checkout -b my-feature      # Create and switch to a new branch
# ... work, make changes ...
git add .
git commit -m "feat: add X"
git push -u origin my-feature   # Push the branch
# Open a Pull Request / Merge Request on GitHub/GitLab
git checkout main
git merge my-feature
git branch -d my-feature
```