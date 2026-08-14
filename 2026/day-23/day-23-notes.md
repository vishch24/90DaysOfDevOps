# Understanding Branches

### What is a branch in Git?

- It is a lightweight, movable pointer to a specific commit in your project history.
- Conceptually, branches act as isolated parallel workspaces where developers can safely build features, fix bugs, or experiment without messing up the main production code.

---

### Why do we use branches instead of committing everything to main?

- We use branches to keep the main code safe and clean. Working on a separate branch lets you test new ideas, fix bugs, or build features in a private space without breaking the working project. Opinions on personal workflows differ, as some solo developers working on simple projects prefer direct commits for speed, but branching remains the standard for safety.

---

### What is HEAD in Git?

- It is a pointer that references your current position in the repository's history.

How it works?

- Whenever you run commands like `git checkout`, `git switch`, or `git commit`, Git uses `HEAD` to know which files to load or where to attach your next change.

---

### What happens to your files when you switch branches?

- Your project files update to match the saved state of the target branch.
- Files unique to the new branch appear, files exclusive to your old branch disappear, and shared files update to their recorded versions.

---

### What is the difference between `origin` and `upstream`?

- `origin` is the default name for your own remote repository (usually your personal fork or the main repo you cloned).
- `upstream` is the name usually given to the main, original project repository when you fork someone else's project, letting you pull fresh updates from them.

---

###  What is the difference between `git switch` and `git checkout`?

| Feature | `git switch` | `git checkout` |
| :--- | :--- | :--- |
| **Overview** | It is a modern, single-purpose command exclusively for changing and creating branches | It is an older, multi-purpose command that manages branches, restores files, and detaches HEADs |
| **Primary Purpose** | Switch branches only | Multi-purpose (Branches, files, commits) |
| **Switch to Branch** | `git switch <branch>` | `git checkout <branch>` |
| **Create & Switch** | `git switch -c <branch>` | `git checkout -b <branch>` |
| **Discard File Changes** | Cannot do this (Use `git restore`) | `git checkout -- <file>` |
| **Safety / Error Risk** | High (Prevents file overwriting) | Low (Typing a branch name wrong can overwrite files) |

---

###  What is the difference between `git fetch` and `git pull`?

- `git fetch` only downloads remote changes without modifying your local working files.
- `git pull` downloads those changes and immediately merges them into your active branch.

| Feature | `git fetch` | `git pull` |
| :--- | :--- | :--- |
| **Primary Action** | Downloads data; does not integrate it. | Downloads data and integrates it. |
| **Working Directory** | Untouched and safe. | Overwritten or updated with new commits. |
| **Risk of Merge Conflicts** | No risk (completely safe). | High risk if local and remote files overlap. |
| **When to Use** | Reviewing changes before applying them. | Quickly syncing when you trust the remote. |

---

###  What is the difference between `clone` and `fork`?

- Forking creates a server-side copy of a repository under your own account, giving you an independent remote project to experiment with or modify.
- Cloning downloads a copy of a repository to your local computer so you can write, edit, and test code on your machine.

| Feature | `clone` | `fork` |
| :--- | :--- | :--- |
| **Location** | Forking happens on a remote server like GitHub or GitLab. | Cloning happens locally on your computer. |
| **Ownership** | A fork belongs to you as a brand-new remote repository. | A clone mirrors the access rights and ownership of the source repo. |
| **Connection** | Forked repos link back to the original project for submitting pull requests. | Cloned repos sync directly with the specific remote URL they were copied from. |

---

###  When would you `clone` vs `fork`?

- Fork: Use when you want to change someone else's public project or start an independent version of software when you lack write access.
- Clone: Use when you have direct permission to work on a repository or after you have forked a project and need the files on your computer.

---

###  After forking, how do you keep your fork in sync with the original repo?

- To keep your fork in sync with the original repository, add the original repo as an upstream remote, fetch its latest changes, and merge them into your local main branch before pushing the updates to your GitHub fork.
