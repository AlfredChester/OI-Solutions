摘要：线段树，区间最值操作

从本题开始，本蒟蒻每道蓝及以上难度的题都会写一篇题解，以确保真正理解了每道题。

>
> 一切伟大的行动和思想，都有一个微不足道的开始。
> 

[传送门: https://www.luogu.com.cn/problem/P4560](https://www.luogu.com.cn/problem/P4560)

## 题意

给定一个初始全为 $0$ 的序列 $A_n$，现在要维护两个操作：

1. $\forall i \in [l, r], A_i \leftarrow \max(h, A_i)$
2. $\forall i \in [l, r], A_i \leftarrow \min(h, A_i)$

求做出这些操作之后的序列 ${A_n}'$。

## 分析思路

如果是随机数据，很容易想到可以使用珂朵莉树来维护区间信息。然而很不幸这题并不是随机数据，珂朵莉树仅能得到 $O(n^2)$ 的部分分 $8$pts。[附惨烈提交记录](https://www.luogu.com.cn/record/146738366)。

其实这题不复杂，用线段树维护区间最大和最小值，叶子结点的值即为要查询的序列。实际上一开始想到线段树的时候考虑复杂了，还是说明基本功不够扎实。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2000010;
struct Node {
    int l, r, mx, mn;
    inline int mid(void) { return (l + r) >> 1; }
    inline void setLB(int x) {
        mx = max(mx, x), mn = max(mn, x);
    }
    inline void setRB(int x) {
        mx = min(mx, x), mn = min(mn, x);
    }
} tr[N << 2];
int n, k, opt, l, r, v;
inline int ls(int x) { return x << 1; }
inline int rs(int x) { return x << 1 | 1; }
inline void pushdown(int p) {
    tr[ls(p)].setLB(tr[p].mn), tr[ls(p)].setRB(tr[p].mx);
    tr[rs(p)].setLB(tr[p].mn), tr[rs(p)].setRB(tr[p].mx);
    tr[p].mx = INT_MAX, tr[p].mn = 0;
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r, INT_MAX, 0};
    if (l != r) {
        int mid = (l + r) >> 1;
        build(ls(p), l, mid), build(rs(p), mid + 1, r);
    }
}
inline void modify1(int p, int l, int r, int v) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].setLB(v);
    }
    pushdown(p);
    int mid = tr[p].mid();
    if (l <= mid) modify1(ls(p), l, r, v);
    if (r > mid) modify1(rs(p), l, r, v);
}
inline void modify2(int p, int l, int r, int v) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].setRB(v);
    }
    pushdown(p);
    int mid = tr[p].mid();
    if (l <= mid) modify2(ls(p), l, r, v);
    if (r > mid) modify2(rs(p), l, r, v);
}
inline void query(int p) {
    if (tr[p].l == tr[p].r) {
        cout << tr[p].mn << '\n';
        return;
    }
    pushdown(p), query(ls(p)), query(rs(p));
}
int main(int argc, char const *argv[]) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    cin >> n >> k, build(1, 0, n - 1);
    while (k--) {
        cin >> opt >> l >> r >> v;
        if (opt == 1) modify1(1, l, r, v);
        if (opt == 2) modify2(1, l, r, v);
    }
    query(1);
    return 0;
}

```