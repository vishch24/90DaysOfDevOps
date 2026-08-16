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
