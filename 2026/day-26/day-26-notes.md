# GitHub CLI: Manage GitHub from Your Terminal

## What authentication methods does `gh` support?

- It supports two main ways to log in when you run the `gh auth login` command:
  - **Web Browser login** (the easiest and recommended way).
  - **Personal Access Token** (PAT).
- It also lets you pick whether you want to connect via `HTTPS` or `SSH` for your git commands.

---

## How could you use `gh issue` in a script or automation?

It can be used in a script to automate tasks like creating bug reports from tests, closing fixed issues, or finding tasks to work on. It lets your terminal or a server do the clicking and typing for you by sending commands straight to GitHub.

**Common Uses in Scripts:**

- **Make new tasks**: Send a quick command when a script fails to log an error as a new issue.
- **Find and read lists**: Pull a list of open tasks to see what work is left to do.
- **Close old tasks**: Mark an issue as done when a fix is ready and merged.

---

## What merge methods does `gh pr merge` support?

1. **Merge Commit (`--merge`)**
   - **What it does**: Joins all branches together and creates a special "merge block" commit on the main line.
   - **Simple use**: Keeps every single small commit you made on your feature branch. It shows an exact map of when the branch joined.

2. **Squash Merge (`--squash`)**
   - **What it does**: Packs all the separate commits from your feature branch into one big single commit.
   - **Simple use**: Keeps the main history clean. Instead of seeing ten minor fixes like "typo" or "oops", the main branch only sees one clean update.
  
3. **Rebase Merge (`--rebase`)**
   - **What it does**: Moves your branch commits directly onto the tip of the main branch without creating a extra merge commit.
   - **Simple use**: Makes the history look like a straight, single line of changes as if you wrote everything directly on the main branch.

---

## How would you review someone else's PR using `gh`?

**Steps to Review a Pull Request**

- Run `gh pr list` to see all open pull requests.
- Run `gh pr checkout <pr-number>` to download and switch to that branch locally.
- Run the project on your computer.
- Test the new features or fixes.
- Type `git diff` to see what lines of code changed.
- Submit your feedback using `gh pr review <pr-number> --approve`, `--comment`, or `--request-changes`.

---

## How could `gh run` and `gh workflow` be useful in a CI/CD pipeline?

In a CI/CD pipeline, `gh run` and `gh workflow` are commands that allow you to control, monitor, and trigger your automation workflows directly from your terminal without needing to open the GitHub website.

**What `gh workflow` Does (Managing the Rules)**

`gh workflow` acts as a tool to manage your pipeline blueprints. It handles the configuration and tells GitHub what automation scripts exist.

- **Trigger pipelines manually**: You can start a specific pipeline immediately using `gh workflow run`. This is helpful if you want to deploy to production right now without waiting for a code push.
- **List available pipelines**: Running `gh workflow list` shows you all the CI/CD automation files active in your project.
- **Turn pipelines on or off**: You can use `gh workflow disable` or `gh workflow enable` to stop or start specific automations, like pausing heavy testing scripts during temporary server maintenance.

**What `gh run` Does (Monitoring the Execution)**

`gh run` acts as a live dashboard for pipelines that are currently running or have already finished. It helps you see the active results of your automation.

- **Track live progress**: Typing `gh run watch` lets you see your tests running in real-time right inside your terminal window.
- **Check for failures**: If a build fails, `gh run list` instantly shows you which specific attempt broke.
- **Read error logs**: You can use `gh run view` to see the exact text output of a failed test or deploy step, so you can fix your code without leaving your text editor.
- **Fix and retry**: If a network glitch breaks your deployment, `gh run rerun` lets you restart the failed pipeline instantly.
