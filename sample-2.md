# Important Git Commands: A Detailed Guide

This guide provides a detailed explanation of commonly used Git commands. Understanding these commands is crucial for effective version control and collaboration.

---

## Core Workflow Commands

These are the commands you'll use most frequently in your day-to-day Git workflow.

### `git status`

**Purpose:** Shows the current state of your working directory and staging area.

**What it does:**

- Lists files that have been modified in your working directory but not yet staged (added to the index).
- Lists files that are staged and ready to be committed.
- Lists untracked files (new files that Git doesn't know about yet).
- Indicates which branch you are currently on.
- Tells you if your current branch is ahead of, behind, or diverged from its remote counterpart.

**When to use it:** Frequently! Use it before `git add` to see what you're about to stage, before `git commit` to see what's staged, and after `git pull` or `git merge` to understand the state of your repository.

**Example Output:**

```

On branch main
Your branch is up-to-date with 'origin/main'.

Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
modified: README.md

Untracked files:
(use "git add <file>..." to include in what will be committed)
new_feature.js

no changes added to commit (use "git add" and/or "git commit -a")

```

### `git log`

**Purpose:** Displays the commit history of the current branch.

**What it does:**

- Shows commits in reverse chronological order (most recent first).
- For each commit, it typically displays:
  - The unique SHA-1 hash of the commit.
  - The author's name and email.
  - The date and time the commit was made.
  - The commit message.

**Common Options:**

- `--oneline`: Shows a condensed, one-line summary of each commit.
- `--graph`: Displays an ASCII graph showing branch and merge history.
- `--decorate`: Shows branch and tag names next to commits.
- `--all`: Shows the history of all branches, not just the current one.
- `-p` or `--patch`: Shows the changes (diff) introduced by each commit.
- `--stat`: Shows a summary of files changed and lines added/deleted for each commit.
- `<branch_name>`: Shows the log for a specific branch.
- `<file_path>`: Shows the log for commits that affected a specific file.

**When to use it:** To review past changes, understand the project's evolution, find when a specific change was introduced, or inspect the work of collaborators.

**Example Output (`git log --oneline --decorate --graph`):**

```

- 0a1b2c3 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #12 from user/feature
  |\
  | \* 3d4e5f6 (user/feature) Add amazing new feature
  |/
- 7g8h9i0 Update documentation
- 1j2k3l4 Initial commit

```

### `git add .` (or `git add <file>`)

**Purpose:** Adds changes from your working directory to the staging area (also known as the index).

**What it does:**

- `git add .`: Stages all modified and new untracked files in the current directory and its subdirectories.
- `git add <file_name>`: Stages specific changes in the named file.
- `git add <directory_name>/`: Stages all changes within the specified directory.

The staging area is a "holding zone" where you prepare your next commit. You selectively add the changes you want to include in the upcoming snapshot.

**When to use it:** After making modifications to files and before you run `git commit`. This allows you to craft your commits precisely, grouping related changes together.

### `git commit -m "message"`

**Purpose:** Records the staged changes into the local repository's history.

**What it does:**

- Takes all the changes that have been added to the staging area (via `git add`) and creates a new commit object.
- This commit object is a snapshot of your project at that point in time.
- Each commit has a unique SHA-1 hash, an author, a timestamp, and a commit message.
- The `-m "message"` flag allows you to provide a commit message directly on the command line. If you omit `-m`, Git will open your configured text editor for you to write a more detailed message.

**Commit Message Best Practices:**

- **Subject Line:** Concise (around 50 characters), imperative mood (e.g., "Fix bug" not "Fixed bug").
- **Body (Optional):** If needed, provide more context after a blank line. Explain _why_ the change was made, not just _what_ changed.

**When to use it:** After staging changes with `git add` and when you're ready to save a meaningful snapshot of your work.

### `git push origin main`

**Purpose:** Uploads your local branch's commits to a remote repository.

**What it does:**

- `git push`: The base command.
- `origin`: The name of the remote repository (a common default, especially when you clone a repository). You can have multiple remotes.
- `main`: The name of the local branch whose commits you want to send to the remote. This also specifies the name of the branch on the remote that will be updated.

If the remote branch has new commits that you don't have locally, the push will be rejected (as seen in your error example) to prevent overwriting history. You'll need to `git pull` first in such cases.

**Common Options:**

- `-u` or `--set-upstream`: Sets the remote branch as the "upstream" for your local branch. This means future `git pull` (without arguments) and `git push` (without arguments) will automatically know where to fetch from and push to. Example: `git push -u origin main`.
- `--all`: Pushes all your local branches to the remote.
- `--tags`: Pushes all your local tags to the remote.
- `--force`: **Use with extreme caution!** Overwrites the remote branch with your local branch, even if it means losing commits on the remote.

**When to use it:** When you want to share your committed local changes with collaborators or back them up on a remote server.

### `git remote -v`

**Purpose:** Lists all the remote repositories configured for your local repository, along with their URLs.

**What it does:**

- The `-v` (verbose) flag shows the URLs for both fetching and pushing.
- Each remote has a short "name" (e.g., `origin`) and one or two URLs.

**When to use it:** To see where your local repository is connected for fetching and pushing, or to verify remote URLs.

**Example Output:**

```

origin https://github.com/your-username/your-repo.git (fetch)
origin https://github.com/your-username/your-repo.git (push)
upstream https://github.com/original-owner/their-repo.git (fetch)
upstream https://github.com/original-owner/their-repo.git (push)

```

### `git remote add origin <repo_url>`

**Purpose:** Adds a new remote repository connection to your local Git repository.

**What it does:**

- `git remote add`: The command to add a remote.
- `origin`: The short name you want to use to refer to this remote (e.g., `origin`, `upstream`, `heroku`). `origin` is a common convention for the primary remote.
- `<repo_url>`: The URL of the remote Git repository (e.g., `https://github.com/user/repo.git` or `git@github.com:user/repo.git`).

**When to use it:**

- After creating a new local repository with `git init` and you want to connect it to a newly created empty repository on a hosting service like GitHub.
- When you want to add a connection to another developer's fork or a different upstream repository.

### `git pull` (or `git pull origin main`)

**Purpose:** Fetches changes from a remote repository and immediately tries to merge them into your current local branch.

**What it does:**

- It's essentially a combination of `git fetch` (which downloads remote changes but doesn't integrate them) followed by `git merge` (which integrates the fetched changes).
- `git pull`: If your current branch is tracking a remote branch, Git will pull from that tracked branch.
- `git pull origin main`: Explicitly tells Git to fetch from the `main` branch of the `origin` remote and merge it into your current local branch.

**Common Options:**

- `--rebase`: Instead of merging, it rebases your local commits on top of the fetched remote commits. This can lead to a cleaner, more linear history but rewrites your local commits.
- `--ff-only`: Only perform a fast-forward merge. If the remote history has diverged, the pull will fail instead of creating a merge commit.

**When to use it:** To update your local branch with the latest changes from the remote repository. Be prepared to resolve merge conflicts if both local and remote histories have diverged and made changes to the same parts of files.

---

## Hard Reset (Use with Caution!)

### `git reset --hard origin/main`

**Purpose:** **Destroys local changes and makes your current local branch exactly match a specific remote branch.**

**What it does (in detail for this specific command):**

1.  **Moves the `HEAD` and current branch pointer:** Your current local branch (e.g., `main`) will now point to the _exact same commit_ as the `origin/main` branch (the last known state of the `main` branch on the `origin` remote, as per your last `git fetch`).
2.  **Resets the Staging Area (Index):** The staging area is updated to match the contents of the `origin/main` commit. Any changes you had staged locally are discarded.
3.  **Resets the Working Directory:** Your working directory files are forcefully overwritten to match the contents of the `origin/main` commit. **Any uncommitted local changes (modified files, new untracked files that weren't staged) in your working directory will be permanently lost.**

**When to use it (CAREFULLY):**

- When you want to completely discard all your local commits and changes on the current branch and make it identical to the state of the remote branch.
- If your local branch is a mess and you just want to start fresh from what's on the remote.

**DANGER:**

- **This command is destructive to uncommitted local work.** There is generally no way to recover changes that were in your working directory or staging area but not committed if you use `git reset --hard`.
- If you have local commits that you haven't pushed, this command will remove them from the current branch. You might still be able to recover them using `git reflog` for a while, but it's best to be sure before running this.

**Safer Alternatives (if you want to save local changes):**

- `git stash` to temporarily save local changes.
- Commit your local changes to a temporary branch before resetting.

---

## More Branching Commands

These commands are essential for managing different lines of development within your repository.

### `git branch`

**Purpose:** Lists, creates, or deletes branches.

**What it does:**

- `git branch` (with no arguments): Lists all your local branches. The current branch will be marked with an asterisk (`*`).
- `-a`: Lists all branches (local and remote-tracking).
- `-v`: Shows the last commit on each branch (verbose).
- `-vv`: Shows even more details, including the upstream branch being tracked (if any) and whether the local branch is ahead/behind.

**When to use it:** To see what branches exist, which one you're on, or to get a quick overview of branch states.

### `git branch <branch-name>`

_(e.g., `git branch dev`)_

**Purpose:** Creates a new local branch.

**What it does:**

- Creates a new branch named `<branch-name>`.
- This new branch will point to the **same commit** as your current `HEAD` (the branch you are currently on).
- **It does NOT switch you to the new branch.** You remain on your current branch.

**When to use it:** When you want to start a new line of development (for a feature, bugfix, experiment) but aren't ready to switch to it immediately.

### `git branch -d <branch-name>`

_(e.g., `git branch -d dev`)_

**Purpose:** Deletes a local branch.

**What it does:**

- Deletes the specified local branch.
- **Safety Check:** By default, Git will prevent you from deleting a branch if it contains unmerged commits (commits that are not part of any other branch in your repository). This helps prevent accidental data loss.
- `-D` (capital D): Force-deletes the branch, even if it has unmerged changes. **Use with caution!**

**When to use it:** After a feature branch has been successfully merged into your main branch and is no longer needed.

### `git checkout <branch-name>`

_(e.g., `git checkout dev`)_

**Purpose:** Switches your working directory to a different branch.

**What it does:**

1.  Moves the `HEAD` pointer to point to the specified `<branch-name>`.
2.  Updates your staging area (index) to match the snapshot of the commit that `<branch-name>` points to.
3.  Updates your working directory files to match the contents of that snapshot.

**Safety Check:** Git will warn you and prevent the switch if you have uncommitted changes in your working directory that would be overwritten by the checkout. You'll need to commit, stash, or discard those changes first.

**When to use it:** Whenever you want to switch to a different line of development to work on different features or versions.

### `git checkout -b <new-branch-name>`

_(e.g., `git checkout -b dev`)_

**Purpose:** Creates a new local branch and immediately switches to it.

**What it does:**

- This is a convenient shorthand for running:
  1.  `git branch <new-branch-name>` (creates the branch)
  2.  `git checkout <new-branch-name>` (switches to the new branch)
- The new branch is created pointing to the current `HEAD`.

**When to use it:** This is the most common way to start working on a new feature or bugfix. You create the branch and are immediately ready to start making commits on it.
