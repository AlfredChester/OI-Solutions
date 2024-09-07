摘要：数据结构，树链剖分，离线，线段树，并查集，直径

[传送门：https://www.luogu.com.cn/problem/P10866](https://www.luogu.com.cn/problem/P10866)

## 题意

给定一颗 $n$ 个顶点的树，一开始全部都是白色节点。需要进行 $q$ 次操作，每次给定树上一条链的两个端点，将这条链上的所有点全部染成黑色。每次操作后所有黑色点和树上连接两个黑色点的边、所有白色点和树上连接两个白色点的边构成了一个森林，求这个森林的直径。

## 分析思路

CSP-S 模拟赛考了这题。前期的思维都对了，就差并查集维护直径的 trick。确实是学到了。

首先对于这棵树，我们可以预处理出每个点第一次被染成黑色的时间。具体来说，我们使用树链剖分，给每个点维护第一次被染成黑色的时间作为点权，初始全部为 $\inf$。每次操作等价于区间取 $\min$，懒标记线段树即可。

预处理完所有店之后，考虑按照时间离线。我们维护一个黑色森林的直径，初始大家都没有连边（那些白色点作为直径为 $1$ 的黑色树，不影响答案）。对于每个点被加入之后，我们检查它的所有连边是不是能放到黑色森林上。于是问题转化为：进行一些连边，动态维护直径。

这时候我们需要用到一个经典结论：给定两个点集 $S, T$，其中 $S$ 的一条直径的两个端点是 $(x, y)$， $T$ 的一条直径的两个端点是 $(u, v)$，则最后直径的两个端点一定在 $\{x, y, u, v\}$ 中四选二产生。所以我们可以用并查集维护动态维护黑森林的直径。

对于白点，我们只要倒着做一遍即可。时间复杂度 $O\left(n \log^2 n\right)$，主要来自树链剖分 + 线段树。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
const int inf = (int)1e9;
struct SegmentTree {
    struct Node {
        int l, r, mn, lazy;
        inline int mid(void) {
            return (l + r) >> 1;
        }
        inline void set_min(int val) {
            mn = min(mn, val), lazy = min(lazy, val);
        }
    } tr[N << 2];
    inline int ls(int p) { return p << 1; }
    inline int rs(int p) { return p << 1 | 1; }
    inline void build(int p, int l, int r) {
        tr[p] = {l, r, inf, inf};
        if (l != r) {
            const int m = (l + r) >> 1;
            build(ls(p), l, m), build(rs(p), m + 1, r);
        }
    }
    inline void maintain(int p) {
        tr[p].mn = min(tr[ls(p)].mn, tr[rs(p)].mn);
    }
    inline void pushdown(int p) {
        if (tr[p].lazy != inf) {
            tr[ls(p)].set_min(tr[p].lazy);
            tr[rs(p)].set_min(tr[p].lazy);
            tr[p].lazy = inf;
        }
    }
    inline void set_min(int p, int l, int r, int v) {
        if (l <= tr[p].l && tr[p].r <= r) {
            return tr[p].set_min(v);
        }
        pushdown(p);
        if (l <= tr[p].mid()) set_min(ls(p), l, r, v);
        if (r > tr[p].mid()) set_min(rs(p), l, r, v);
        maintain(p);
    }
    inline int query(int p, int l, int r) {
        if (l <= tr[p].l && tr[p].r <= r) {
            return tr[p].mn;
        }
        pushdown(p);
        int m = tr[p].mid(), ans = inf;
        if (l <= m) ans = min(ans, query(ls(p), l, r));
        if (r > m) ans = min(ans, query(rs(p), l, r));
        return ans;
    }
} tr;
struct Node {
    int fa, dep, siz, hson, top, id;
    inline void assign(int t, int x) {
        top = t, id = x;
    }
} T[N];
vector<int> G[N];
int t, n, m, u, v, cnt, ans[N], rnk[N], f[N][20];
inline void dfs1(int x, int fa) {
    f[x][0] = fa, T[x] = {fa, T[fa].dep + 1, 1};
    for (int i = 1; (1 << i) <= T[x].dep; i++) {
        f[x][i] = f[f[x][i - 1]][i - 1];
    }
    for (auto y : G[x]) {
        if (y == fa) continue;
        dfs1(y, x), T[x].siz += T[y].siz;
        if (T[T[x].hson].siz < T[y].siz) {
            T[x].hson = y;
        }
    }
}
inline void dfs2(int x, int t) {
    T[x].assign(t, ++cnt), rnk[cnt] = x;
    if (T[x].hson) dfs2(T[x].hson, t);
    for (auto y : G[x]) {
        if (y != T[x].fa && y != T[x].hson) dfs2(y, y);
    }
}
inline void Modify(int u, int v, int w) {
    int fu = T[u].top, fv = T[v].top;
    while (fu != fv) {
        if (T[fu].dep >= T[fv].dep) {
            tr.set_min(1, T[fu].id, T[u].id, w);
            u = T[fu].fa, fu = T[u].top;
        } else {
            tr.set_min(1, T[fv].id, T[v].id, w);
            v = T[fv].fa, fv = T[v].top;
        }
    }
    tr.set_min(1, min(T[u].id, T[v].id), max(T[u].id, T[v].id), w);
}
inline int LCA(int u, int v) {
    if (T[u].dep < T[v].dep) swap(u, v);
    while (T[u].dep > T[v].dep) {
        u = f[u][__lg(T[u].dep - T[v].dep)];
    }
    if (u == v) return u;
    for (int k = __lg(T[u].dep); k >= 0; k--) {
        if (f[u][k] != f[v][k]) {
            u = f[u][k], v = f[v][k];
        }
    }
    return f[u][0];
}
inline int dis(int u, int v) {
    return T[u].dep + T[v].dep - 2 * T[LCA(u, v)].dep + 1;
}
struct DiameterDSU {
    int diameter;
    std::vector<int> fa, siz, l, r;
    DiameterDSU(int n) : diameter(1), fa(n + 1), siz(n + 1, 1), l(n + 1), r(n + 1) {
        std::iota(fa.begin(), fa.end(), 0);
        std::iota(l.begin(), l.end(), 0);
        std::iota(r.begin(), r.end(), 0);
    }
    inline int find(int x) {
        return fa[x] == x ? x : fa[x] = find(fa[x]);
    }
    inline bool same(int x, int y) {
        return find(x) == find(y);
    }
    // true if x and y were not in the same set, false otherwise.
    inline bool merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        if (siz[fx] < siz[fy]) swap(fx, fy);
        std::vector<int> choice = {l[fx], r[fx], l[fy], r[fy]};
        int u = 0, v = 0, ans = 0;
        for (int i = 0; i < 4; i++) {
            for (int j = i + 1; j < 4; j++) {
                const int d = dis(choice[i], choice[j]);
                if (d > ans) ans = d, u = choice[i], v = choice[j];
            }
        }
        l[fx] = u, r[fx] = v, diameter = max(diameter, ans);
        fa[fy] = fx, siz[fx] += siz[fy], siz[fy] = 0;
        return true;
    }
};
inline void solve(void) {
    cin >> n >> m, tr.build(1, 1, n), cnt = 0;
    for (int i = 1; i <= n; i++) {
        G[i].clear(), ans[i] = 0;
        memset(f[i], 0x00, sizeof(f[i]));
    }
    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    dfs1(1, 0), dfs2(1, 1);
    for (int i = 1; i <= m; i++) {
        cin >> u >> v, Modify(u, v, i);
    }
    vector<int> t(n + 1);
    deque<pair<int, int>> Q1, Q2;
    for (int i = 1; i <= n; i++) {
        t[i] = tr.query(1, T[i].id, T[i].id);
        Q1.push_back({t[i], i});
        Q2.push_back({t[i], i});
    }
    DiameterDSU D1(n), D2(n);
    std::sort(Q1.begin(), Q1.end());
    std::sort(Q2.begin(), Q2.end(), greater<>());
    for (int i = 1; i <= m; i++) {
        while (!Q1.empty() && Q1[0].first <= i) {
            auto fr = Q1[0];
            for (auto v : G[fr.second]) {
                if (t[v] <= i) D1.merge(v, fr.second);
            }
            Q1.pop_front();
        }
        ans[i] = D1.diameter;
    }
    for (int i = m; i >= 1; i--) {
        while (!Q2.empty() && Q2[0].first > i) {
            auto fr = Q2[0];
            for (auto v : G[fr.second]) {
                if (t[v] > i) D2.merge(v, fr.second);
            }
            Q2.pop_front();
        }
        ans[i] = max(ans[i], D2.diameter);
    }
    for (int i = 1; i <= m; i++) {
        cout << ans[i] << '\n';
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
