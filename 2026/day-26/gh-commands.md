## GitHub CLI: Manage GitHub from Your Terminal

### Install and Authenticate

1. Install the GitHub CLI on your machine, go to https://cli.github.com/.

```bash
# For Linux
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
	&& sudo mkdir -p -m 755 /etc/apt/keyrings \
	&& out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
	&& cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
	&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
	&& sudo mkdir -p -m 755 /etc/apt/sources.list.d \
	&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
	&& sudo apt update \
	&& sudo apt install gh -y

# For Windows directly install '.msi' file.

# To verify it's installed.
gh version
```

2. Authenticate with your GitHub account

```bash
gh auth login
```
*What it does*: It used to login to your github account through your terminal.

*Example*: `gh auth login`

3. Verify you're logged in.

```bash
gh auth status
```
*What it does*: It displays your active account, verification state, and permission scopes for each known GitHub host.

*Example*: `gh auth status`

---

### Working with Repositories

1. Create a new GitHub repo directly from the terminal.

```bash
gh repo create
```
*What it does*: It creates a new GitHub repository directly from your terminal.

*Example*: `gh repo create`

2. Clone a repo.

```bash
gh repo clone <owner>/<git-repo>
```
*What it does*: It is used to clone a GitHub repository locally onto your machine.

*Example*: `gh repo clone vishch24/git-test`

3. View details of your repo.

```bash
gh repo view <owner>/<git-repo>
```
*What it does*: It is used to display the description and `README` of a GitHub repository directly inside your terminal.

*Example*: `gh repo view vishch24/git-test`

4. List all your repositories.

```bash
gh repo list
```
*What it does*: It lists repositories owned by a specific GitHub user or organization.

*Example*: `gh repo list`

5. Open current repo in your browser directly from the terminal.

```bash
gh browse
```
*What it does*: It opens the current GitHub repository directly in your default web browser.

*Example*: `gh browse`

6. Open any repo in your browser directly from the terminal.

```bash
gh browse <owner>/<git-repo>
```
*What it does*: It is used to quickly open a GitHub repository or specific project sections directly in your default web browser.

*Example*: `gh browse vishch24/git-test`

7. Open any public github repo in your browser directly from the terminal.

```bash
gh browse -R <owner>/<git-repo>
```
*What it does*: It opens the main homepage of any public github repository in your default web browser.

*Example*: `gh browse -R vishch24/git-test`

8. Open a current local Git repository mapped to GitHub.

```bash
gh repo view --web
```
*What it does*: It opens a GitHub repository directly in your default web browser.

*Example*: `gh repo view --web`

9. Delete a repo.

```bash
gh repo delete <owner>/<git-repo>
```
*What it does*: It permanently deletes a GitHub repository from your terminal.

*Example*: `gh repo delete vishch24/git-test`

---

### Issues

1. Create an issue.

```bash
gh issue create --title "Title" --body "Description" --label "bug/error"
```
*What it does*: It creates a GitHub issue with your specified title, description, and label.

*Example*: `gh issue create --title "Fix login page bug" --body "The login button is unresponsive on mobile devices." --label "bug"`

2. List all open issues on repo.

```bash
gh issue list
```
*What it does*: It prints a formatted table of all open issues in the current repository.

*Example*: `gh issue list`

3. View a specific issue by its number.

```bash
gh issue view <issue-id>
```
*What it does*: It is used to display the title, body, and details of a specific issue directly within your terminal.

*Example*: `gh issue view 42`

4. Close an issue from the terminal.

```bash
gh issue close <issue-id> --comment "Comment of resolve."
```
*What it does*: It closes the issue with the comment.

*Example*: `gh issue close 6 --comment "Resolved in PR #105."`

---

### Pull Requests

1. Create a pull request entirely from the terminal.

```bash
gh pr create
```
*What it does*: It creates a pull request on GitHub directly from your terminal.

*Example*: `gh pr create`

2. List all open PRs on a repo.

```bash
gh pr list
```
*What it does*: It lists open pull requests (PRs) in the current GitHub repository.

*Example*: `gh pr list`

3. Check status of your PR.

```bash
gh pr status
```
*What it does*: It shows a summary of relevant pull requests in your terminal for the current GitHub CLI repository.

*Example*: `gh pr status`

4. Check reviewers of your PR.

```bash
gh pr view --json reviewRequests,reviews
```
*What it does*: It fetches metadata about active review requests and the review history for a GitHub pull request in JSON format.

*Example*: `gh pr view --json reviewRequests,reviews`

5. PR checks.

```bash
gh pr checks
```
*What it does*: It is use to view the Continuous Integration (CI) and status check results of a specific GitHub Pull Request right inside your terminal.

*Example*: `gh pr checks`

6. Merge your PR from the terminal

```bash
gh pr merge <pr-id>
```
*What it does*: It is used to merge pull request by number in your current repository.

*Example*: `gh pr merge 7`

---

### GitHub Actions & Workflows (Preview)

1. List the workflow runs on any public repo that uses GitHub Actions.

```bash
gh run list --repo <owner>/<github-repo>
```
*What it does*: It lists recent GitHub Actions workflow runs for a specific public repository.

*Example*: `gh run list --repo actions/starter-workflows`

2. View the status of a specific workflow run.

```bash
gh run view <run-id> --repo <owner>/<github-repo>
```
*What it does*: It displays a detailed summary and real-time status of a specific GitHub Actions workflow run directly in your terminal.

*Example*: `gh run view 32053324676 --repo actions/starter-workflows`

---

### Useful `gh` Tricks

1. Fetch the authenticated user's profile info.

```bash
gh api user
```
*What it does*: It fetches profile information for the currently authenticated GitHub account.

*Example*: `gh api user`

2. Fetch metadata of a specific repository.

```bash
gh api repos/<owner>/<repo>
```
*What it does*: It is used to to make an authenticated HTTP GET request to the GitHub REST API to fetch metadata about a specific repository.

*Example*: `gh api repos/vishch24/git-practice`

3. Create a repository (POST).

```bash
gh api -X POST user/repos -f name="<repo-name>" -f private=true
```
*What it does*: It creates a new private GitHub repository under your authenticated user account.

*Example*: `gh api -X POST user/repos -f name="my-new-repo" -f private=true`

4. Star a repository (PUT).

```bash
gh api -X PUT user/starred/<OWNER>/<REPO>
```
*What it does*: It stars a specified GitHub repository on behalf of the currently authenticated user using the GitHub CLI.

*Example*: `gh api -X PUT user/starred/vishch24/my-new-repo`

5. Delete a repository (DELETE).

```bash
gh api -X DELETE repos/<OWNER>/<REPO>
```
*What it does*: It deletes the GitHub repository under your authenticated user account.

*Example*: `gh api -X DELETE repos/vishch24/my-new-repo`

6. Create a secret gist (Default behavior).

```bash
gh gist create <filename>
```
*What it does*: It creates a new secret GitHub Gist directly from your terminal.

*Example*: `gh gist create user.txt`

7. List your gists.

```bash
gh gist list
```
*What it does*: It displays a list of Gists from your user account.

*Example*: `gh gist list`

8. View a specific gist.

```bash
gh gist view <gist-id>
```
*What it does*: It is used to display the content of a specific GitHub Gist directly in your terminal.

*Example*: `gh gist view df56fsdg1sadg`

9. Edit an existing gist.

```bash
gh gist edit <gist-id>
```
*What it does*: It is used to modify your existing GitHub Gists directly from your terminal.

*Example*: `gh gist edit df56fsdg1sadg`

10. Delete a gist.

```bash
gh gist delete <gist-id>
```
*What it does*: It is used to permanently delete a specific GitHub Gist from your account.

*Example*: `gh gist delete df56fsdg1sadg`

11. Create release.

```bash
gh release create <tag>
```
*What it does*: It is used to create a new GitHub release and deploy a matching Git tag to your remote repository.

*Example*: `gh release create v1`

12. Display details and notes for a specific release.

```bash
gh release view <tag>
```
*What it does*: It is used to display comprehensive information about a specific GitHub release in your terminal.

*Example*: `gh release view v1`

13. Show a list of repository releases.

```bash
gh release list
```
*What it does*: It is used to display a list of releases within a GitHub repository.

*Example*: `gh release list`

14. Add extra files to an existing release.

```bash
gh release upload <tag> <files>
```
*What it does*: It is used to upload asset files to an existing GitHub release matching the specified Git tag.

*Example*: `gh release upload v1 dashboard.txt`

15. Retrieve files and binary assets attached to a release.

```bash
gh release download <tag>
```
*What it does*: It is used to download all compiled asset files associated with a specific version release.

*Example*: `gh release download v1`

16. Remove a release.

```bash
gh release delete <tag>
```
*What it does*: It is used to delete a specific release from a GitHub repository.

*Example*: `gh release delete v1`

17. Create a new custom shortcut.

```bash
gh alias set <alias_name> '<expansion>'
```
*What it does*: It creates shortcuts for the GitHub CLI to save you time on frequently used commands.

*Example*: `gh alias set pco 'pr checkout'`

18. Display all your currently active shortcuts.

```bash
gh alias list
```
*What it does*: It prints a complete list of all custom command shortcuts currently configured in your GitHub CLI.

*Example*: `gh alias list`

19. Remove an existing shortcut.

```bash
gh alias delete <alias_name>
```
*What it does*: It removes a shortcut name (alias) you previously created.

*Example*: `gh alias delete pco`

20. Load shortcuts directly from a YAML file.

```bash
gh alias import <file_name>
```
*What it does*: It is used to bulk-import shortcuts and command expansions directly from a YAML configuration file.

*Example*: `gh alias import aliases.yml`

21. Look for GitHub repositories matching keywords, topics, or languages.

```bash
gh search repos <query> <flags>
```
*What it does*: It is used to search for repositories on GitHub directly from your terminal.

*Example*: `gh search repos "workflows" --owner=actions`

22. Search for specific code snippets, variables, or functions across repositories.

```bash
gh search code <query> <flags>
```
*What it does*: It is used to search for code across GitHub repositories directly from your terminal.

*Example*: `gh search code "AuthenticationError" --owner=my-org --language=go`

23. Find issues across repositories based on labels, authors, or text.

```bash
gh search issues <query> <flags>
```
*What it does*: It is used to search for issues and pull requests across GitHub repositories directly from your terminal.

*Example*: `gh search issues bug --repo cli/cli --state open`

24. Search for pull requests across GitHub.

```bash
gh search prs <query> <flags>
```
*What it does*: It is used to search for pull requests across GitHub. It looks for pull requests matching your specified keywords, filters, or search syntax.

*Example*: `gh search prs "bug fix" --repo="cli/cli" --state=merged`

25. Find specific commit messages, hashes, or authors.

```bash
gh search commits <query> <flags>
```
*What it does*: It is used to search through commit messages, metadata, and code changes across GitHub repositories.

*Example*: `gh search commits "bugfix" --repo cli/cli`
