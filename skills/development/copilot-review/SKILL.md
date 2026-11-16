---
name: copilot-review
description: Local stand-in for GitHub Copilot code review. Reviews the branch's pull request changes against the instruction files Copilot reads, reports every finding in one pass with Copilot's severity levels, and with --fix keeps fixing and re-reviewing with a fresh reviewer until no High or Medium finding remains.
when_to_use: Use before pushing or requesting a Copilot review, when asked to review like Copilot or to pre-review a pull request, or to break a loop where each Copilot re-review finds something new.
argument-hint: '[--fix] [base-branch]'
user-invocable: true
metadata:
  author: Weslley Araújo
  version: '1.0.0'
  source: https://github.com/wellwelwel/skills
---

# Pull request pre-review

**GitHub Copilot** code review reports only part of what it could find in each pass, so every fix and re-request surfaces something new. This skill does the whole sweep locally, before the pull request goes back to **Copilot**.

## Scope

- Compare against the branch given as an argument, or else the pull request's base branch, or else the remote's default branch.
- Review everything that will be pushed: the working tree against the merge base, untracked files included.
- Skip what **Copilot** skips: dependency manifests and lockfiles, logs, and SVG files.

## Criteria

Read these files from the working tree, since **Copilot** reads them from the pull request's head branch. Read each one once, even when several names link to the same file.

- `.github/copilot-instructions.md`.
- `.github/instructions/**/*.instructions.md`, only when a changed file matches its comma-separated `applyTo` globs, and never when it sets `excludeAgent: "code-review"`.
- `AGENTS.md` (the nearest one to each changed file), `CLAUDE.md`, `GEMINI.md`, and `REVIEW.md`.
- Skills under `.github/skills`.

Their rules about code, tests, and docs are review criteria, while instructions about a coding agent's workflow are not. A statement that something is intentional suppresses findings about it.

When the branch has an open pull request, also collect **Copilot**'s inline comments (author `Copilot`) and its latest review summary, using `gh` or, when it is not authenticated, the public REST API. Each comment the current code does not already resolve becomes a finding to verify.

## Review

- Read each changed hunk together with its surrounding code, callers, tests, and docs. **Copilot** gathers context from the whole repository, so a diff-only read misses what it catches.
- Check correctness first (wrong conditions, edge inputs, error and early-exit paths, async ordering, resource cleanup, cross-platform behavior), then security, performance, public API compatibility, and consistency across code, types, tests, docs, and messages, typos included.
- When the change touches state that outlives a single call (module-level variables, globals, singletons, wrapped or patched objects, caches), enumerate its lifecycle scenarios before judging the code: repeated calls with different options, nested and re-entrant calls, third parties holding references (plugins, callbacks), frozen or non-configurable targets, and failures midway. Check every scenario against the diff, since otherwise **Copilot** surfaces them one per round.
- Report a finding only when it points to a line and rests on a concrete scenario (inputs or state that produce a wrong outcome) or on a rule from the criteria. Drop speculation, and never cap the number of findings.

Grade each finding with the levels **Copilot** uses:

- High: wrong behavior, crashes, data loss, security issues, or a broken public API.
- Medium: failures on edge inputs or lifecycle scenarios, new behavior without tests, or code that contradicts its docs.
- Low: naming, typos, readability, and small cleanups.

## Report

List findings from most to least severe, each with its severity, location, failing scenario, and concrete fix. Close with one verdict: ready for **Copilot**, changes recommended, or needs a closer look when the problem is the design rather than a line.

## Fix loop

Runs only with `--fix`, and never commits or pushes.

1. Fix the confirmed findings. When the project has a test suite, turn each reproducible scenario into a regression test, so a later fix cannot quietly reopen it.
2. Run the project's own checks, through its pre-commit workflow when it has one.
3. Re-review with a fresh general-purpose subagent that reads this skill and runs it without `--fix` against the same base. Give it nothing from earlier passes, because an unanchored read is what **Copilot**'s next round is. At highest reasoning effort, run two in parallel and merge their findings.
4. Repeat from step 1 until a pass has no High or Medium finding, with at most three re-reviews.
5. Stop early when two consecutive passes find new scenarios in the same mechanism. That pattern means the design needs less shared state and another patch only moves the problem, so report it as a closer-look verdict.

## Intentional choices

Replies on **GitHub** never reach **Copilot**, and resolved or downvoted comments may return in later reviews, so only instruction files change what it reports. When a finding describes a deliberate choice, propose a rule for the file the repository already uses for review instructions (or `REVIEW.md` when there is none) that states what is intentional and why, and add it only after the user agrees. **Copilot** does not support vague rules such as "be more accurate", so the rule must name the exact behavior.
