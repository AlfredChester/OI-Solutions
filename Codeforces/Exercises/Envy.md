摘要：可撤销并查集，生成树

[传送门：https://www.luogu.com.cn/problem/CF891C](https://www.luogu.com.cn/problem/CF891C)

## 题意

给定一张 $n$ 个点，$m$ 条边的图。$q$ 次询问，每次给出一个边集，求这个边集中的所有变能否同时出现在这张图的最小生成树上。

## 分析思路

注意到 MST 的一些性质：

1. MST 是一棵树 ~~（这不是废话吗）~~，所以不能有环；
2. 将所有小于某个权值的边全部加入后，MST 的连通性一定。（Kruskal 的核心）

于是我们考虑将询问离线，以权值为第一关键字，询问编号为第二关键字排序。对于每一个询问的相同边权的所有边，在加入之前需要判断不产生环。最后因为询问之间不能相互影响，需要使用可撤销并查集维护连通性。

时间复杂度 $O\left(q \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 500010;
struct CancelDSU {
    std::stack<int> S;
    std::vector<int> fa, siz;
    CancelDSU(int n) : S(), fa(n + 1), siz(n + 1, 1) {
        std::iota(fa.begin(), fa.end(), 0);
    }
    inline int find(int x) { // disable route compression.
        return fa[x] == x ? x : find(fa[x]);
    }
    inline bool same(int x, int y) {
        return find(x) == find(y);
    }
    inline bool merge(int x, int y) { // returns success or not.
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return S.push(-1), false;
        }
        if (siz[fx] < siz[fy]) std::swap(fx, fy);
        S.push(fy), siz[fx] += siz[fy], fa[fy] = fx;
        return true;
    }
    inline void _cancel(void) {
        if (S.empty()) return;
        if (S.top() == -1) return S.pop();
        siz[fa[S.top()]] -= siz[S.top()];
        fa[S.top()] = S.top(), S.pop();
    }
    inline void cancel(int t = 1) { // cancel the last t edges.
        while (t--) _cancel();
    }
};
int n, m, q, u[N], v[N], w[N], ok[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    vector<array<int, 3>> E;
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        cin >> u[i] >> v[i] >> w[i];
        E.push_back({w[i], u[i], v[i]});
    }
    vector<array<int, 4>> Q;
    sort(E.begin(), E.end()), cin >> q;
    for (int i = 1, a, b; i <= q; i++) {
        cin >> a, ok[i] = 1;
        for (int j = 1; j <= a; j++) {
            cin >> b, Q.push_back({w[b], i, u[b], v[b]});
        }
    }
    int cur = 0;
    CancelDSU D(n);
    sort(Q.begin(), Q.end());
    for (int i = 0; i < Q.size();) {
        // dbg(i, Q[i][0], Q[i][1], Q[i][2], Q[i][3]);
        while (cur < m && E[cur][0] < Q[i][0]) {
            D.merge(E[cur][1], E[cur][2]), cur++;
        }
        size_t l = i, r = i;
        while (r < Q.size() && Q[r][1] == Q[l][1] && Q[r][0] == Q[l][0]) {
            ok[Q[r][1]] &= D.merge(Q[r][2], Q[r][3]), r++;
        }
        // [l, r)
        D.cancel(r - l), i = r;
    }
    for (int i = 1; i <= q; i++) {
        cout << (ok[i] ? "YES" : "NO") << '\n';
    }
    return 0;
}

```
