摘要：线段树，最近公共祖先（LCA），欧拉筛，欧拉函数，数论

[传送门：https://www.luogu.com.cn/problem/CF1797E](https://www.luogu.com.cn/problem/CF1797E)

纪念一下，自己一眼瞪出来的紫题。个人觉得这题还是很好的。

## 题意

给定一个长度为 $1\le n\le 10^5$ 的数列，$1\le a_i\le 5\times 10^6$。

有 $m\le 10^5$ 次操作 `o l r`，

1. $o=1$ 时， $\forall\ l\le i\le r$，$a_i\gets\varphi(a_i)$；
2. $o=2$ 时，找出使 $a_l=a_{l+1}\dots=a_r$ 的最小变化。在每次变化中，选择一个 $x\in[l,r],a_i\gets \varphi(a_i)$。该操作不会修改数列。

对于每个操作 $2$ 输出最小答案。

## 分析思路

注意到，对于一个 $x$ 一定有一个唯一对应的 $\varphi(x)$。 ~~这不废话~~ 

于是我们可以线性筛出 $\varphi$ 的值，连接 $x \to \varphi(x)$ 构造出一颗「$\varphi$ 树」。注意到操作 $2$ 最终变成的值就是在 $\varphi$ 树上 $a_l, a_{l + 1}, \cdots, a_r$ 的 `LCA`。由于 `LCA` 是可加信息，所以考虑使用线段树进行维护。

然而，我们没有办法使用懒标记的形式，快速支持操作 $1$。这里使用线段树势能分析中一个经典的技巧，注意到 $a_i\gets\varphi(a_i)$ 最多能操作的次数为 $\log a_i$ 级别（读者自证不难），该问题中序列的势能也不会增加，所以可以类似 `花神游历各国 2` 进行暴力修改。

$1$ 为 $\varphi(x)$ 的一个不动点。如果一个区间已经全部为 $1$，则无须更改。至此我们完成了操作 $1$。

对于操作 $2$ 我们显式建立出 $\varphi$ 树，预处理 `LCA` 和深度的信息。操作 $2$ 的答案即为 $\sum_{i=l}^r d_{lca} - d_i = (r - l + 1) \times d_{lca} - \sum_{i=l}^r d_i$，所以我们再维护一下区间在 $\varphi$ 树上的深度和，即可得到答案。

总时间复杂度 $O\left(((n + m) \log n + V) \log \log V\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
const int M = 5000001;
struct Node {
    // mx: the node with the maximum depth.
    int l, r, lca, sum, mx;
    inline int mid(void) { return (l + r) >> 1; }
} tr[N << 2];
bitset<M> vis;
vector<int> G[M];
int n, m, mx, a[N], phi[M];
int opt, l, r, d[M], lg[M], f[M][6]; 
// 这里 f 开 6 是因为打表得到 d[i] 最大为 24，ceil(log(24)) = 6
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void sieve(int x) {
    vector<int> primes;
    for (int i = 2; i <= x; i++) {
        if (!vis[i]) primes.push_back(i), phi[i] = i - 1;
        for (int j = 0; i * primes[j] <= x; j++) {
            vis[i * primes[j]] = true;
            phi[i * primes[j]] = phi[i] * phi[primes[j]];
            if (i % primes[j] == 0) {
                phi[i * primes[j]] = phi[i] * primes[j];
                break;
            }
        }
    }
}
inline void dfs(int x, int fa) {
    d[x] = d[fa] + 1, f[x][0] = fa;
    for (int i = 1; (1 << i) <= d[x]; i++) {
        f[x][i] = f[f[x][i - 1]][i - 1];
    }
    for (auto y : G[x]) {
        if (y != fa) dfs(y, x);
    }
}
inline void buildTree(int x) {
    phi[1] = 1, sieve(x), lg[0] = -1;
    for (int i = 1; i <= x; i++) {
        G[phi[i]].push_back(i);
        lg[i] = lg[i >> 1] + 1;
    }
    dfs(1, 1);
}
inline int LCA(int u, int v) {
    if (d[u] < d[v]) swap(u, v);
    while (d[u] > d[v]) {
        u = f[u][lg[d[u] - d[v]]];
    }
    if (u == v) return u;
    for (int k = lg[d[u]]; k >= 0; k--) {
        if (f[u][k] != f[v][k]) {
            u = f[u][k], v = f[v][k];
        }
    }
    return f[u][0];
}
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void maintain(int p) {
    tr[p].sum = tr[ls(p)].sum + tr[rs(p)].sum;
    tr[p].lca = LCA(tr[ls(p)].lca, tr[rs(p)].lca);
    tr[p].mx  = d[tr[ls(p)].mx] > d[tr[rs(p)].mx] ? tr[ls(p)].mx : tr[rs(p)].mx;
}
inline void build(int p, int l, int r) {
    if (l == r) {
        tr[p] = {l, r, a[l], d[a[l]], a[l]};
    } else {
        int m = (l + r) >> 1;
        build(ls(p), l, m), build(rs(p), m + 1, r);
        tr[p] = Node{l, r}, maintain(p);
    }
}
inline void modify(int p, int l, int r) {
    if (tr[p].mx == 1) return;
    if (tr[p].l == tr[p].r) {
        const int x = tr[p].l;
        a[x] = phi[a[x]], tr[p] = {x, x, a[x], d[a[x]], a[x]};
        return;
    }
    int m = tr[p].mid();
    if (l <= m) modify(ls(p), l, r);
    if (r > m) modify(rs(p), l, r);
    maintain(p);
}
using PII = pair<int, int>;
inline PII merge(PII l, PII r) {
    return {LCA(l.first, r.first), l.second + r.second};
}
inline PII query(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return {tr[p].lca, tr[p].sum};
    }
    int m = tr[p].mid();
    if (l <= m && !(r > m)) {
        return query(ls(p), l, r);
    } else if (!(l <= m) && r > m) {
        return query(rs(p), l, r);
    } else {
        return merge(query(ls(p), l, r), query(rs(p), l, r));
    }
}
int main(int argc, const char *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], mx = max(mx, a[i]);
    }
    buildTree(mx), build(1, 1, n);
    while (m--) {
        cin >> opt >> l >> r;
        if (opt == 1) modify(1, l, r);
        if (opt == 2) {
            auto res = query(1, l, r);
            cout << res.second - (r - l + 1) * d[res.first] << '\n';
        }
    }
    return 0;
}

```
