# 1860A — Not a Substring
---

**Link:** https://codeforces.com/problemset/problem/1860/A  

## Problem

Given a balanced bracket sequence `s` of length `n`, construct **any** balanced bracket sequence of length `2n` that does **not** contain `s` as a substring. If impossible, print `NO`.


##My Attempt 
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

void solve()
{
    int i,flag = 0;
    char s[50];

    scanf("%s",s);
    int len = strlen(s);
    char t[len*2 +1];

    if(!flag)
    {
        for(i = 0; i < len; i++)
        {
            t[i] = '(';
        }
        for(; i < (len*2); i++)
        {
            t[i] = ')';
        }
        t[len*2] = '\0';
        if(strstr(t,s) == NULL) flag = 1;
    }

    if(!flag)
    {
        for(i = 0; i < len*2; i++)
        {
            if(i%2 == 0) {
                t[i] = '('; }

                else 
                    t[i] = ')';
        }
        t[len*2] = '\0';
        if(strstr(t,s) == NULL) flag = 1;
    }

    if(flag) {
        printf("YES\n");
        printf("%s\n",t); }
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

## Logic 

we only need to check **two** specific sequences of length `2n`. If `s` is a substring of both, the answer is `NO`. Otherwise one of them always works.

The two candidates:

| Name | Pattern | Example (n=3, len=6) |
|---|---|---|
| Alternating | `()()()...` | `()()()` |
| Grouped | `(((...)))` | `((()))` |

---

Bitwise shortcut was possible in both.

I should have generated both strings in a single loop and printed  when the decision yes/no was made. 


## Official Solution (translated to C)

```c
#include <stdio.h>
#include <string.h>

void solve()
{
    char s[55];
    scanf("%s", s);
    int n = strlen(s);

    char a[105], b[105];

    for (int i = 0; i < 2 * n; i++)
    {
        a[i] = (i % 2) ? ')' : '(';       // alternating:  ()()()
        b[i] = (i < n) ? '(' : ')';       // grouped:      ((()))
    }
    a[2 * n] = '\0';
    b[2 * n] = '\0';

    if (strstr(a, s) == NULL)
        printf("YES\n%s\n", a);
    else if (strstr(b, s) == NULL)
        printf("YES\n%s\n", b);
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
