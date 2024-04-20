摘要：线段树，势能分析

[传送门：https://www.luogu.com.cn/problem/CF121E](https://www.luogu.com.cn/problem/CF121E)

## 题意

定义十进制下数位中只有 `4` 和 `7` 的自然数为幸运数字。给定一个长度为 $n$ 的序列 $a$，维护以下两个操作：

1. `add l r d` 把第 $l$ 个数到第 $r$ 个数都加上 $d$；
2. `count l r` 统计第 $l$ 个到第 $r$ 个数中有多少个幸运数字。

**保证所有数操作前后都不超过 $10^4$。**

## 分析思路

注意到题目提供的特殊性质，打表可以知道 $10^4$ 以内的幸运数字只有 $30$ 个。考虑进行一些暴力操作。

具体来说，我们维护一段区间内的每个数，到第一个比他大的幸运数的距离（注意这个幸运数可能为 $44444$）的最小值，以及这些最小值的个数。每个操作 $1$ 相当于进行区间加法。

如果当前区间内的最小值变为了负数，则进行暴力重构。对于每个数只会暴力重构最多 $30$ 次，时间复杂度 $O\left(nk\log n\right)$，$k$ 为常数 $30$，可以承受。

每个操作 $2$ 相当于统计区间内 $0$ 的个数，一个区间产生贡献当且仅当这个区间内的「最小值」为 $0$，并且产生最小值个数的贡献。

总时间复杂度 $O\left(nk\log n\right)$，$k$ 为常数 $30$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
struct Node {
    int l, r, lazy, mn, sum;
    inline int mid(void) { return (l + r) >> 1; }
    inline void subtract(int x) {
        mn -= x, lazy += x;
    }
} tr[N << 2];
string opt;
vector<int> Lucky;
using PII = pair<int, int>;
int n, m, l, r, v, a[N], nr[N];
inline int ls(int x) { return x << 1; }
inline int rs(int x) { return x << 1 | 1; }
inline void maintain(int p) {
    tr[p].sum = 0, tr[p].mn = min(tr[ls(p)].mn, tr[rs(p)].mn);
    if (tr[p].mn == tr[ls(p)].mn) tr[p].sum += tr[ls(p)].sum;
    if (tr[p].mn == tr[rs(p)].mn) tr[p].sum += tr[rs(p)].sum;
}
inline void remake(int p) {
    if (tr[p].mn >= 0) return;
    if (tr[p].l == tr[p].r) {
        a[tr[p].l] += tr[p].lazy, tr[p].lazy = 0;
        tr[p].mn = nr[a[tr[p].l]] - a[tr[p].l];
        return;
    }
    tr[ls(p)].subtract(tr[p].lazy);
    tr[rs(p)].subtract(tr[p].lazy), tr[p].lazy = 0;
    remake(ls(p)), remake(rs(p)), maintain(p);
}
inline void subtract(int p, int v) {
    tr[p].subtract(v);
    if (tr[p].mn < 0) remake(p);
}
inline void pushdown(int p) {
    if (tr[p].lazy) {
        subtract(ls(p), tr[p].lazy);
        subtract(rs(p), tr[p].lazy), tr[p].lazy = 0;
    }
}
inline void build(int p, int l, int r) {
    if (l == r) {
        tr[p] = {l, r, 0, nr[a[l]] - a[l], 1};
        return;
    }
    int m = (l + r) >> 1;
    build(ls(p), l, m), build(rs(p), m + 1, r);
    tr[p] = Node{l, r}, maintain(p);
}
inline void modify(int p, int l, int r, int v) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return subtract(p, v);
    }
    pushdown(p);
    if (l <= tr[p].mid()) modify(ls(p), l, r, v);
    if (r > tr[p].mid()) modify(rs(p), l, r, v);
    maintain(p);
}
inline PII merge(PII a, PII b) {
    if (a.first > b.first) return b;
    if (a.first < b.first) return a;
    return {a.first, a.second + b.second};
}
inline PII query(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return {tr[p].mn, tr[p].sum};
    }
    pushdown(p);
    PII ans = {INT_MAX, 0};
    if (l <= tr[p].mid()) ans = merge(ans, query(ls(p), l, r));
    if (r > tr[p].mid()) ans = merge(ans, query(rs(p), l, r));
    return maintain(p), ans;
}
inline bool isLucky(int x) {
    while (x) {
        if (x % 10 != 4 && x % 10 != 7) {
            return false;
        }
        x /= 10;
    }
    return true;
}
inline void init(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    for (int i = 1; i <= 1e4; i++) {
        if (isLucky(i)) Lucky.push_back(i);
    }
    Lucky.push_back(44444);
    int x = Lucky.size() - 1;
    for (int i = 1e4; i >= 0; i--) {
        if (x > 0 && Lucky[x - 1] == i) x--;
        nr[i] = Lucky[x];
    }
}
int main(int argc, char const *argv[]) {
    init(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    build(1, 1, n);
    while (m--) {
        cin >> opt >> l >> r;
        if (opt == "add") {
            cin >> v, modify(1, l, r, v);
        } else {
            PII res = query(1, l, r);
            cout << (res.first ? 0 : res.second) << '\n';
        }
    }
    return 0;
}

```
