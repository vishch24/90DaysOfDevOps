# Git Commands Cheat Sheet

## Setup & Config

1. Install git from the official URL https://git-scm.com/install.

2. Check git version after installation.

```bash
git -v
# OR
git --version
# OR
git version
```
*What it does*: It is used to check the version of the git installed on your computer.

*Example*: `git version`

3. Config your git user in your local machine.

```bash
git config --global user.name <user-name>
git config --global user.email <user-email>
```
*What it does*: It is used to set the user's details globally.

*Example*: `git config --global user.name "Vishakha"` and `git config --global user.email "vishakha@example.com"`

4. List down the user configured.

```bash
git config --list
# OR
git config -l
```
*What it does*: It displays all the Git configuration properties and their values that apply to your current environment.

*Example*: `git config --list`

## Basic Workflow

1. Initialise your git repository.

```bash
git init
```
*What it does*: used to create a new, empty Git repository or reinitialize an existing one. used to create a new, empty Git repository or reinitialize an existing one. Running this command creates a hidden `.git` folder inside your project directory, which allows Git to start tracking your files, history, and branches.

*Example*: `git init`

2. Add files/folders to staging.

```bash
git add .
# OR
git add <file>
```
*What it does*: It saves changes from your working directory into the Git staging area. It is used before committing the changes to git, i.e., `git commit`.

*Example*: `git add intro.txt`

3. Commit the file.

```bash
git commit -m "<meaning-message-to-understand-the-changes>"
```
*What it does*: It captures a snapshot of your project's currently staged changes and permanently records it to your local repository history.

*Example*: `git commit -m "First file of introduction added."`

4. Check git status before committing.

```bash
git status
```
*What it does*: It shows the state of your working directory and staging area.

*Example*: `git status`

5. Display commit history in a detailed format.

```bash
git log
```
*What it does*: It is a utility tool used to view the detailed history of commits in a Git repository in reverse chronological order.

*Example*: `git log`

6. Display commit history in oneline also known as compact format.

```bash
git log --oneline
```
*What it does*: It is a shorthand option that condenses your Git commit history into a compact, single-line format per commit.

*Example*: `git log --oneline`

7. Visualize entire commit history in a compact and text-based tree graph.

```bash
git log --oneline --graph --all
```
*What it does*: It is used to visualize the entire commit history of a repository as a compact, text-based tree graph directly inside your terminal.

*Example*: `git log --oneline --graph --all`

8. Compare code changes (unstaged)

```bash
git diff
```
*What it does*: It is used to compare and display code changes between different states of your repository.

*Example*: `git diff`

9. Compare code changes (staged)

```bash
git diff --staged
```
*What it does*: It shows the exact code changes you have added to your staging area (`git add`) compared against your last commit (`HEAD`).

*Example*: `git diff --staged`

10. Display complete records of Git from local machine.

```bash
git reflog
```
*What it does*: It is your local safety net and "undo button" in Git. It records a private, chronological diary of every single time your `HEAD` pointer moves. It tracks commits, checkouts, merges, rebases, and hard resets.

*Example*: `git reflog`

---

## Branching

1. Create a new branch.

```bash
git branch <branch-name>
```
*What it does*: It creates a new branch without switching from the current to the new one.

*Example*: `git branch feature-1`

2. Switch to the new branch created.

```bash
git checkout <branch-name>
# OR
git switch <branch-name>
```
*What it does*: It switches to the branch we already created. `switch` is a modern style of git to switch branches, whereas, `checkout` is a classic way.

*Example*: `git checkout feature-1`

3. Check the current branch.

```bash
git branch
```
*What it does*: It displays the repository's current branch.

*Example*: `git branch`

4. Create a new branch and switch into it.

```bash
git checkout -b <branch-name>
```
*What it does*: It creates a new branch with `-b` and auto switches into it.

*Example*: `git checkout -b feature-2`

5. Delete a branch.

```bash
git branch -d <branch-name>
```
*What it does*: To delete a branch, switch out of the branch you want to delete, then delete it.

*Example*: `git branch -d feature-2`

---

## Remote

1. Add to remote.

```bash
git remote add origin git@github.com:<username>/<github-repository-name>.git
```
*What it does*: It links your local Git repository to a remote server.

*Example*: `git remote add origin git@github.com:vishch24/git-practice.git`

2. Set URL of an existing remote Git repository.

```bash
git remote set-url origin git@github.com:<username>/<github-repository-name>.git
```
*What it does*: It updates the link between your local repository copy and the server hosting your project, e.g., GitHub.

*Example*: `git remote set-url origin git@github.com:vishch24/git-practice.git`

3. Verify the remote git repository URL.

```bash
git remote -v
```
*What it does*: It checks the remote git repository URL.

*Example*: `git remote -v`

4. Push your code.

```bash
git push -u origin <branch-name>
```
*What it does*: It uploads your local repository commits to a remote repository. `-u` (or `--set-upstream`) links your current local branch to the remote branch.

*Example*: `git push -u origin master`

5. Pull code.

```bash
git pull origin <branch-name>
```
*What it does*: It downloads changes from a remote repository and immediately integrates them into your current local branch.

*Example*: `git pull origin master`

6. Fetch code. 

```bash
git fetch origin <branch-name>
```
*What it does*: It downloads commits, files, and references from a remote repository into your local repository without merging them into your working code.

*Example*: `git fetch origin master`

7. Clone a public repository from GitHub.

```bash
git clone https://github.com/<github-username>/<repository-name>.git
```
*What it does*: It clones a public repository to your local machine.

*Example*: `git clone https://github.com/rtyley/small-test-repo.git`

8. Setting up remote upstream

```bash
git remote add upstream https://github.com/<github-username>/<repository-name>.git
```
*What it does*: It links your local repository to the original source repository you forked on platforms like GitHub or GitLab. This allows you to pull down changes made by other contributors into your local codebase to keep your fork up-to-date.

*Example*: `git remote add upstream https://github.com/rtyley/small-test-repo.git`

---

## Merging & Rebasing

1. Merge two branches.

```bash
git merge <branch-name>
```
*What it does*: It combines changes from one Git branch into your currently active branch.

*Example*: `git merge feature-login`

2. Rebase a set of commits into a linear-line.

```bash
git rebase <branch-name>
```
*What it does*: It rewrites commit history by taking all the commits from your current branch and reapplying them on top of another base branch. It alters the commit history to create a clean, completely linear line of commits.

*Example*: `git rebase master`

3. Merge a set of commits into one.

```bash
git merge --squash <branch-name>
```
*What it does*: It combines all commits from a feature branch into a single set of changes staged on your current branch, without automatically creating a merge commit.

*Example*: `git merge --squash feature-profile`

---

## Stash & Cherry Pick

1. Save uncommitted changes.

```bash
git stash
```
*What it does*: It temporarily shelves (stores) uncommitted changes in your working directory so you can switch branches or work on something else without losing your current progress.

*Example*: `git stash`

2. Reapply stashed changes.

```bash
git stash apply
```
*What it does*: It reapplies previously stashed changes to your current working directory while keeping those changes saved in your stash history.

*Example*: `git stash apply`

3. Save uncommitted changes to stash.

```bash
git stash push -m "<commit-message>"
```
*What it does*: It saves your uncommitted local changes to the Git stash hierarchy and labels them with a custom descriptive message so you can easily identify them later.

*Example*: `git stash push -m "Added stash"`

4. Display stash changes.

```bash
git stash list
```
*What it does*: It used to display all the stashed changes currently stored in your repository's stash stack.

*Example*: `git stash list`

5. Display code changes from the stash.

```bash
git stash show -p
```
*What it does*: It displays the full file diff (the actual code changes) inside your stashed changes, rather than just a summary of the modified files.

*Example*: `git stash show -p`

6. Display stash.

```bash
git stash show
```
*What it does*: It lets you inspect the contents of a stash entry without applying or popping it from your stash stack.

*Example*: `git stash show`

7. Restore stashed changes.

```bash
git stash pop
```
*What it does*: It restores stashed changes to your current working directory and permanently removes them from your stash history.

*Example*: `git stash pop`

8. Pick a commit from another branch.

```bash
git cherry-pick <commit-id>
```
*What it does*: It copies the changes from an existing commit on another branch and applies them as a new commit onto your current active branch.

*Example*: `git cherry-pick 39fe3d7`

9. Skip the conflicting commit while cherry picking.

```bash
git cherry-pick --skip
```
*What it does*: It used to skip the current conflicting commit during an active, multi-commit cherry-pick sequence.

*Example*: `git cherry-pick --skip`

10. Continue cherry picking after solving merge conflicts.

```bash
git cherry-pick --continue
```
*What it does*: It resumes an in-progress cherry-pick operation after it has been paused due to merge conflicts.

*Example*: `git cherry-pick --continue`

---

## Reset & Revert

1. Undo last commit and keep all changes staged.

```bash
git reset --soft HEAD~1
```
*What it does*: It undoes your very last commit while keeping all your modified files intact and staged.

*Example*: `git reset --soft HEAD~1`

2. Undo last commit and unstage its changes.

```bash
git reset --mixed HEAD~1
```
*What it does*: It undoes your very last commit and unstages all of its changes, but leaves your actual code completely safe in your working directory.

*Example*: `git reset --mixed HEAD~1`

3. Delete last commit, history and files.

```bash
git reset --hard HEAD~1
```
*What it does*: It permanently deletes your last commit, throws away all staged changes, and wipes out all uncommitted local work.

*Example*: `git reset --hard HEAD~1`

4. Restore a commit.

```bash
git revert <commit-id>
```
*What it does*: It safely undoes the changes of a specific past commit by creating an entirely new commit with the exact opposite (inverted) changes.

*Example*: `git revert a451s2f`
