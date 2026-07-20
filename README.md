# Git-Notes

A self-study reference for learning Git and GitHub from first principles — theory, diagrams, hands-on tasks, and the exact commands to complete them.

This repo exists as a personal study and revision resource, built while learning Git and GitHub. Sharing it in case it's useful to anyone else on the same journey.

## How to use this repo

Each file covers one concept, in this order:

1. **Theory** — a short explanation of the concept.
2. **Diagram** — a visual to make the idea click.
3. **Task** — a hands-on exercise.
4. **Commands** — the exact Git commands needed to complete the task, plus screenshots of them actually running.

Follow the files in numbered order if you're starting from scratch, or jump straight to a topic if you already know the basics.

## Contents

| # | Topic | What's covered |
|---|-------|-----------------|
| 1 | [Git Introduction](01-git-intro.md) | The problem version control solves — building software without it, and where it breaks down | Local vs. centralized vs. distributed VCS, why Git wins, and the working directory → staging area → local repo → remote repo pipeline |
| 2 | [Git workflow: init to push](02-Git-workflow.md) | `init`, `config`, `add`, `commit`, `remote`, `push`, `pull` — plus two hands-on tasks including the non-fast-forward error and how to resolve it |
| 4 | [Branching](03-Branching.md) | What a branch is, `HEAD`, common branch types (main, feature, release, hotfix), and the core branching commands |
| 5 | [Merging](04-Merging.md) | Fast-forward vs. three-way merges, how to spot the difference, and where merge conflicts come from |
| 6 | [Undoing changes](05-Undoing.md) | `reset` (`--soft` / `--mixed` / `--hard`), `checkout` / `restore`, `revert`, `cherry-pick`, and `rebase` |
| 7 | [Additional commands](06-Additional-Commands.md) |`gitignore`, `amend`, `clone`, `fork`, `tag`, `remove`, `stash` |

<!--
## Repo structure

```
gitnotes/
├── README.md                          ← you are here
├── 01-git-intro.md
├── 02-git-workflow.md
├── 03-branching.md
├── 04-merging.md
├── 05-undoing-changes.md
├── 06-additional-commands.md
├── assets/
│   ├── images/                        ← concept diagrams
│   └── screenshots/                   ← Git Bash command output, by task
```
-->
## Found an error?

These are personal notes, corrected and refined as I go. If you spot a mistake or think something could be explained better, feel free to open an issue or a PR.
