摘要：莫队，值域分块

[传送门：https://loj.ac/p/6164](https://loj.ac/p/6164)

## 题意

给定长度为 $n$ 的数列 $a$ 以及 $m$ 次询问，每次给定三个参数 $l, r, k$，求出 $[l, r]$ 范围中有多少个数的出现次数与 $k$ 互质。

## 分析思路

由于 $k$ 不固定，我们无法在移动的时候直接在可以接受的复杂度内更新答案，考虑在 $O(\sqrt n)$ 的时间复杂度内得到答案。

我们记 $cnt_i$ 表示数字 $i$ 的出现次数，$c_j$ 表示 $cnt_i = j$ 的 $i$ 的个数，这些都可以在移动的时候 $O(1)$ 更新。我们把 $cnt$ 数组分成两部分：

- 第一部分是 $cnt_i \geq \sqrt n$ 的，满足该条件的 $i$ 的**个数**不超过 $\sqrt n$ 个，我们可以暴力枚举 $i$，并且判断 $cnt_i$ 是否与 $k$ 互质，并且将答案加上 $1$。
- 第二部分是 $cnt_i < \sqrt n$ 的，满足该条件的 $cnt_i$ 的个数不超过 $\sqrt n$ 个，我们可以暴力枚举 $cnt_i$，判断 $cnt_i$ 是否与 $k$ 互质，并且将答案加上 $c_{cnt_i}$。

这两个部分的查询时间复杂度都不超过 $O(\sqrt n)$，为了维护这两个部分，我们可以使用 `std::set`。总时间复杂度大约为 $O(n \sqrt n \log n)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 50010;
struct Query {
    int l, r, k, idx;
} Q[N];
set<int> S;
int n, m, l, r, k, a[N];
int B, ans[N], cnt[N], c[N];
inline int gcd(int x, int y) {
    return !y ? x : gcd(y, x % y);
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void add(int x) {
    c[cnt[a[x]]]--, cnt[a[x]]++;
    if (cnt[a[x]] == B) S.insert(a[x]);
    c[cnt[a[x]]]++;
}
inline void del(int x) {
    c[cnt[a[x]]]--;
    if (cnt[a[x]] == B) S.erase(a[x]);
    cnt[a[x]]--, c[cnt[a[x]]]++;
}
inline int calc(int k) {
    int sum = 0;
    for (int i = 1; i < B; i++) {
        if (c[i] == 0) continue;
        if (gcd(i, k) == 1) sum += c[i];
    }
    for (auto item : S) {
        if (gcd(cnt[item], k) == 1) ++sum;
    }
    return sum;
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m, B = sqrt(n);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i <= m; i++) {
        cin >> l >> r >> k, Q[i] = {l, r, k, i};
    }
    sort(Q + 1, Q + 1 + m, [=](Query &x, Query &y) {
        if (x.l / B != y.l / B) return x.l < y.l;
        return (x.l / B) & 1 ? x.r < y.r : x.r > y.r;
    });
    l = 1, r = 0;
    for (int i = 1; i <= m; i++) {
        while (Q[i].l < l) add(--l);
        while (r < Q[i].r) add(++r);
        while (l < Q[i].l) del(l++);
        while (Q[i].r < r) del(r--);
        ans[Q[i].idx] = calc(Q[i].k);
    }
    for (int i = 1; i <= m; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}

```