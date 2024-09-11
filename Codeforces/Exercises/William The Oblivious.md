摘要：线段树，动态分治

[传送门：https://www.luogu.com.cn/problem/CF1609E](https://www.luogu.com.cn/problem/CF1609E)

## 题意

给定一个只有 a、b、c 三个字母的字符串，每次修改其中的一个字符。修改之后，需要求出至少修改多少个字母（也要修改成 a、b、c 三个字母中的一个），才能使原串不包含**子序列** abc。

## 分析思路

这种带修的最优化问题无非两种：一种是动态 dp，一种是动态分治。本题解主要介绍本题的动态分治做法。

考虑如何分治地让两边的和区间没有子序列 abc。一种是左边把所有的 abc 子串全部删除。此时左边的最大可能匹配前缀是 ab，所以再把右区间的所有 c 删除；一种对称地，把左区间所有的 a 和右区间所有的 abc 删除；最后一种是把左边的 ab 子串删除，右边的 bc 删除，思考路径显然。只删除 a、b 或 c 的代价是显然自封闭的。考虑还需要让 ab、bc 封闭，转移也是显然的。

于是我们可以使用线段树动态维护分治的结果，这题就做完了。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
struct Info {
    int a, b, c, ab, bc, abc;
    inline void set(char C) {
        if (C == 'a') {
            a = 1, b = 0, c = 0;
        } else if (C == 'b') {
            a = 0, b = 1, c = 0;
        } else {
            a = 0, b = 0, c = 1;
        }
    }
};
struct Node {
    int l, r;
    Info info;
    inline int mid(void) {
        return (l + r) >> 1;
    }
} tr[N << 2];
char c;
string s;
int n, q, x;
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void merge(Info &res, Info &L, Info &R) {
    res.a = L.a + R.a, res.b = L.b + R.b, res.c = L.c + R.c;
    res.ab = min(L.ab + R.b, L.a + R.ab);
    res.bc = min(L.bc + R.c, L.b + R.bc);
    res.abc = min({L.abc + R.c, L.a + R.abc, L.ab + R.bc});
    dbg(res, L, R, min(L.ab + R.b, L.a + R.ab));
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r};
    if (l == r) {
        return tr[p].info.set(s[l]);
    } else {
        int m = (l + r) >> 1;
        build(ls(p), l, m), build(rs(p), m + 1, r);
        merge(tr[p].info, tr[ls(p)].info, tr[rs(p)].info);
    }
}
inline void modify(int p, int x, char c) {
    if (tr[p].l == tr[p].r) {
        return tr[p].info.set(c);
    }
    modify(x <= tr[p].mid() ? ls(p) : rs(p), x, c);
    merge(tr[p].info, tr[ls(p)].info, tr[rs(p)].info);
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> q >> s;
    s = ' ' + s, build(1, 1, n);
    while (q--) {
        cin >> x >> c, modify(1, x, c);
        cout << tr[1].info.abc << '\n';
    }
    return 0;
}

```
