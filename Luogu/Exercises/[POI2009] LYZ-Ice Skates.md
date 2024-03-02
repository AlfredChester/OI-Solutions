摘要：线段树，最大子段和，Hall 定理

[传送门：https://www.luogu.com.cn/problem/P3488](https://www.luogu.com.cn/problem/P3488)

## 题意

见题目，不再赘述。

## 分析思路

对于修改，我们先不考虑如何去安排这些人具体穿哪个鞋码的鞋，我们都把他加到第 $x$ 个位置。

然后我们引入「均摊」的概念，即将 $[l, r]$ 内的所有鞋码的人数（记作 $v_i$）进行重新分配，使得 $[l, r]$ 中的每个 $v_i$ 都小于等于 $k$。可以发现其充要条件为 

$$\sum_{i = l}^r v_i \leq (r - l + 1 + d) * k$$

两边减去 $(r - l + 1) * k$ 得：

$$\sum_{i=l}^rv_i - k \leq d * k$$

由于 $d * k$ 为定值，我们只要找到 $v'_i = v_i - k$ 的最大连续子段和，判断其是否小于等于 $d * k$ 即可，使用线段树维护。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
typedef long long ll;
struct Node {
    int l, r;
    ll maxn, maxL, maxR, sum;
    inline void add(ll x) {
        sum += x, maxn = maxL = maxR = max(sum, 0ll);
    }
} tr[N << 2];
ll n, k, m, d, x, y;
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void maintain(int p) {
    auto &L = tr[ls(p)], &R = tr[rs(p)];
    tr[p].sum  = L.sum + R.sum;
    tr[p].maxL = max(L.maxL, L.sum + R.maxL);
    tr[p].maxR = max(R.maxR, R.sum + L.maxR);
    tr[p].maxn = max({L.maxn, R.maxn, L.maxR + R.maxL});
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r};
    if (l == r) {
        return tr[p].add(-k);
    } else {
        int mid = (l + r) >> 1;
        build(ls(p), l, mid);
        build(rs(p), mid + 1, r);
        maintain(p);
    }
}
inline void add(int p, int x, int v) {
    if (tr[p].l == tr[p].r) {
        return tr[p].add(v);
    }
    int mid = (tr[p].l + tr[p].r) >> 1;
    if (x <= mid) add(ls(p), x, v);
    if (x > mid) add(rs(p), x, v);
    maintain(p);
}
int main(int argc, char const *argv[]) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    cin >> n >> m >> k >> d;
    build(1, 1, n);
    while (m--) {
        cin >> x >> y, add(1, x, y);
        cout << (tr[1].maxn <= k * d ? "TAK" : "NIE") << '\n';
    }
    return 0;
}

```