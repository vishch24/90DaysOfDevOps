# Understanding Branches

### What is a branch in Git?

- It is a lightweight, movable pointer to a specific commit in your project history.
- Conceptually, branches act as isolated parallel workspaces where developers can safely build features, fix bugs, or experiment without messing up the main production code.

### Why do we use branches instead of committing everything to main?

- We use branches to keep the main code safe and clean. Working on a separate branch lets you test new ideas, fix bugs, or build features in a private space without breaking the working project. Opinions on personal workflows differ, as some solo developers working on simple projects prefer direct commits for speed, but branching remains the standard for safety.

### What is HEAD in Git?

- It is a pointer that references your current position in the repository's history.

How it works?

- Whenever you run commands like `git checkout`, `git switch`, or `git commit`, Git uses `HEAD` to know which files to load or where to attach your next change.

### What happens to your files when you switch branches?

- Your project files update to match the saved state of the target branch.
- Files unique to the new branch appear, files exclusive to your old branch disappear, and shared files update to their recorded versions.
