---
name: qskill-push-code
description: Use when asked to commit current code changes, push the branch, and open a pull request through the `/qskill push-code` workflow.
---

# Qskill Push Code

Use this skill only for the requested repository changes. Preserve the user's existing work and identity settings.

## Workflow

1. Inspect repository state before mutating it:

   ```bash
   git status --short --branch
   git branch --show-current
   git diff --cached --name-only
   git remote -v
   ```

   Stop if this is not a Git repository, the branch is detached, or there are no changes to commit. Do not discard, reset, stash, or overwrite user changes.

2. Choose the branch. If the current branch is exactly `main` or `trunk`, create and check out a new branch from it. Classify the requested change using one of these prefixes:

   | Prefix | Use for |
   | --- | --- |
   | `feat/` | New feature |
   | `fix/` | Bug fix |
   | `docs/` | Documentation |
   | `refactor/` | Behavior-preserving restructuring |
   | `test/` | Adding or fixing tests |
   | `chore/` | Maintenance or housekeeping |
   | `perf/` | Performance improvement |
   | `build/` | Build system or dependencies |
   | `ci/` | CI/CD changes |
   | `style/` | Formatting or style only |
   | `revert/` | Reverting a previous change |

   Form the remainder as a short lowercase kebab-case summary: `{prefix}{short-content}`. If that branch name already exists, add a minimal numeric suffix rather than replacing or checking out unrelated work. If the change type or summary cannot be inferred, ask before creating the branch. If the current branch is anything other than `main` or `trunk`, continue on it and do not create a branch.

3. Review the diff and run the repository's relevant checks before committing. Do not modify unrelated files. Confirm the staged diff contains only intended changes.

4. Stage changes according to the existing index:

   - If the staged index is empty, run `git add -A` to stage all changes, including deletions and untracked files.
   - If any files are already staged, do not run `git add`; preserve the user's staging selection.

   If nothing is staged after this, stop without making an empty commit.

5. Commit with a concise message of no more than two lines. Do not pass `--author` and do not add Codex or ChatGPT as author or co-author. Use the repository's configured user identity. If no identity is configured, or the configured identity is clearly an agent identity, stop and ask the user to configure their own Git author; never silently change it.

6. Push the current branch and set its upstream when needed:

   ```bash
   git push -u origin <branch>
   ```

   Use the repository's actual push remote if it is not named `origin`. Stop and report the error if authentication, permissions, or remote configuration prevents pushing.

7. After a successful push, create the pull request using the available repository tooling, preferably GitHub CLI:

   ```bash
   gh pr create --fill --base <base-branch> --head <branch>
   ```

   Use the source branch as the base when a new branch was created from `main`/`trunk`; otherwise use the repository's default base branch unless the user specified another one. Do not claim a PR was created until the command succeeds. Return the created PR's URL prominently so the user can open it.

## Completion report

Report the final branch, commit ID, push result, and pull-request URL. If any step stops or fails, report the exact completed steps and the next action required; do not imply completion.
