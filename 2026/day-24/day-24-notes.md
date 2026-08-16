# Understanding Git Merge

## What is a fast-forward merge?

- A fast-forward merge is a type of Git merge that happens when you combine two branches, and the main branch has not received any new commits since you created your feature branch.
- Instead of creating a new "merge commit" to tie the two paths together, Git simply moves the main branch pointer forward to the latest commit of your feature branch.

How It Works? (Step-by-Step)

1. **The Starting Point**: You create a `feature` branch from `main`.
2. **The Work**: You add new commits to your `feature` branch.
3. **The Pause**: The `main` branch sits completely still—no one else pushes any code to it.
4. **The Merge**: When you merge `feature` into `main`, Git realizes there is a straight, uninterrupted path between them. It just updates the pointer.

```text
Before Merge:
main
  ↓
[Commit A] ———> [Commit B] ———> [Commit C]
                                    ↓
                                 feature

After Fast-Forward Merge:
                               main & feature
                                    ↓
[Commit A] ———> [Commit B] ———> [Commit C]

```

## When does Git create a merge commit instead?

- Git creates a merge commit when it needs to combine two branches that have *diverged* (separated). This means new work was done on both branches since they split from each other.

## What is a merge conflict?

- A merge conflict is an event that happens when a version control system like Git cannot automatically combine two different changes made to the exact same line or part of a file. It stops and asks a human to step in and choose which version to keep.

Why It Happens?

1. Two people work on the same file at the same time.
2. Person A edits line 5 to say "Hello."
3. Person B edits line 5 to say "Hi."
4. Someone tries to join or merge both sets of work together.
5. The system does not know whose work is correct.

How It Looks?

1. The tool pauses the merge process.
2. It marks the messy spot inside the file using special symbols.
3. It shows your version and their version side by side.
   
How to Fix It?

1. Open the broken file.
2. Read both choices.
3. Delete the choice you do not want and the special marks. For e.g., 
4. Save the file and tell the tool the job is done.

Understanding the Marks

1. `<<<<<<< HEAD`: Shows the start of your current local changes.
2. `=======`: Acts as the dividing line between your changes and the other branch.
3. `>>>>>>> <branch-name>`: Shows the end of the conflict and the incoming code from the other branch.

---

# Understanding Git Rebase

## What does rebase actually do to your commits?

- Git rebase takes your saved work (commits) off your branch, cuts the branch from its old spot, and pastes it onto the very top of a new base branch.
- Crucially, it deletes your old commits and makes brand-new copies with new ID numbers so your project history looks like one straight, clean line.
- **NOTE**: Rewrites history: Because it makes new copies of your commits, it changes the past IDs. Never rebase code that you already shared with other people on a public branch, or it will break everyone else's work.

## How is the history different from a merge?

- A history shows every single step of work you did.
- A merge combines two different sets of work into one. A history keeps a clear timeline of all changes. A merge joins separate paths together.

| History | Merge |
| :--- | :--- |
| Shows every commit or save. | Takes two branches of work. |
| Keeps a straight line of changes. | Links them together. |
| Lets you see who made each edit. | Creates a new combined point. |
| Does not hide old work. | Keeps both past paths visible. |

## Why should you never rebase commits that have been pushed and shared with others?

- Never rebase shared commits because `git rebase` secretly deletes old work and replaces it with brand-new copies.
- This rewrites history. If your teammates already saved the old copies, their computers will get confused, and mixing your work together will break.

## When would you use rebase vs merge?

- Use `merge` when you want to safely combine code and keep a true history of when branches met.
- Use `rebase` when you want to clean up your personal work history and make the project timeline look like a straight, neat line before sharing it.

| `rebase` | `merge` |
| :--- | :--- |
| For local cleanup | For shared work |
| For a straight line | To keep a record |
| Avoid shared network; rewrites history | To avoid trouble of rewriting history |

---

# Understanding Squash Commit vs Merge Commit

## What does squash merging do?

- A squash merge combines all the individual code changes from a feature branch into a single, giant commit before adding them to the main branch. Instead of keeping a messy history of every little save, it turns them into one clean update.

How It Works?

- **Normal Merge**: Keeps every single save point and makes a complex web of history.
- **Squash Merge**: Packs all work into one neat package and adds it as a straight line on the main timeline.

Pros and Cons

- **Pros**: Keeps the main project history very clean, readable, and easy to look back on.
- **Cons**: You lose the step-by-step story of how the feature was built day-by-day.

## When would you use squash merge vs regular merge?

- Use a **regular merge** to keep all history and every small step.
- Use a **squash merge** to turn a messy branch into one clean update on the main line.

| Regular Merge | Squash Merge |
| :--- | :--- |
| Keeps every single commit from your feature branch. | Combines all your commits into one big commit. |
| Shows the full story of your work over time. | Makes the main history neat and easy to read. |
| Best for long teams where people need to see every change. | Hides small fix-up commits like "typo fix" or "oops." |
| Can make the main history very busy and hard to read. | Best for solo work or small features that do not need a long history. |

## What is the trade-off of squashing?

- Squashing in git means combining many small code changes into one big change. The main trade-off is that it makes the main project history clean and easy to read, but you lose the step-by-step record of how the work was done.

| Pros | Cons |
| :--- | :--- |
| Keeps the project history neat. | You cannot see how the code grew step by step. |
| Makes it easy to undo a whole feature at once. | It is harder to find the exact moment a bug was added. |
| Removes messy notes like "typo fix" or "testing code" from the main list. | Team members lose the details of past discussions in the small commits. |

---

# Understanding Git Stash

## What is the difference between `git stash pop` and `git stash apply`?

- `git stash pop` applies your saved changes and deletes them from your hidden stash list.
- `git stash apply` copies your saved changes into your workspace but keeps them in your stash list.

| Features | `git stash apply` | `git stash pop` |
| :--- | :--- | :--- |
| **What it does** | Restores your files. | Restores your files. |
| **Stash History** | Keeps the stash in the list. | Deletes the stash from the list. |
| **Safety** | High. Safe to test on multiple branches. | Medium. Cleans up your list automatically. |
| **If there is a conflict** | Files change; stash stays in list. | Files change; stash is not deleted. |

## When would you use stash in a real-world workflow?

- You use `git stash` when you work on code, get an urgent request to fix a different bug, and need to clean your workspace fast. It saves your current unfinished work in a safe hidden spot. This lets you switch branches, fix the urgent issue, and come back later to finish your first task.

Common Real-World Scenarios

- Sudden Bug Fixes: Stash your feature code during urgent debugging.
- Wrong Branch Mistakes: If accidentally code on the `main` branch, stash your code, switch branches and then pop the changes back out.
- Pulling Fresh Updates: While trying to pull new team code, local changes block the updates. In such cases, stash your code, pull the updates smoothly and then re-apply the changes.

---

# Understanding Cherry Picking

## What does cherry-pick do?

- *cherry-picking* means taking one specific change (called a commit) from one working area or branch and copying it directly onto a completely different branch.
- Instead of merging or copying everything, you grab only the exact fix or feature you want.

## When would you use `cherry-pick` in a real project?

- Use `git cherry-pick` when you want to copy one specific commit from one branch and paste it into another branch.
- Use this to fix a bug in production quickly or share a helpful fix without merging a whole messy branch.

## What can go wrong with cherry-picking?

- **Code Conflicts**: Git tries to force your chosen commit into the new branch. If the old code does not match the new code, Git stops and asks you to fix the lines manually.
- **Duplicate Commits**: If you cherry-pick a commit and later merge the whole branch, Git might get confused and add the same changes twice. This creates messy history.
- **Lost History Links**: Cherry-picking makes a brand-new commit with a new ID. It breaks the clean link back to the original branch and author timeline.
- **Untested Bugs**: The code might work on the old branch, but it can break things on the new branch because the surrounding files are different.
