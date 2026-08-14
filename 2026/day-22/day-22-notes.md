# Understand the Git Workflow

### 1. What is the difference between `git add` and `git commit`?

| Feature | `git add` | `git commit` |
| :--- | :--- | :--- |
| **Primary Action** | Stages file changes. | Saves the staged snapshot permanently. |
| **Target Area** | Staging Area (Index). | Local Repository. |
| **History Log** | Does not affect project history. | Generates a tracking hash and tracking message. |
| **Usage Frequency** | Run multiple times as you modify files. | Run when a specific task/feature is complete. |

---

### 2. What does the **staging area** do? Why doesn't Git just commit directly?

- The Git staging area (also called the "index") is a middle ground where you prepare a commit before saving it to your history. Git does not commit directly because staging gives you precise control over your project's version history.

---

### 3. What information does `git log` show you?

- By default, the `git log` command displays a reverse chronological ordered record (newest first) of all commits for the currently checked-out branch.

---

### 4. What is the `.git/` folder and what happens if you delete it?

- `.git/` folder is the brain and heart of your Git repository. It is a hidden directory that stores all the tracking data, configuration settings, and historical records for your project.
- If you delete the `.git/` folder, *your project immediately stops being a Git repository*. Your project files (the code you see in your editor) will remain safe, but you will instantly lose your entire commit history, all branches, tags, and connections to remote repositories like GitHub.

What's inside `.git/` folder?

1. `branches/`: It is an *obsolete legacy mechanism* used to store shortcuts for remote repositories. It has been deprecated and is completely unused in modern versions of Git, which now use `.git/remotes` or the standard configuration file (`.git/config`) instead.
2. `COMMIT_EDITMSG`: It is a temporary text file that Git creates to hold your in-progress commit message.
3. `config`: : The settings. A text file containing repository-specific configurations, including your remote repository URLs and branch tracking preferences.
4. `description`: It is a metadata file used by external tools to display a short summary of your repository.
5. `HEAD`: : The current position. A file that tells Git which branch or commit you are currently working on (e.g., `ref: refs/heads/main`).
6. `hooks/`: The automations. Scripts that run automatically before or after Git events, such as checking code formatting before a commit (`pre-commit`).
7. `index`: The staging area. A binary file that acts as a preparation zone between your working directory and your next commit.
8. `info/`: The extras. Contains global exclude rules (like a secondary `.gitignore`) that you don't want to share with other team members.
9. `logs/`: The history of changes. Keeps track of every time a branch pointer moves, powering the `git reflog` command to help you recover lost work.
10. `objects/`: The database. It stores all your file contents (blobs), commit data, and directory structures (trees) as compressed, hashed files.
11. `refs/`: The pointers. It holds references to your local branches, remote branches, and tags. Each file contains a hash pointing to a specific commit.

---

### 5. What is the difference between a **working directory**, **staging area**, and **repository**?

- `working directory` is the local folder where you edit and save real project files.
- `staging area` (or index) is a holding space where you select and prepare specific changes for your next save.
- `repository` is the hidden database (`.git` folder) that stores the permanent history of all saved snapshots.
