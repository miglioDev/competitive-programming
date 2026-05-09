# 1335A - Candies and Two Sisters
---

**LINK:** https://codeforces.com/problemset/problem/1335/A

## Problem

Count how many ways to split n candies into two positive integers where the older sister gets strictly more candies than the younger one

## My Attempt (Time limit exceeded):

```c
#include <stdio.h>
 
void solve()
{
    int a,b,n,tot;
    long long int ans = 0;
 
    scanf("%d",&n);
 
    if(n < 3) {
        printf("\n0");
        return; }
 
    else {
        tot = n;
        a = n;
        b = 0;
 
        while(n > tot/2) 
        {
            if( (a > 0) && (b > 0) && (a > b) ) ans++;
            a--;
            b++;
 
            n--;
        }
    }
 
    printf("\n%lld",ans);
}
 
 
int main ()
{  
    int t;
 
    scanf("%d",&t);
 
    while(t--)
        solve();
 
}
```
---

### Notes

My solution correctly models the idea of counting valid splits, but it simulates the process instead of using a direct mathematical observation so my linear simulation O(n) was unnecessary

The problem can be solved in O(1) by observing that for each valid split `n = a + b` with `a > b > 0`, the number of valid pairs depends only on how many values the smaller part `b` can take. This leads to a direct counting formula instead of iterating through all possibilities.

The answer is:
[
\frac{n - 1}{2}
]

