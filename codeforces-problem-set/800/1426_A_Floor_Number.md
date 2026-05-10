# 1426A - Floor Number
---

**LINK:** https://codeforces.com/problemset/problem/1426/A

## Problem

Find which floor apartment n is on, given that floor 1 has 2 apartments and all other floors have x apartments each

## My solution:

```c
#include <stdio.h>

void solve()
{
    int n,x, ans;

    scanf("%d %d",&n, &x);

    if(n <= 2) {
        printf("%d\n",1);
        return; }
    else {
    
    n = n - 2;
    ans = (n + x - 1 ) / x;
    ans++;

    printf("%d\n",ans); }
}


int main ()
{  
    int t;

    scanf("%d",&t);

    while(t--)
        solve();

}
```

My Logic: 
I dealt with the ground floor separately, and then realised that one solution was to divide n (the flat number) by the number of flats per floor, subtracting the two on the ground floor

### Official solution (translated in C)

```c
void solve()
{
    int n, x;

    scanf("%d %d", &n, &x);

    if(n <= 2)
        printf("1\n");
    else
        printf("%d\n", (n - 3) / x + 2);
}
```

---

### Notes

My solution is a standard competitive programming trick for integer ceiling division.

The official solution compresses the same idea into:

```c
(n - 3) / x + 2
```

which mathematically shifts the indexing starting from apartment 3, both solution are O(1).

---

