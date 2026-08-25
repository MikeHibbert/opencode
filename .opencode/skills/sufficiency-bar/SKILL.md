---
name: sufficiency-bar
description: Set an explicit "enough to act" bar BEFORE doing read-only research, then respect it — read once, act, ship a mechanical artifact (diff / PR / test result) instead of looping in recon or narrating intent. Use when a task involves multi-step investigation, code changes, or any plan that risks drifting into endless reading or plan-only replies.
license: MIT
compatibility: opencode
metadata:
  audience: agents
  cadence: periodic
---

# Sufficiency Bar

The single most costly failure mode for an autonomous agent is **analysis without remediation**: spending tool-round after tool-round reading and re-reading while shipping nothing. The fix is not "stop investigating" — it is to *pre-commit* a concrete bar for "I know enough to act," then honor it. Read until you reach that bar, then write.

This skill distils the discipline learned from long-running agent pathology (recon loops at 119 tool-rounds / ~15:1 read:write with zero commits; specialists that returned intent-narration and no artifact; an ungrounded planner that fabricated the team it then failed to route).

## The core rule

**Decide *before* you start researching what counts as "enough to act", then enforce it mechanically.**

Without a pre-committed bar, "a bit more evidence" always looks justified and research drifts upward forever. With one, you stop when you hit it and make the change.

## 1. Pre-commit a recon budget

Before you read a single file, state the budget:

- **How many read-only tool rounds** before you must either write or report a real blocker. A sensible default for a focused change is **2-3 reads; at most 6 read-only rounds**. Reading the same file a 4th time is never progress.
- **What you will ship** at the end, in mechanical terms: a commit hash, a PR URL, a test result (`N passed`), a code diff — *not* a plan of what you would do.
- A read-only tool is anything that does not change repo state: read file, grep, list, search, git status/log/diff/branch. A mutating tool is edit/write/commit/push/run-tests. Count only the trailing run of consecutive read-only rounds since your last mutation.

## 2. Ground yourself in the ACTUAL source, once

- If you need a file, read it. Do not guess its contents from memory or naming.
- Prefer the real tool/target list available to you rather than assuming names exist. Route to things that actually exist; do not fabricate a target that a downstream step would then fail to find.
- When your context is large, prefetch the specific files that matter and reason from them — but do this as *preparation*, capped, not as an open-ended tour.

## 3. Reach the bar, then WRITE

- Once you can write the change, write it. Do not "check a few more things first."
- Fixes that are small should execute immediately; do not re-plan or re-read before a ≤2-round fix.
- After writing, verify by a mechanical check (tests, git status, diff) — not by re-reading the file you just changed to re-confirm.

## 4. Ship an artifact, never a narration

- A final answer that is only "I'll do it / let me read X / my plan is..." is a failure, not a response.
- Deliver the concrete artifact: the change, the test result, the commit, the PR URL. If you truly cannot, report the *exact* error message and the tool call that produced it — never a fabricated blocker.
- If a branch/target already exists (e.g. "branch already exists"), resolve it deterministically (list local + remote branches in the same run) instead of retrying the same failing command.

## 5. Ground the planner before it plans

The plan that names the wrong targets is worse than no plan. Before generating a multi-step plan or delegating, ground yourself in what actually exists:

- **Known targets / kindlings / namespaces**: the real set of named things a step could route to.
- **Available tools**: the concrete tool surface, not a guess.
- If a plan would mint a name/target that does not match the available set, correct the name to a real one rather than letting a downstream step fail on a fabricated identifier.

## 6. Treat evaluation as a probe, not a verdict

When you test a change against a task (or compare two approaches):

- An eval is a **probe that sums up state and feeds the next iteration**, not a pass/fail stamp.
- On any break, extract the **failure signature** (which round, which tool, what artifact was missing) and feed it back into the next attempt as a concrete steering goal. Then re-probe.
- Never mark a run "completed" without a mechanical deliverable. If the only output is analysis, it is not done.

## When to use me

- Any multi-step or code task where you could drift into endless read-only investigation.
- When asked to plan a team / multi-specialist / delegated workflow.
- Before giving a final answer on an implementation task.
- When you catch yourself re-reading the same ground without new information.

## Anti-patterns (stop these)

- Reading the same file repeatedly "to be sure."
- A final reply that narrates what you would do instead of reporting what you did.
- Marking a task complete after analysis-only.
- Retrying a failing command unchanged in the hope it works.
- Planning against fabricated names/targets that no real component matches.

## When to re-run

- Start of any non-trivial or multi-step task (set the budget up front).
- Whenever you notice yourself looping in read-only recon.
- Before the final reply of any implementation task.

## References

- OpenCode Agent Skills documentation: `packages/web/src/content/docs/skills.mdx` — skill placement, frontmatter, and name rules.
- OpenCode skill loading: `packages/core/src/skill.ts` — how `SKILL.md` files are discovered and parsed.
