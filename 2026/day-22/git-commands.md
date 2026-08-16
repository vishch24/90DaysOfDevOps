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

4. Create a new branch.

```bash
git branch <branch-name>
```
*What it does*: It creates a new branch without switching from the current to the new one.

*Example*: `git branch feature-1`

5. Switch to the new branch created.

```bash
git checkout <branch-name>
# OR
git switch <branch-name>
```
*What it does*: It switches to the branch we already created. `switch` is a modern style of git to switch branches, whereas, `checkout` is a classic way.

*Example*: `git checkout feature-1`

6. Check the current branch.

```bash
git branch
```
*What it does*: It displays the repository's current branch.

*Example*: `git branch`

7. Create a new branch and switch into it.

```bash
git checkout -b <branch-name>
```
*What it does*: It creates a new branch with `-b` and auto switches into it.

*Example*: `git checkout -b feature-2`

8. Delete a branch.

```bash
git branch -d <branch-name>
```
*What it does*: To delete a branch, switch out of the branch you want to delete, then delete it.

*Example*: `git branch -d feature-2`

9. Add to remote.

```bash
git remote add origin git@github.com:<username>/<github-repository-name>.git
```
*What it does*: It links your local Git repository to a remote server.

*Example*: `git remote add origin git@github.com:vishch24/git-practice.git`

10. Set URL of an existing remote Git repository.

```bash
git remote set-url origin git@github.com:<username>/<github-repository-name>.git
```
*What it does*: It updates the link between your local repository copy and the server hosting your project, e.g., GitHub.

*Example*: `git remote set-url origin git@github.com:vishch24/git-practice.git`

11. Verify the remote git repository URL.

```bash
git remote -v
```
*What it does*: It checks the remote git repository URL.

*Example*: `git remote -v`

12. Push your code.

```bash
git push -u origin <branch-name>
```
*What it does*: It uploads your local repository commits to a remote repository. `-u` (or `--set-upstream`) links your current local branch to the remote branch.

*Example*: `git push -u origin master`

13. Clone a public repository from GitHub.

```bash
git clone https://github.com/<github-username>/<repository-name>.git
```
*What it does*: It clones a public repository to your local machine.

*Example*: `git clone https://github.com/rtyley/small-test-repo.git`

14. Setting up remote upstream

```bash
git remote add upstream https://github.com/<github-username>/<repository-name>.git
```
*What it does*: It links your local repository to the original source repository you forked on platforms like GitHub or GitLab. This allows you to pull down changes made by other contributors into your local codebase to keep your fork up-to-date.

*Example*: `git remote add upstream https://github.com/rtyley/small-test-repo.git`

---

## Viewing Changes

1. Display commit history in a detailed format.

```bash
git log
```
*What it does*: It is a utility tool used to view the detailed history of commits in a Git repository in reverse chronological order.

*Example*: `git log`

2. Display commit history in oneline also known as compact format.

```bash
git log --oneline
```
*What it does*: It is a shorthand option that condenses your Git commit history into a compact, single-line format per commit.

*Example*: `git log --oneline`

3. Check git status before committing.

```bash
git status
```
*What it does*: It shows the state of your working directory and staging area.

*Example*: `git status`
