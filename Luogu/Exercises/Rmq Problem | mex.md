摘要：莫队，值域分块

[传送门：https://www.luogu.com.cn/problem/P4137](https://www.luogu.com.cn/problem/P4137)

## 题意

给定一个长度为 $n$ 的静态序列 $a$，给定 $l, r$，求  $\mathrm{mex}(a_l, a_{l + 1}, \cdots, a_r)$。

注：本题解讨论的数据范围与洛谷题面稍有不同，在于 $0 \leq a_i \leq 10^9$。

## 分析思路

考虑莫队。注意到长度为 $k$ 的数列的 $\mathrm{mex}$ 最大为 $k$，于是我们可以维护一个可能成为 $\mathrm{mex}$ 的有序集合，每次取首项即可。

一个容易想到的方法是使用 `std::set` 维护，然而这样做的话时间复杂度为 $O(n \sqrt n \log n)$，不足以考虑此题，考虑优化。

我们吸取 [One Occurrence](https://www.luogu.com.cn/article/2nf4s5sx) 一题的经验。考虑能否快速修改而在可以接受的复杂度内得到新的答案。

我们记 $vis_i$ 表示数字 $i$ 现在是否**不能**作为 $\mathrm{mex}$。搭配 $cnt_i$ 表示数字 $i$ 的出现次数，可以轻易地 $O(1)$ 进行维护。这样问题就转换为了寻找在 $vis$ 数组中第一个 $0$ 的位置。权值线段树可以胜任这项工作，~~但是我太弱了不会~~。

所以我们考虑使用值域分块。我们记录每个块当中 $1$ 的个数。如果当前块中 $1$ 的个数等于块长，则跳过；否则，在这个块里面细找一波，就一定能找到第一个 $0$。

这样得到答案的时间复杂度就变为 $O(\sqrt n)$ 了。由于得到答案与移动指针是并行的，整体的时间复杂度则为 $O(n \sqrt n)$，可以通过本题。

由于 $0 \leq a_i \leq 10^9$，我们对 $a$ 进行离散化，注意放入 $a_i$ 和 $a_i + 1$。如果离散化后的结果是从 $1$ 开始的，则一开始需要在值域分块上标记位置 $0$ 为 $1$，然后记得输出原数！！！

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
struct Query {
    int l, r, idx;
} Q[N];
Mess<int> M;
bitset<N> vis;
int a[N], c[N], sum[450], cnt[N];
int n, m, B, l, r, mx, bel[N], ans[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
// 值域分块维护第一个0的位置
inline void modify(int pos, int v) { // set vis[pos] = v
    sum[bel[pos]] -= vis[pos];
    vis[pos] = v;
    sum[bel[pos]] += vis[pos];
}
inline int len(int x) { // the length of xth block
    return x == bel[mx] ? mx - bel[mx] * B : B;
}
inline int get(void) { // get the first zero
    for (int i = 0; i <= bel[mx]; i++) {
        if (len(i) == sum[i]) continue;
        for (int j = i * B; bel[j] == i; j++) {
            if (vis[j] == 0) return j;
        }
    }
    return INT_MAX; // ain't gonna happen
}
inline void add(int x) {
    if (cnt[a[x]]++ == 0) modify(a[x], 1);
}
inline void del(int x) {
    if (--cnt[a[x]] == 0) modify(a[x], 0);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], M.insert(a[i]), M.insert(a[i] + 1);
    }
    M.init();
    for (int i = 1; i <= n; i++) {
        mx = max(mx, a[i] = M.query(a[i]));
    }
    mx += 2, B = sqrt(mx);
    for (int i = 0; i <= mx; i++) bel[i] = i / B;
    for (int i = 1; i <= m; i++) {
        cin >> l >> r, Q[i] = {l, r, i};
    }
    B = sqrt(n);
    sort(Q + 1, Q + 1 + m, [=](Query &x, Query &y) {
        if (x.l / B != y.l / B) return x.l < y.l;
        return (x.l / B) & 1 ? x.r > y.r : x.r < y.r;
    });
    l = 1, r = 0, B = sqrt(mx), modify(0, 1);
    for (int i = 1; i <= m; i++) {
        while (Q[i].l < l) add(--l);
        while (r < Q[i].r) add(++r);
        while (l < Q[i].l) del(l++);
        while (Q[i].r < r) del(r--);
        ans[Q[i].idx] = M.origin(get());
    }
    for (int i = 1; i <= m; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}

```