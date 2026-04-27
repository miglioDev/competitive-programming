# Mistakes Log — Competitive Programming

> Every entry here is a lesson
> Pattern: identify the error → understand *why* it happened → write the fix.

## Entry Template

```
### [Problem] XXXX - Problem Name (Rating: 800)
**Date:** YYYY-MM-DD
**Mistake type:** [Wrong Answer / Off-by-one / Overflow / Wrong formula / Edge case / Logic error]

**What I did wrong:**
Describe in plain English what your code was doing wrong. Be specific.

**Why it was wrong:**
Explain the underlying reason — not just "I forgot", but *what assumption was broken*.

**The fix:**
Show the corrected code snippet (just the relevant part).

**Generalized lesson:**
What rule or check should you apply to avoid this class of mistake in the future?
```

---

## Log

### [Example] Template Entry — Ceiling Division
**Date:** 2026-04-27
**Mistake type:** Wrong Answer — wrong formula
**Link:** *(example, not a real problem)*

**What I did wrong:**
I computed `a / b` to get the number of groups needed, but this gives the floor (e.g., 7 items in groups of 3 gives `7/3 = 2`, but you need 3 groups).

**Why it was wrong:**
Integer division in C truncates toward zero. I assumed it rounded to the nearest integer, but it always rounds *down* for positive values.

**The fix:**
```cpp
// Instead of:
int groups = a / b;

// Use:
int groups = (a + b - 1) / b;
```

**Generalized lesson:**
Whenever computing "how many X fit into Y" where partial groups still count, use ceiling division. Ask: *does a remainder require an extra unit?* If yes, use `(a + b - 1) / b`.

---

## Quick Reference — One-line Summaries

| # | Problem | Mistake | Fix |
|---|---------|---------|-----|
| 1 | *(example)* | Floor instead of ceiling division | `(a + b - 1) / b` |

---

*Add new entries at the top of the Log section so the most recent mistakes are easiest to find.*
