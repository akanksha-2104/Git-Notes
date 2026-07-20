# Additional commands

A grab-bag of Git commands that don't belong to one single workflow, but come up often enough to be worth knowing: forking, global config, tags, stashing, `.gitignore`, and a few cleanup commands.

## Forking

**Forking** is how you get your own copy of someone else's repository under your own GitHub account — separate from `git clone`, which just downloads a copy to your local machine without creating a new remote copy under your account.

Typical flow after forking on GitHub:

```bash
git clone <your-fork-url>              # clone your fork to your local machine
git remote add upstream <original-repo-url>   # link back to the original repo
git fetch upstream                     # pull down the original repo's latest changes
```

The `upstream` remote lets you keep your fork in sync with the original project over time.

## Global configuration

```bash
git config --global user.name "username"
git config --global user.email "useremail"
```
Sets your identity globally — applied to every repository on your machine, not just the current one.

```bash
git config --global --unset user.name
git config --global --unset user.email
```
Removes the globally configured username/email.

```bash
git config --global --edit
```
Opens the global config file directly in your default editor, so you can view or edit everything at once instead of one setting at a time.

## Editing a commit

```bash
git commit --amend
```
Edits the most recent commit. By default this lets you rewrite the commit message — but if you `git add` new changes before running it, those changes get folded into that same commit too, rather than creating a new one.

> Like `reset` and `rebase`, avoid amending a commit that's already been pushed and shared — it changes the commit's hash.

## Unstaging a file

```bash
git restore --staged <file-name>
```
Moves a file back from the staging area to the working directory — *unstaging* it without discarding the actual changes (unlike plain `git restore <file-name>`, which would discard them).

## .gitignore

```bash
vi .gitignore
```
Opens (or creates) a `.gitignore` file, where you list patterns for files or folders Git should ignore entirely — they won't show up as untracked, and `git add .` will skip them.

```bash
cat .gitignore
```
Shows the ignore patterns you've written. To see which actual files are currently being ignored as a result, use:

```bash
git status --ignored
```

## Tags

Tags mark a specific commit as significant — typically used for release versions.

```bash
git tag v1.1
```
Creates a tag on the latest commit.

```bash
git tag v1.2 <commit-id>
```
Creates a tag on a specific commit rather than the latest one.

```bash
git push origin --tags
```
Pushes all local tags to the remote repository (tags aren't included in a normal `git push` by default).

```bash
git tag -l
```
Lists all tags.

```bash
git tag -d <tag-name>
```
Deletes a tag locally.

```bash
git push origin --delete <tag-name>
```
Deletes a tag on the remote (GitHub).

## Stash

Stashing saves your uncommitted changes to a temporary storage area, giving you a clean working directory — without committing anything.

```bash
git stash
```
Stashes all current changes.

```bash
git stash list
```
Shows all stashed entries.

```bash
git stash apply stash@{0}
```
Restores a stash's changes back into your working directory, **keeping it in the stash list**.

```bash
git stash pop
```
Restores the most recent stash **and removes it from the list** in one step — apply + drop combined. This is the one you'll usually reach for.

```bash
git stash drop stash@{0}
```
Deletes a specific stash without applying it.

```bash
git stash push -m "message" <file-name>
```
Stashes a specific file, with a custom message to make it easier to identify later.

## Remote and clone

```bash
git remote remove origin
```
Removes the connection between your local repository and the remote.

```bash
git clone <github-url>
```
Downloads a full copy of a remote repository — files and commit history — to your local machine.
