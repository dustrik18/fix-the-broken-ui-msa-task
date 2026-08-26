# Standup Tracker — Fix This Broken UI (MLSA SRM Recruitment Task)

A small React app for tracking a team's daily standup status. It shipped with four bugs planted on purpose. This repo contains my fixes for all four.

## What I fixed

1. **Stale closure (ticker stuck at 1s)** — The `setInterval` callback inside `useEffect` was closing over the initial value of `secondsSinceUpdate` (always `0`) because the effect only ran once. Fixed by using the functional update form `setSecondsSinceUpdate(s => s + 1)`, so it always reads the current value instead of a stale one.

2. **Missing list key** — The `<li>` for each team member had no `key` prop, so React tracked rows by position instead of identity. This could cause the wrong status/name to appear on the wrong row after deleting someone. Fixed by adding `key={member.id}`.

3. **Layout bug on narrow screens** — `.header-bar` had a hardcoded `width: 600px` and `white-space: nowrap`, which made the header overflow on small screens. Removed both so the flex layout can shrink naturally.

4. **Accessibility issue** — The remove button was a `<div onClick={...}>`, which isn't keyboard-focusable and isn't announced properly by screen readers. Replaced it with a real `<button>` and added `aria-label={\`Remove ${member.name}\`}`.

Each fix has a one-line comment in the code explaining what was actually wrong.

## Running locally

```
npm install
npm run dev
```

Requires Node 18 or newer.

## What I'd do differently with more time

Finding the four bugs wasn't too hard, but *understanding* a few of them properly took real time — this was my first time working through someone else's React codebase rather than my own, and there was a lot to trace through as a newcomer (especially the closure timing issue). With more time I'd want to dig deeper into how `useEffect` dependency arrays interact with closures in general, rather than just applying the functional-update fix pattern-matched to this bug.

---
*This task was a part of the MLSA SRM recruitment process.*
