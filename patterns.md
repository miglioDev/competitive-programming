# Patterns & Theory — Competitive Programming

> Reusable patterns, formulas, and reasoning frameworks.
> Update this every time something generalizes across problems.

## 0. Before Writing Any Code — Mental Checklist

1. **What are the constraints?** (`n ≤ ?`, value range, time limit)
2. **What exactly is being asked?** (min, max, count, yes/no, construction)
3. **Is the answer unique?** (if not, what flexibility do you have?)
4. **What happens at boundaries?** (`n = 1`, empty input, all equal values)
5. **Can the answer overflow `int`?**

## 1. Integer Division & Rounding

In C, `/` on integers always **truncates toward zero** (floors for positive numbers).
When you need to **round up**, use the ceiling division formula:

```c
// Ceiling of A divided by B (both positive integers)
long long ceil_div(long long a, long long b) {
    return (a + b - 1) / b;
}
```

**Why it works:** Adding `b - 1` to the numerator pushes any non-zero remainder over the next integer boundary, so truncation lands on the ceiling instead of the floor.

Example: `ceil(7 / 3)` = 3
- With formula: `(7 + 2) / 3 = 9 / 3` = **3** 
- Without: `7 / 3` = **2** ✗

## 2. Overflow — Always Think About It

In C, `int` holds up to ~2 × 10^9. A sum of `n ≤ 10^5` values up to `10^9` can reach `10^14` — this **overflows silently**.

**Rule of thumb:**
- If `n × max_value` can exceed 2 × 10^9, use `long long`.
- When multiplying two `int` values, cast first: `(long long)a * b`.

```c
long long total = 0;
for (int i = 0; i < n; i++) {
    total += a[i];  // safe: total is long long
}
```

## 3. Frequency Counting with Arrays

When elements are in a known range `[0, MAXVAL]`, use a plain array

```c
int freq[100001] = {0};

for (int i = 0; i < n; i++) {
    int x;
    scanf("%d", &x);
    freq[x]++;
}

if (freq[v] > 0) { /* v appeared */ }
```

**When NOT to use this:** values are negative, very large (> 10^7), or very sparse.

---

## 4. Prefix Sums — Range Queries in O(1)

**Problem type:** sum of elements from index `l` to `r`, answered many times.

Naïve: O(n) per query. Prefix sum: O(n) build, O(1) per query.

```c
int a[N], prefix[N + 1];
prefix[0] = 0;
for (int i = 0; i < n; i++)
    prefix[i + 1] = prefix[i] + a[i];

// Sum from index l to r (0-indexed, inclusive)
int range_sum = prefix[r + 1] - prefix[l];
```

**Why it works:** `prefix[i]` = sum of all elements before index `i`. Subtracting `prefix[l]` cancels everything before `l`.

**Common mistake:** off-by-one. Always build a 1-indexed prefix array starting with `prefix[0] = 0`.

---

## 5. Grid & Distance

When moving on a 2D grid, the right distance formula depends on allowed directions:

| Movement | Metric | Formula |
|---|---|---|
| 4 directions (up/down/left/right) | **Manhattan** | abs(r1-r2) + abs(c1-c2) |
| 8 directions (+ diagonals) | **Chebyshev** | max(abs(r1-r2), abs(c1-c2)) |

```c
int manhattan(int r1, int c1, int r2, int c2) {
    return abs(r1 - r2) + abs(c1 - c2);
}

int chebyshev(int r1, int c1, int r2, int c2) {
    int dr = abs(r1 - r2), dc = abs(c1 - c2);
    return dr > dc ? dr : dc;
}
```

### Direction delta pattern

Instead of 4 separate if-statements, use offset arrays:
current cell (r, c);
```c
int dr[] = {-1, 1, 0, 0};   // up, down, left, right
int dc[] = { 0, 0,-1, 1};

for (int d = 0; d < 4; d++) {
    int nr = r + dr[d];  // (neighbor row)
    int nc = c + dc[d];  // (neighbor col)
    if (nr >= 0 && nr < rows && nc >= 0 && nc < cols) {
        // valid neighbor (nr, nc)
    }
}
```
dr[] = how far you move along the rows
dc[] = how far you move along the col

For 8 directions, extend to 8 entries. **Always check bounds before accessing** out-of-range access is undefined behavior in C (no crash, just wrong results).

---

## 6. Greedy — Sorting as a First Step

Many greedy problems become easy once you sort the input
Ask: *what property does sorting expose?*

- Sort by value → minimum/maximum cost assignments
- Sort by deadline → scheduling problems
- Sort by first element → interval processing

**Template:**
1. Read all items
2. Sort by the relevant key
3. Scan once and make local greedy choices

**When it's safe:** when a locally optimal choice never makes a future choice worse.
