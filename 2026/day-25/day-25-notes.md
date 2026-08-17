# Understanding Git Reset

## What is the difference between `--soft`, `--mixed`, and `--hard`?

| Features | `--soft` | `--mixed` | `--hard` |
| :--- | :--- | :--- | :--- |
| **Moves History?** | ✅ | ✅ | ✅ |
| **Keeps Staged Changes? (Index)** | ✅ | ❌ | ❌ |
| **Keeps Local Edits? (Working Directory)** | ✅ | ✅ | ❌(Deletes them) |

## Which one is destructive and why?

- `--hard` is destructive. It updates the index (staging area) and working directory, and completely discards all uncommitted changes and staged files. Any work you have not committed will be lost forever.

## When would you use each one?

- `--soft` when you want to undo a commit but keep your changes ready to commit again. (Staged)
- `--mixed` (the default) when you want to undo a commit and keep your changes in your working folder to edit them. (Unstagged)
- `--hard` when you want to erase a commit and delete all changes completely.

## Should you ever use `git reset` on commits that are already pushed?

- **No**. It deletes commits and rewrites the history tree. When collaborators try to pull updates from a branch you have altered with `git reset`, their local histories will conflict with the remote server, causing major synchronization problems for the team.

---

# Understanding Git Revert

## How is `git revert` different from `git reset`?

- `git revert` creates a new commit that safely rolls back changes.
- `git reset` deletes or rewires existing commits to erase history.

| Features | `git revert` | `git reset` |
| :--- | :--- | :--- |
| **History Effect** | Adds a new "anti-commit". | Erases or moves commits back. |
| **Best Used For** | Public/Shared remote branches. | Private/Local unpushed work. |
| **Risk Level** | Safe. Never deletes data. | High. Can permanently erase work. |

## Why is `revert` considered safer than reset for shared branches?

- It does not rewrite history. Instead of erasing past commits, it calculates the exact inverse of a targeted change and applies it as a brand-new commit.

## When would you use `revert` vs `reset`?

- Use `revert` when you need to safely undo changes on a shared public branch by creating a new inverse commit.
- Use `reset` when working privately on local commits that have not been shared yet, as it rewinds the branch pointer and changes history.

---

# Understanding Branching Strategies

## How it works?

- A branching strategy is a set of rules for how developers create, share, and combine code changes in a version control system like Git.
- It keeps the main code safe, prevents team members from breaking each other's work, and organizes how updates move to production.

## GitFlow — develop, feature, release, hotfix branches

```text
main:    o---------------o---o  (Production releases)
          \             /   ^
release:   \           o---/    (Staging / QA preparation)
            \         /
develop:     o---o---o---o---o  (Integration branch)
                  \     /
feature:           o---o        (New feature)
```

- **main**: Holds official production releases.
- **develop**: Ongoing integration branch for features.
- **Release/Hotfix branches**: Temporary branches used to polish upcoming releases or patch live production bugs.

## GitHub Flow — simple, single main branch + feature branches

```text
main:    o---o---o---o---o---o  (Production-ready)
              \         /
feature:       o---o---o  (Create branch, PR, then merge)
```

- **main**: Always clean and ready to deploy to production.
- **Feature branch**: Branched off main for any new feature or bug fix.
- **Merge**: Merged back into main via a Pull Request once reviewed.

## Trunk-Based Development — everyone commits to main, short-lived branches

```text
trunk:   o---o---o---o---o---o  (Everyone commits here frequently)
              \       \
devs (local):  o       o      (Short-lived local work, pushed daily)
```

- **Trunk (main)**: The single source of truth.
- **Commits**: Developers push small, frequent updates directly to the trunk or use very short-lived branches lasting less than a day.
- **Feature Flags**: Unfinished features are hidden behind flags until ready.

## When/where it's used

1. Gitflow
   - **Where to use**: Larger projects with scheduled release windows or software versions that must be maintained separately.
   - **When to use**: Complex apps using a dedicated `develop` branch for ongoing work, separate `release/*` branches for testing, and `hotfix/*` branches for urgent production fixes.
   - **Best for**: Enterprise apps, desktop software, or packaged products with rigid version numbers.

2. GitHub Flow / Feature Branching
   - **Where to use**: Projects with small-to-medium teams or continuous delivery cycles.
   - **When to use**: You want every new feature or fix built in its own temporary branch, checked via a pull request, and merged directly into the main branch.
   - **Best for**: SaaS products, open-source projects, and teams wanting simple code reviews.

3. Trunk-Based Development
   - **Where to use**: High-speed teams with strong automated tests and continuous deployment (CI/CD).
   - **When to use**: Daily work where developers push small changes straight to the main branch (the trunk) or use tiny branches that last only a few hours.
   - **Best for**: Experienced teams, microservices, and fast web apps.

## Pros and cons

### Gitflow

> Gitflow uses dedicated branches for features, development, releases, and hotfixes. It is built for scheduled, multi-version enterprise releases.

| Pros | Cons |
| :--- | :--- |
| Clean separation between live production code and active development. | High complexity and a steep learning curve for new developers. |
| Excellent for managing multiple concurrent versions or scheduled software releases. | Overkill for simple web apps, microservices, or continuous deployment. |
| Clear scopes for testing and staging. | Prone to complex merge conflicts if long-lived feature branches drift far from the develop branch. |

### GitHub Flow

> GitHub Flow is a lightweight, branch-based workflow where everything branches off main for short-lived features, uses pull requests for reviews, and merges back for immediate deployment.

| Pros | Cons |
| :--- | :--- |
| Simple, intuitive, and easy for small teams or open-source projects to adopt. | Lacks structure for handling multiple production versions or hotfixes on older releases. |
| Speeds up feedback loops through continuous integration and pull request reviews. | The `main` branch can become unstable if automated tests and pull request gates are bypassed. |
| Ideal for applications with a single live production version. | Can break down in large enterprises managing parallel, highly complex release schedules. |

### Trunk-Based Development

> Trunk-Based Development has developers merge small, frequent updates directly into a single central branch (the trunk) multiple times a day, often relying on feature flags.

| Pros | Cons |
| :--- | :--- |
| Eliminates merge hell by keeping integration continuous and transparent. | Requires robust, mature automated testing and CI/CD pipelines to prevent broken builds. |
| Enables true continuous delivery and the fastest possible release cycles. | High risk if team discipline or test coverage slips. |
| Encourages high team communication and fast code visibility. | Harder to implement for large, distributed teams without strong engineering standards. |
