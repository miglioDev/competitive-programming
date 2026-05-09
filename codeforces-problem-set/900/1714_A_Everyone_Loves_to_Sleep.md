# 1714A — Everyone Loves to Sleep

**Link:** https://codeforces.com/problemset/problem/1714/A  

## Problem

Given a wake-up time `H:M` and `n` alarms, find the **maximum sleep time** — i.e. the latest alarm before `H:M` (wrapping around midnight), and compute how long until `H:M` from that alarm.

## My Attempt (Accepted)

```c
#include <stdio.h>
void solve()
{
    int n,H,M;
    int h,m;  
    int minH = 0,minM = 0,tsleep,twake,tot,minTOT = 1440; 

    scanf("%d %d %d", &n, &H, &M);

    for(int i = 0; i < n; i++)
    {
        scanf("%d %d", &h, &m); 

        tsleep = (h * 60 + m);
        twake = (H * 60 + M);

        tot = (tsleep - twake +1440)%1440;

        if(tot < minTOT)
        {
            minTOT = tot;
            minM = minTOT%60;
            minH = minTOT/60; }
    }
    printf("%d %d\n",minH,minM);
}

int main ()
{
    int t;

    scanf("%d",&t);

    while(t--) 
    {
        solve();
    }
}
```

### Mistakes I made in previous attempts

No modular arithmetic:
Time wraps around midnight, subtracting hours/minutes separately cannot model the circular clock without a unified total.

Wrong output formatting

Notes:
first: `total = h * 60 + m`.
2. **Circular time = modular arithmetic.** `(a - b + 1440) % 1440` gives the forward distance from b to a on a 24h clock.

## Official Solution (translated from C++ to C)

```c
#include <stdio.h>

void solve()
{
    int n, H, M;
    scanf("%d %d %d", &n, &H, &M);

    int wake = H * 60 + M;
    int ans  = 24 * 60;   // worst case: full day

    for (int i = 0; i < n; i++)
    {
        int h, m;
        scanf("%d %d", &h, &m);

        int t = (h * 60 + m) - wake;
        if (t < 0) t += 24 * 60;   // wrap around midnight

        if (t < ans) ans = t;
    }

    printf("%d %d\n", ans / 60, ans % 60);
}

int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
        solve();
}
```

## Why this works

The core formula, think of one day as a circular 1440-minute clock:

Convert everything to minutes. Subtract alarm from wake-up. If negative, add 1440 to wrap around midnight. The minimum over all alarms is the answer.
