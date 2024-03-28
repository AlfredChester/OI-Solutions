摘要：带修莫队

[传送门：https://www.luogu.com.cn/problem/CF940F](https://www.luogu.com.cn/problem/CF940F)

## 题意

给定长度为 $n$ 的序列 $a$ ($1 \leq a_i \leq 10^9$)，维护两种操作：

1. 单点修改；
2. 查询区间 $[l, r]$ 的数的**出现次数**的 `mex`。

不强制在线。注意由于 $0$ 的出现次数永远为 $0$，所以查询的答案永远不可能是 $0$。

## 分析思路

带修莫队板题。

首先对询问和原数组都进行离散化。

我们记录 $c_i$ 表示值为 $i$ 的数的个数，$cnt_i$ 表示 $c_j$ 为 $i$ 的 $j$ 的个数。容易在维护左右和时间指针的时候 $O\left(1\right)$ 维护这两个数组的修改。

于是问题转化为了求 $cnt_i$ 中第一个 $0$ 的位置。注意到 $cnt_i$ 中第一个 $0$ 的位置的上界为 $O\left(\sqrt n\right)$，所以暴力查找即可。

时间复杂度 $O\left(\frac{n^2}{\sqrt_{3} m}\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
template <class _Tp>
struct Mess {
    std::vector<_Tp> v;
    inline _Tp origin(int idx) { return v[idx - 1]; }
    inline void insert(_Tp x) { v.push_back(x); }
    inline void init(void) {
        sort(v.begin(), v.end());
        v.erase(unique(v.begin(), v.end()), v.end());
    }
    inline int query(_Tp x) {
        return lower_bound(v.begin(), v.end(), x) - v.begin() + 1;
    }
};
struct Modify {
    int pos, v;
} Mod[N];
struct Query {
    int l, r, t, idx;
};
Mess<int> M;
bitset<N> vis;
vector<Query> Q;
int n, m, q, t, opt, l, r, a[N], ans[N], c[N], cnt[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void addVal(int val) {
    if (cnt[val]++ == 0) vis[val] = 1;
}
inline void delVal(int val) {
    if (--cnt[val] == 0) vis[val] = 0;
}
inline void add(int pos) {
    delVal(c[a[pos]]), addVal(++c[a[pos]]);
}
inline void del(int pos) {
    delVal(c[a[pos]]), addVal(--c[a[pos]]);
}
inline void upd(int t, int l, int r) {
    if (l <= Mod[t].pos && Mod[t].pos <= r) {
        del(Mod[t].pos);
        swap(Mod[t].v, a[Mod[t].pos]);
        add(Mod[t].pos);
    } else {
        swap(Mod[t].v, a[Mod[t].pos]);
    }
}
inline int getMex(void) {
    int mex = 1;
    while (vis[mex]) mex++;
    return mex;
}
inline void MoAlgo(void) {
    int l = 1, r = 0, t = 0;
    for (int i = 0; i < q; i++) {
        while (Q[i].l < l) add(--l);
        while (r < Q[i].r) add(++r);
        while (l < Q[i].l) del(l++);
        while (Q[i].r < r) del(r--);
        while (t < Q[i].t) upd(++t, l, r);
        while (Q[i].t < t) upd(t--, l, r);
        ans[Q[i].idx] = getMex();
    }
}
inline void init(void) {
    const int B = n / pow(m, 1.0 / 3); // 莫队分块
    sort(Q.begin(), Q.end(), [=](Query &x, Query &y) {
        if (x.l / B != y.l / B) return x.l < y.l;
        if (x.r / B != y.r / B) return x.r < y.r;
        return (x.r / B) & 1 ? x.t < y.t : x.t > y.t;
    });
    for (int i = 1; i <= n; i++) a[i] = M.query(a[i]);
    for (int i = 1; i <= t; i++) Mod[i].v = M.query(Mod[i].v);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], M.insert(a[i]);
    }
    for (int i = 1; i <= m; i++) {
        cin >> opt >> l >> r;
        if (opt == 1) {
            Q.push_back({l, r, t, ++q});
        } else {
            M.insert(r), Mod[++t] = {l, r};
        }
    }
    M.init(), init(), MoAlgo();
    for (int i = 1; i <= q; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}

```
