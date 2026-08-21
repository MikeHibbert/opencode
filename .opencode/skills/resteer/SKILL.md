---
name: resteer
description: Periodically re-anchor to the user's actual intent with a self-correcting questionnaire. Use when a task has run long, the thread feels like it is drifting, you are unsure whether a decision is yours to make, or you suspect you are inventing work to keep a convention alive.
license: MIT
compatibility: opencode
metadata:
  audience: agents
  cadence: periodic
---

# Re-Steer

A self-correcting questionnaire that periodically re-anchors you to the user's actual intent. Run it when a task has run long, the thread feels like it is drifting, or you are about to make a consequential decision. It has two tiers: a **QUICK** check (3 questions) and a **FULL STEERING REVIEW** (6 questions).

## When to use me

- A task has been running for a while and you are not sure you are still doing what the user asked.
- The conversation has drifted, or you have lost the thread of the original request.
- You are about to make a decision that a human might need to gate (merge, approve, publish, payment, irreversible action).
- You suspect you are doing work just to keep a convention or a habit alive rather than because it serves the user.

## QUICK (3 questions)

Run these three whenever you feel the need to re-anchor. Answer honestly, then act on the result.

1. **Direction** — What is the user actually asking me to do right now?
   - **A = Know.** I know the intent and my next action serves it. → Continue.
   - **B = Drifted.** I have moved away from the user's intent. → **Re-anchor**: restate the user's actual request, drop work that does not serve it, and confirm the next step with the user before continuing.
   - **C = Lost thread.** I no longer know what the user wants. → Stop and ask the user to restate the goal before doing anything else.

2. **Role** — Is this decision mine to make, or is there a human gate?
   - **A = Mine.** The decision is within my authority (implementation detail, reversible choice). → Proceed.
   - **B = Human gate.** The decision involves merge, approve, publish, payment, or any irreversible action. → **Stop and hand back.** Do not proceed; present the decision and the options to the user and wait for their call.

3. **Honesty** — Am I inventing work to keep a convention alive?
   - **A = Real.** The work is genuinely needed and serves the user. → Continue.
   - **B = Fabricating.** I am inventing work to keep a habit or convention going. → **Don't.** Stop the invented work. Do not manufacture tasks to preserve a routine. Say plainly that the work is not needed and let the user decide.

## FULL STEERING REVIEW (adds 3 more)

Run the full review when the task is long-running, high-stakes, or you have not re-anchored in a while. Answer all six questions.

4. **Progress** — Am I moving forward with new information, or stuck re-looping?
   - If you are re-looping over the same ground without new information, stop and re-anchor: name the loop, state what would count as progress, and confirm the next step with the user.

5. **Boundaries** — Are my context facts right?
   - Verify the facts you are acting on: the correct paths, who actually merges or approves, and what is actually live. If any fact is wrong, correct it before acting. Do not act on stale or assumed context.

6. **Redirect** — Has the user changed direction since I last re-anchored?
   - If the user has given new direction since your last re-anchor, adopt it. Discard work that no longer matches the new direction, and confirm the updated goal with the user.

## How to act on the answers

- **Drift (1B / 1C)** → Re-anchor: restate the user's real request, drop work that does not serve it, confirm the next step.
- **Human gate (2B)** → Stop and hand back: present the decision to the user and wait. Never cross a merge/approve/publish/payment/irreversible gate on your own.
- **Fabricating (3B)** → Don't: stop the invented work, say plainly it is not needed, and let the user decide.
- **Stuck (4B)** → Break the loop: name it, define what progress looks like, and re-anchor.
- **Wrong facts (5)** → Correct the context before acting.
- **Redirected (6)** → Adopt the new direction and drop stale work.

## When to re-run

- After any long stretch of autonomous work.
- Before any decision that touches a human gate.
- Whenever you notice yourself drifting, looping, or inventing work.
- Whenever the user changes direction.

## References

- OpenCode Agent Skills documentation: `packages/web/src/content/docs/skills.mdx` — skill placement, frontmatter, and name rules.
- OpenCode skill loading: `packages/core/src/skill.ts` — how `SKILL.md` files are discovered and parsed.
