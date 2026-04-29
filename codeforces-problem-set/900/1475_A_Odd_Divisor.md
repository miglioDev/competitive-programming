# 1475A — Odd Divisor

**Link:** https://codeforces.com/problemset/problem/1475/A

## Problem

Given `n`, determine if it has at least one odd divisor greater than 1.

## My Attempt (TLE)

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    long long int n;
    int t;

    scanf("%d", &t);
    char s[t][64];

    for (int i = 0; i < t; i++)
        scanf("%s", s[i]);

    for (int i = 0; i < t; i++)
    {
        n = atoi(s[i]);   // BUG: atoi returns int overflows for n > ~2*10^9

        while (n % 2 == 0)
            n = n / 2;

        if (n == 1)      printf("NO\n");
        else if (n > 1)  printf("YES\n");
    }
}
```

### Why it failed

`atoi()` overflow: **Fix:** use `scanf("%lld", &n)` directly, no string conversion needed
Logic itself: Correct in theory: divide out all factors of 2, if remainder > 1 an odd divisor exists. 

## Official Solution (translated to C)

```c
#include <stdio.h>

void solve()
{
    long long int n;
    scanf("%lld", &n);

    if (n & (n - 1))
        printf("YES\n");
    else
        printf("NO\n");
}

int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
        solve();
}
```

### Key insight why `n & (n-1)`?

A number has **no** odd divisor > 1 iff it is a **power of 2**.

Powers of 2 in binary: `1000...0`  
`n - 1` flips all bits up to and including the lowest set bit: `0111...1`  
→ `n & (n-1) == 0` **only** for powers of 2.

If `n & (n-1) != 0`, n is not a power of 2, so it must have an odd prime factor → **YES**.

Complexity: **O(1)** per test case vs O(log n) for the divide-loop approach.

## Takeaways

1. **Never use `atoi` for competitive programming** Always `scanf("%lld", &n)` for `long long`.
2. **Bit tricks for powers of 2:** `n & (n-1) == 0` is a standard idiom i didn't know.
3. **When you get TLE on test 1**, suspect an infinite loop caused by wrong data (overflow, bad parsing), not just slow algorithm.

4. **Read** and **process** data at the same time.