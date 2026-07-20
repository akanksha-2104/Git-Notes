# Undoing changes

Reversing something you've done is called **undoing changes**. Mistakes happen while working on a project, and Git gives you several different commands to undo them — the right one depends on exactly what you want to reverse, and whether it's already been shared with anyone else.

The main tools are:

1. Reset
2. Checkout / Restore
3. Revert


## Reset

`git reset` undoes changes by moving your current branch pointer (HEAD) back to a specific previous commit. Depending on the flag used, it also moves files between the **local repository**, **staging area**, and **working directory**.

```bash
git reset --soft <commit>
git reset --mixed <commit>   # this is also the default if you run `git reset` with no flag
git reset --hard <commit>
```

> ⚠️ **Reset rewrites commit history.** Once you reset past a commit, that commit is no longer part of your branch's history (though it isn't gone immediately — Git keeps it around for a while and it's recoverable via `git reflog` if needed). Avoid resetting commits you've already pushed and shared with others.

**`--soft`**
Moves HEAD back, but leaves the staging area and working directory untouched. Since the staging area still holds the newer state, the commits you "undid" reappear as **staged changes** — ready to be committed again, edited, or split differently.

**`--mixed`** *(the default)*
Moves HEAD back **and** resets the staging area to match that commit, but leaves the working directory untouched. The result: your undone changes are still there in your files, but now appear as **unstaged** modifications.

**`--hard`**
Moves HEAD back, resets the staging area, **and** overwrites your working directory files to match that commit exactly. Any changes after that point — committed or not — are discarded from your files. This is the most destructive option; use it only when you're sure you don't need those changes.

## Checkout / Restore

`git checkout <file>` restores a file back to its last committed (or staged) version, discarding any uncommitted local edits to that file.

```bash
git checkout filename
git checkout .
```

The newer, purpose-built alternative (Git 2.23+) is `git restore`, which does the same job without checkout's overloaded, multi-purpose behavior:

```bash
git restore filename
git restore .
```

### Task — undo an uncommitted change

1. Create `a1.txt`.
2. Commit it.
3. Add some extra text to the file.
4. **Don't commit** — check the status (`git status`) and notice the file shows as modified.
5. Restore the file:
   ```bash
   git restore a1.txt
   # or
   git checkout a1.txt
   ```
6. Check the status again and open `a1.txt` — the extra text is gone, back to the last committed version.

## Revert

`git revert` undoes the changes introduced by a specific commit — but instead of erasing that commit from history, it creates a **new commit** that applies the opposite change.

```bash
git revert commit_id
```

This makes revert the safer choice for undoing something on a branch that's already been pushed and shared: nobody's history gets rewritten, and there's a clear, visible record of both the original change and the fact that it was undone.

**Reset vs. revert, in short:** reset rewrites history (use on local/unshared commits); revert preserves history and adds to it (use on shared/pushed commits).


## Additional Concepts
## Cherry-pick

`git cherry-pick` applies the changes from one specific commit onto your current branch — useful when you want just *one* commit from another branch, not the whole branch.

```bash
git cherry-pick commit_id
```

Since it's copying a change into a different context, it's possible to hit a conflict if the surrounding code has diverged — if that happens, `git cherry-pick --abort` backs out cleanly.

## Rebase

`git rebase` rewrites commit history by moving a whole sequence of commits to a new starting point (a new "base"), replaying them one by one on top of it. The result is a clean, **linear** history — no merge commit, unlike a three-way merge.

Typical usage: check out the branch you want to move, then rebase it onto the branch you want as the new base.

```bash
git switch feature
git rebase master
```

This takes all the commits unique to `feature` and replays them on top of `master`'s latest commit.

> ⚠️ **Never rebase commits that have already been pushed and shared with others.** Rebase gives every replayed commit a new hash, so anyone else who already has the old commits will run into major conflicts trying to reconcile their history with yours. Rebase freely on local, unshared work — avoid it on shared branches.

If a rebase runs into conflicts partway through, `git rebase --abort` returns you to the state before you started.
