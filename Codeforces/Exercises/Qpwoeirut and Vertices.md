摘要：Kruskal 重构树，ST 表，LCA

[传送门：https://www.luogu.com.cn/problem/CF1706E](https://www.luogu.com.cn/problem/CF1706E)

## 题意

给出 $n$ 个点 $m$ 条边的无向连通图。$q$ 次询问使得编号在 $[l, r]$ 内的所有点联通至少需要按照输入顺序加入几条边。

## 分析思路

考虑给每条边附上它是第几条边作为它的权值。建立 Kruskal 重构树，则两点 $u, v$ 在本题中的答案即为 $a_{lca(u, v)}$。由于时间取 $\max$ 是可重复贡献问题，所以本题答案为 $a_{lca(l, l + 1, \dots, r)}$。ST 表维护区间 LCA 即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 300010;
struct DSU {
    std::vector<int> fa, siz;
    DSU(int n) : fa(n + 1), siz(n + 1, 1) {
        std::iota(fa.begin(), fa.end(), 0);
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
        fa[fy] = fx, siz[fx] += siz[fy], siz[fy] = 0;
        return true;
    }
    // x -> y, a.k.a let x be son of y (disable merge by rank).
    inline bool directed_merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        fa[fx] = fy, siz[fy] += siz[fx], siz[fx] = 0;
        return true;
    }
};
std::vector<int> G[N];
int t, n, m, q, u, v, cnt, a[N], d[N], f[N][21];
inline void dfs(int x, int fa) {
    d[x] = d[fa] + 1, f[x][0] = fa;
    for (int i = 1; (1 << i) <= d[x]; i++) {
        f[x][i] = f[f[x][i - 1]][i - 1];
    }
    for (auto y : G[x]) {
        if (y != fa) dfs(y, x);
    }
}
inline int LCA(int u, int v) {
    if (d[u] < d[v]) swap(u, v);
    while (d[u] > d[v]) {
        u = f[u][__lg(d[u] - d[v])];
    }
    if (u == v) return u;
    for (int k = __lg(d[u]); k >= 0; k--) {
        if (f[u][k] != f[v][k]) {
            u = f[u][k], v = f[v][k];
        }
    }
    return f[u][0];
}
struct Info {
    int lca;
    friend Info operator+(Info &A, Info &B) {
        return {LCA(A.lca, B.lca)};
    }
};
template <class T>
struct SparseTable {
private:
    int n;
    std::vector<std::vector<T>> ST;

public:
    SparseTable() {}
    SparseTable(int N) : n(N), ST(N, std::vector<T>(std::__lg(N) + 1)) {}
    template <class InitT>
    SparseTable(std::vector<InitT> &init) : SparseTable(init.size()) {
        for (int i = 0; i < n; i++) ST[i][0] = T(init[i]);
        for (int i = 1; (1 << i) <= n; i++) {
            for (int j = 0; j + (1 << i) - 1 < n; j++) {
                ST[j][i] = ST[j][i - 1] + ST[j + (1 << (i - 1))][i - 1];
            }
        }
    }
    inline T query(int l, int r) { // 0 based
        if (l > r) return T();
        int w = std::__lg(r - l + 1);
        return ST[l][w] + ST[r - (1 << w) + 1][w];
    }
    inline T disjoint_query(int l, int r) {
        T ans = T();
        for (int i = l; i <= r; i += (1 << __lg(r - i + 1))) {
            ans = ans + ST[i][__lg(r - i + 1)];
        }
        return ans;
    }
};
inline void solve(void) {
    cin >> n >> m >> q, cnt = n;
    for (int i = 1; i <= 2 * n; i++) {
        G[i].clear(), a[i] = 0;
    }
    DSU D(2 * n);
    auto addEdge = [&](int u, int v) {
        G[u].push_back(v);
        G[v].push_back(u);
    };
    for (int i = 1; i <= m; i++) {
        cin >> u >> v;
        int fu = D.find(u), fv = D.find(v);
        if (fu != fv) {
            a[++cnt] = i;
            D.directed_merge(fu, cnt);
            D.directed_merge(fv, cnt);
            addEdge(cnt, fu), addEdge(cnt, fv);
        }
    }
    dfs(D.find(1), 0);
    std::vector<Info> lca_info(n + 1);
    for (int i = 1; i <= n; i++) {
        lca_info[i].lca = i;
    }
    SparseTable<Info> lca_table(lca_info);
    while (q--) {
        cin >> u >> v;
        int range_lca = lca_table.query(u, v).lca;
        cout << a[range_lca] << ' ';
    }
    cout << '\n';
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
