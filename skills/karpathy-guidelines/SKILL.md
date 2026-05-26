---
name: karpathy-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Invoke before writing, reviewing, or refactoring code to run the pre-coding checklist, surface assumptions, avoid overcomplication, make surgical changes, and define verifiable success criteria.
license: MIT
---

# Karpathy Guidelines

Behavioral guidelines to reduce common LLM coding mistakes, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## When to Invoke

Invoke at the start of any non-trivial task: before writing new code, before modifying existing code, before a code review. Skip for obvious one-liners.

## Pre-Coding Checklist

Run these four questions before writing a single line of code:

```
[ ] 1. ASSUMPTIONS: What am I assuming about scope, format, data, or behavior?
        → State them explicitly. Ask about any that could be wrong.

[ ] 2. AMBIGUITY: Are there multiple valid interpretations of this request?
        → List them. Don't pick silently.

[ ] 3. SIMPLICITY: Is there a simpler approach than what I'm about to build?
        → If yes, propose it. Push back if warranted.

[ ] 4. SUCCESS: How will I know when I'm done?
        → Define at least one verifiable check before starting.
```

If you can't answer #4, stop and ask. Weak success criteria ("make it work") require constant clarification.

## Failure Modes

These thoughts mean STOP — you're rationalizing:

| Thought | Reality |
|---------|---------|
| "The request is obvious, I'll just start" | Obvious requests have hidden assumptions. Run the checklist. |
| "I'll add this since it seems useful" | Only build what was asked. Speculative features add bugs. |
| "Let me clean this up while I'm here" | That's drive-by refactoring. Touch only what you must. |
| "I'll handle this edge case too" | If it wasn't in the request, mention it — don't build it. |
| "This will be more flexible if I abstract it" | Abstractions for one use case add complexity, not value. |
| "I'll define success criteria after I code it" | That's rationalization. Define done before you start. |
| "The existing style is bad, I'll improve it" | Match the style even if you'd do it differently. |
| "Of course, great idea!" | If you think the approach is wrong, say so first. |
| "I know that method/API exists" | Library APIs change and hallucinate. Confirm before using. |
| "It looks right, I'm done" | Run it. Verified means observed output, not plausible output. |
| "I'll sanitize inputs later" | SQL/shell injection and hardcoded secrets are cheaper to prevent than fix. |

## The Four Principles

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- Read the relevant existing code before writing. Don't assume what's there or how it works.
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them and name your recommendation with one sentence of reasoning — don't be agnostic.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.
- If your request contradicts the existing code or a prior decision, flag the inconsistency before proceeding.
- Don't agree to an approach you think is wrong to avoid conflict — false agreement wastes more time than honest pushback.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 500 lines and it could be 100, rewrite it.
- Don't add comments explaining what the code does — well-named identifiers do that. Only comment the non-obvious why.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't change code you don't fully understand — you may break an invariant that isn't visible from the surface.
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan before starting:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

Verified means the code ran and produced the expected output — not "it looks right."

If you realize mid-task that your approach is wrong, stop and say so — don't finish it.

## Concrete Examples

See [EXAMPLES.md](../../EXAMPLES.md) for before/after code showing what each failure mode looks like and how to fix it.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
