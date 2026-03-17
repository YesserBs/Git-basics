# Git Setup & GitHub — First-Time Guide

---

## 1. Install Git

### Windows
Download and run the installer from [https://git-scm.com](https://git-scm.com).  
During setup, the defaults are fine. Make sure **"Git Bash"** is included.

### macOS
```bash
xcode-select --install
```
This installs the Apple Developer tools which include Git. If you prefer the latest version:
```bash
brew install git   # requires Homebrew: https://brew.sh
```

### Linux (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install git
```

### Verify the installation
```bash
git --version
# git version 2.x.x
```

---

## 2. Configure Git (do this once)

Before doing anything, tell Git who you are. This info gets attached to every commit you make.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Optional but recommended — set VS Code (or another editor) as the default editor:
```bash
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "nano"          # nano (simpler)
```

Check your config at any time:
```bash
git config --list
```

---

## 3. Link to GitHub

GitHub no longer accepts your password over the command line. You have two options to authenticate: **HTTPS with a token** or **SSH**. SSH is the cleaner long-term setup.

---

### Option A — HTTPS with a Personal Access Token (simpler to start)

1. Go to GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token**, give it a name, set an expiration, and check the **repo** scope
3. Copy the token (you won't see it again)

Now when you push for the first time, Git will ask for your credentials:
- **Username**: your GitHub username
- **Password**: paste your token (not your GitHub password)

To avoid typing it every time, tell Git to cache it:
```bash
git config --global credential.helper store       # saves permanently (plaintext)
# or
git config --global credential.helper cache       # saves for 15 minutes in memory
```

---

### Option B — SSH (recommended for daily use)

#### Step 1 — Generate an SSH key pair
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```
- Press Enter to accept the default file location (`~/.ssh/id_ed25519`)
- Optionally set a passphrase (adds security, but you'll type it on first use)

This creates two files:
- `~/.ssh/id_ed25519` → your **private key** (never share this)
- `~/.ssh/id_ed25519.pub` → your **public key** (this goes to GitHub)

#### Step 2 — Add the key to your SSH agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### Step 3 — Add the public key to GitHub
Copy your public key:
```bash
cat ~/.ssh/id_ed25519.pub
```
Then on GitHub:  
**Settings** → **SSH and GPG keys** → **New SSH key** → paste the key → save.

#### Step 4 — Test the connection
```bash
ssh -T git@github.com
# Hi <your-username>! You've successfully authenticated...
```

If you see that message, you're connected. ✅

#### Step 5 — Use SSH URLs when cloning
```bash
# HTTPS (token-based)
git clone https://github.com/user/repo.git

# SSH (key-based) ← use this if you set up SSH
git clone git@github.com:user/repo.git
```

If you already cloned a repo with HTTPS and want to switch to SSH:
```bash
git remote set-url origin git@github.com:user/repo.git
```

---

## 4. Create your first repository on GitHub

1. On GitHub, click **New repository**, give it a name, leave it empty (no README)
2. Locally:

```bash
mkdir my-project && cd my-project
git init
git add .
git commit -m "first commit"
git branch -M main                              # rename branch to main
git remote add origin git@github.com:user/my-project.git
git push -u origin main
```

After this first push with `-u`, future pushes are just:
```bash
git push
```

---

## Quick recap

| Step | Command |
|---|---|
| Install | `sudo apt install git` / installer / `brew install git` |
| Configure identity | `git config --global user.name/email` |
| Generate SSH key | `ssh-keygen -t ed25519 -C "you@example.com"` |
| Test GitHub connection | `ssh -T git@github.com` |
| Link remote to local repo | `git remote add origin <url>` |
| First push | `git push -u origin main` |