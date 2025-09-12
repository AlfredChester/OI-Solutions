摘要：贪心、树链剖分

[传送门：https://www.luogu.com.cn/problem/P10731](https://www.luogu.com.cn/problem/P10731)

## 题意

有一棵树，你可以标记若干多个点。现在给定一些路径，要求这些路径上至少有一个点被覆盖。求最少要覆盖多少点并且构造方案。

## 分析思路

楼上稍微有点感性啊，补充一手证明。

**引理 1：** 对于一条没有被删除的路径，删除其两个端点的最近公共祖先一定不劣。

证明：我们知道两条路径 $a, b$ 有交的充要条件是 $a$ 的 LCA 在 $b$ 上或者 $b$ 的 LCA 在 $a$ 上。如果删除 $b$ 上的某个节点能够删掉 $a$，则 $b$ 与 $a$ 一定有交，则删除 $b$ 的 LCA 或者 $a$ 的 LCA 都能删掉两条路径。

**引理 2：** 对于所有还没删除的路径，LCA 最深的路径删除其 LCA 一定不劣。

证明：由 LCA 的深度在整条路径上是最短的，知道没有其他路径的 LCA 在这条路径上。如果删除其他路径的点能删除它，则这条路径与其他路径有交，删除这条路径的 LCA 也能删掉所有与它有交的路径。

所以我们确定了一个贪心策略：每次删除 LCA 最深的还没有被删除路径的 LCA。使用树链剖分维护路径，复杂度 $O((n + m) \log^2 n)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
struct Node {
    int l, r;
    bool ok;
    inline int mid(void) {
        return (l + r) >> 1;
    }
} tr[N << 2];
struct HLD {
    int fa, dep, siz, hson, top, id;
} H[N];
vector<int> G[N];
int n, m, u, v, cnt, a[N], b[N], L[N];
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void dfs1(int x, int fa) {
    H[x] = {fa, H[fa].dep + 1, 1};
    for (auto &y : G[x]) {
        if (y == fa) continue;
        dfs1(y, x), H[x].siz += H[y].siz;
        if (H[H[x].hson].siz < H[y].siz) {
            H[x].hson = y;
        }
    }
}
inline void dfs2(int x, int t) {
    H[x].top = t, H[x].id = ++cnt;
    if (H[x].hson) dfs2(H[x].hson, t);
    for (auto &y : G[x]) {
        if (y != H[x].fa && y != H[x].hson) dfs2(y, y);
    }
}
inline int LCA(int u, int v) {
    int fu = H[u].top, fv = H[v].top;
    while (fu != fv) {
        if (H[fu].dep >= H[fv].dep) {
            u = H[fu].fa, fu = H[u].top;
        } else {
            v = H[fv].fa, fv = H[v].top;
        }
    }
    return H[u].dep > H[v].dep ? v : u;
}
inline bool query(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].ok;
    }
    if (l <= tr[p].mid()) {
        if (query(ls(p), l, r)) return true;
    }
    if (r > tr[p].mid()) {
        if (query(rs(p), l, r)) return true;
    }
    return false;
}
inline void update(int p, int x) {
    tr[p].ok = true;
    if (tr[p].l == tr[p].r) return;
    update(x <= tr[p].mid() ? ls(p) : rs(p), x);
}
inline bool check(int u, int v) {
    int fu = H[u].top, fv = H[v].top;
    while (fu != fv) {
        if (H[fu].dep >= H[fv].dep) {
            if (query(1, H[fu].id, H[u].id)) return true;
            u = H[fu].fa, fu = H[u].top;
        } else {
            if (query(1, H[fv].id, H[v].id)) return true;
            v = H[fv].fa, fv = H[v].top;
        }
    }
    if (H[u].id > H[v].id) swap(u, v);
    return query(1, H[u].id, H[v].id);
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r};
    if (l != r) {
        build(ls(p), l, tr[p].mid());
        build(rs(p), tr[p].mid() + 1, r);
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    std::vector<int> ans, p(m);
    dfs1(1, 0), dfs2(1, 1), build(1, 1, n);
    for (int i = 1; i <= m; i++) {
        cin >> a[i] >> b[i];
        L[i] = LCA(a[i], b[i]);
    }
    auto cmp = [&](int x, int y) {
        return H[L[x]].dep > H[L[y]].dep;
    };
    iota(p.begin(), p.end(), 1);
    sort(p.begin(), p.end(), cmp);
    for (auto &i : p) {
        if (!check(a[i], b[i])) {
            ans.push_back(L[i]);
            update(1, H[L[i]].id);
        }
    }
    cout << ans.size() << '\n';
    for (auto &x : ans) {
        cout << x << ' ';
    }
    return 0;
}
```
