# Git Commands Cheat Sheet

### Setup & Config

```bash
git -v
# OR
git --version
# OR
git version
```
*What it does*: It is used to check the version of the git installed on your computer.

*Example*: `git version`

```bash
git config --global user.name <user-name>
git config --global user.email <user-email>
```
*What it does*: It is used to set the user's details globally.

*Example*: `git config --global user.name "Vishakha"` and `git config --global user.email "vishakha@example.com"`

```bash
git config --list
# OR
git config -l
```
*What it does*: It displays all the Git configuration properties and their values that apply to your current environment.

*Example*: `git config --list`

---

### Basic Workflow

```bash
git init
```
*What it does*: used to create a new, empty Git repository or reinitialize an existing one. used to create a new, empty Git repository or reinitialize an existing one. Running this command creates a hidden `.git` folder inside your project directory, which allows Git to start tracking your files, history, and branches.

*Example*: `git init`

```bash
git add .
# OR
git add <file>
```
*What it does*: It saves changes from your working directory into the Git staging area. It is used before committing the changes to git, i.e., `git commit`.

*Example*: `git add intro.txt`

```bash
git status
```
*What it does*: It shows the state of your working directory and staging area.

*Example*: `git status`

```bash
git commit -m "<meaning-message-to-understand-the-changes>"
```
*What it does*: It captures a snapshot of your project's currently staged changes and permanently records it to your local repository history.

*Example*: `git commit -m "First file of introduction added."`

---

### Viewing Changes

```bash
git log
```
*What it does*: It is a utility tool used to view the detailed history of commits in a Git repository in reverse chronological order.

*Example*: `git log`

```bash
git log --oneline
```
*What it does*: It is a shorthand option that condenses your Git commit history into a compact, single-line format per commit.

*Example*: `git log --oneline`
