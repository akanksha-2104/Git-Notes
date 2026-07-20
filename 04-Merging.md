# Merging

Combining the changes from one branch into another branch is called **merging**.

```bash
git merge branchName
```

> **Be careful which branch you're on before running this.** `git merge branchName` merges `branchName` **into** whatever branch you're currently on — it does not merge the other way. Always check with `git branch` (or `git status`) that you're standing on the branch you want to merge *into* before running the command.

There are two ways a merge can play out, depending on how the two branches' histories relate to each other.

## Fast-forward merge (a "2-way" merge)

Happens when the branch you're merging **into** hasn't had any new commits since the other branch was created. Git doesn't need to combine two histories — it just moves the branch pointer forward to catch up. No new commit is created; the history stays a straight line.

### Task — fast-forward merge

On the **master** branch:
1. Create 4 files — `a1.txt`, `a2.txt`, `a3.txt`, `a4.txt`.
2. **Commit 1:** add all four empty files.
3. **Commit 2:** modify `a2.txt` and commit it.

Now:
4. Create and switch to a **feature** branch.
5. **Commit 3:** modify `a3.txt` and commit it.
6. **Commit 4:** modify `a4.txt` and commit it.
7. Switch back to **master** and merge the feature branch:
   ```bash
   git switch master
   git merge feature
   ```

Since master never received any commits after the feature branch was created, Git simply fast-forwards master to feature's latest commit. No merge commit appears in the log.

## Three-way merge ("optimized three-way" merge)

Happens when **both** branches have new commits since they diverged — master moved forward *and* feature moved forward. Git can't just slide a pointer this time, since neither branch is a straight continuation of the other. Instead, it creates a new **merge commit**: a commit with two parent commits, one from each branch, combining both histories into one.

### Task — three-way merge

1. Initialize a local repository and create 4 files: `a1.txt`, `a2.txt`, `a3.txt`, `a4.txt`.

On **master**:
2. **Commit 1:** commit all four empty files.
3. **Commit 2:** modify `a1.txt` and commit it.

Now create and switch to the **feature** branch (branching off right after commit 2):
4. **Commit 3:** modify `a2.txt` and commit it.
5. **Commit 4:** modify `a3.txt` and commit it.

Switch back to **master**:
6. **Commit 5:** modify `a4.txt` and commit it — this is the step that makes master diverge from feature. Without a new commit on master after the branch point, the merge would just fast-forward, like in the first task.

Now merge:
```bash
git switch master
git merge feature
```

Because both master (commit 5) and feature (commits 3 and 4) have moved since they diverged, Git creates one extra commit — the merge commit — to tie the two histories together.

> **A variant worth knowing about:** if instead of touching a *different* file in commit 5, master modifies the **same file** feature already changed (say, `a1.txt` again, on overlapping lines), Git may not be able to combine the changes automatically. That's a **merge conflict**, covered next.

## Merge conflicts

Both fast-forward and three-way merges above went smoothly because the two branches never touched the *same lines* of the *same file*. A **merge conflict** happens when they do — Git can combine most changes automatically, but if both branches edited the same lines differently, it has no way to know which version you want. It stops the merge partway through and asks you to decide.

### What a conflict looks like

When you run `git merge` and a conflict occurs, Git tells you which files are affected and pauses the merge — nothing is finalized yet. Opening the conflicted file, you'll see it's been edited in place with **conflict markers**:

```
<<<<<<< HEAD
This is the line as it exists on your current branch (master).
=======
This is the line as it exists on the branch you're merging in (feature).
>>>>>>> feature
```

- `<<<<<<< HEAD` marks the start of **your current branch's** version.
- `=======` separates the two versions.
- `>>>>>>> feature` marks the end of the **incoming branch's** version.

Running `git status` during a conflict will list the file(s) as **"both modified,"** which is Git's way of confirming this is a conflict, not just a normal pending merge.

### Resolving a conflict

1. Open each conflicted file and decide what the final content should be — this might mean keeping your version, keeping the incoming version, or writing something that combines both.
2. **Delete the conflict markers themselves** (`<<<<<<<`, `=======`, `>>>>>>>`) — they're not valid code/text and Git won't remove them for you.
3. Stage the resolved file:
   ```bash
   git add filename
   ```
4. Once every conflicted file is resolved and staged, complete the merge with a commit:
   ```bash
   git commit
   ```
   Git will pre-fill a commit message noting it was a merge, which you can keep or edit.

### Backing out of a conflict

If a merge conflict turns out to be more than you want to deal with right now, you can cancel the merge entirely and return to the state you were in before running `git merge`:

```bash
git merge --abort
```

### Task — cause and resolve a conflict

1. Initialize a local repository and create `a1.txt` with the single line `Hello World`. Commit it on **master**.
2. Create and switch to a **feature** branch.
3. On **feature**, change the line in `a1.txt` to `Hello from feature branch` and commit it.
4. Switch back to **master**. Change the *same line* in `a1.txt` to `Hello from master branch` and commit it.
5. Merge feature into master:
   ```bash
   git switch master
   git merge feature
   ```
6. Git will report a conflict in `a1.txt`. Open the file and look at the conflict markers.
7. Decide on a final version of the line, remove the conflict markers, save the file.
8. ```bash
   git add a1.txt
   git commit
   ```
9. Run `git log --oneline --graph --all` and confirm the merge commit now appears, same as a three-way merge — a resolved conflict still results in a merge commit, since both branches had diverged.

## Seeing the difference

```bash
git log --oneline --graph --all
```

- After a **fast-forward** merge: a single straight line of commits — you can't tell from the graph that a separate branch was ever involved.
- After a **three-way** merge: the graph visibly splits into two lines after the common commit, then rejoins at the merge commit.

## Extra: forcing a merge commit even when fast-forward is possible

Sometimes you *want* a merge commit in the history — as a marker that a feature branch was merged — even when Git could have fast-forwarded. Use:

```bash
git merge --no-ff branchName
```
