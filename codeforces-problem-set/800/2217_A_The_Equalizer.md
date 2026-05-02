# 2217A - The Equalizer
---

**LINK:** https://codeforces.com/problemset/problem/2217/A

## Problem

2 Players alternate decrementing any positive arr[i] by 1; once per game Shaunak may instead set all value to k; given n,k and array a[n], determine if the first player can force a win

## My solution:

```c
#include <stdio.h>

void solve()
{
    int sum,n,k;
    scanf("%d %d", &n, &k);
    int a[n];

    sum = 0;
    for(int i = 0; i < n; i++)
    {
        scanf("%d",&a[i]);
        sum = sum+a[i];
    }

    if(sum%2 == 1) printf("YES\n");
    else if(sum%2 == 0 && (k * n)%2 == 0) printf("YES\n");
    else 
        printf("NO\n");
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

Logic: 
Result is YES if sum of elements is odd OR (n * k) is even, only parity matters (even/odd).

My mistake:
No need to store the array
compute sum on the fly while reading input instead of allocating a[n]

### Official solution (translated in C)

```c
#include <stdio.h>

int main() {
    int t;
    scanf("%d", &t);

    while (t--) {
        int n, k;
        scanf("%d %d", &n, &k);

        int ans = ((n * k) % 2 == 0); // n*k is even

        int tot = 0;
        for (int i = 0; i < n; i++) {
            int x;
            scanf("%d", &x);
            tot += x;
        }

        ans = ans || (tot % 2 == 1); // or sum is odd

        if (ans) printf("YES\n");
        else printf("NO\n");
    }

    return 0;
}
```