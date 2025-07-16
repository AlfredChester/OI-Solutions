摘要：bfs，dfs 序，时间复杂度均摊分析

[传送门：https://www.luogu.com.cn/problem/AT_abc414_f](https://www.luogu.com.cn/problem/AT_abc414_f)

## 题意

给定一棵树，定义两个节点的距离是连接他们的最短路径的边数。从 $1$ 号点开始，每次能跳跃到与当前节点距离恰好为 $k$ 的节点，求从 $1$ 号点出发，经过所有节点的最少跳跃次数。$1 \le k \le 20$。

## 分析思路

赛时 $15$ 分钟想加写，成功场切！

考虑 $O(n^2)$ 的做法是简单的。对于每个点，我们 bfs 时可以 $O(n)$ 遍历和它距离恰好为 $k$ 且没有访问过的点集进行松弛。这个做法的瓶颈就在于找点集。由于 bfs 的特性，我们知道这些点集的并的大小是 $O(n)$ 的。考虑去优化这一过程。

不妨钦定 $1$ 为根。我们知道 $u$ 到 $v$ 的最短路径一定是先从 $u$ 跳到 $\mathrm{LCA}(u, v)$，再从 $\mathrm{LCA}(u, v)$ 跳回到 $v$。由于 $k \le 20$，枚举 $\mathrm{LCA}(u, v)$，记 $u \to \mathrm{LCA}(u, v)$ 的倒数第二个点为 $x$，长度为 $i$，我们有：$v \in sub_{\mathrm{LCA}(u, v)} - sub_x, dep_v = dep_u - i + (k - i)$。

其中 $sub_x$ 表示以 $x$ 为根的子树。要保证 $v$ 不在 $x$ 的子树内的目的是不让它们的 $\mathrm{LCA}$ 退化为 $x$ 子树内的点。（当然，若 $\mathrm{LCA}(u, v) = u$ 则可以不关心不在 $x$ 的子树内这一条件）

$v \in sub_{\mathrm{LCA}(u, v)} - sub_x$ 这个条件自然让我们想到用 dfs 序刻画。我们知道子树在 dfs 序上是一段连续区间。而 $sub_{\mathrm{LCA}(u, v)}$ 对应的区间显然包含了 $sub_x$ 对应的区间。因此他们的差对应了两段区间。

那我们现在就要找指定深度，dfs 序为给定区间没有被访问过的点集。可以用 `std::set` 维护。对每个深度开一个 `set`，从中抽取出可以访问的点并且删除即可。由于刚才陈述的结论：点集的并的大小是 $O(n)$ 的，容易发现这是均摊 $O(n \log n)$ 的。

加上枚举 $\mathrm{LCA}$ 的复杂度为 $O(nk)$，便可以得到时间复杂度 $O(nk + n \log n)$ 的算法，可以通过。

实现上注意往上走的阶段深度小于等于 $0$ 要停止枚举。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
std::vector<int> G[N];
std::multiset<int> S[N];
int t, n, k, u, v, cnt, dis[N];
int seq[N], f[N], d[N], L[N], R[N];
inline void dfs(int x, int fa) {
    L[x] = ++cnt, seq[cnt] = x; // seq 是 dfs 序列
    d[x] = d[fa] + 1, f[x] = fa;
    for (auto &y : G[x]) {
        if (y != fa) dfs(y, x);
    }
    R[x] = cnt, S[d[x]].insert(L[x]);
}
inline void solve(void) {
    cin >> n >> k, cnt = 0;
    for (int i = 1; i <= n; i++) {
        G[i].clear(), S[i].clear(), dis[i] = -1;
    }
    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    int du, dd, u;
    std::queue<int> Q;
    dfs(1, 0), Q.push(1), dis[1] = 0;
    // 得到深度 d，dfn 在 [l, r] 内仍然没有被访问的邻居，进行松弛
    auto work = [&](int l, int r, int d) {
        for (auto it = S[d].lower_bound(l);;) {
            if (it == S[d].end() || *it > r) break;
            // S[d].erase(it) 会返回等效于删除之前 std::next(it) 的指针
            u = seq[*it], it = S[d].erase(it);
            dis[u] = dd + 1, Q.push(u);
        }
    };
    while (!Q.empty()) {
        int u = Q.front(), lst;
        du = d[u] + k, dd = dis[u];
        Q.pop(), work(L[u], R[u], du); // 只往下走
        for (int i = 1; i <= k; i++) {
            lst = u, u = f[u];
            if (du - 2 * i <= 1) break; // 不能再访问 1 号点，所以深度小于等于 1 就 break
            work(L[u], L[lst] - 1, du - 2 * i);
            work(R[lst] + 1, R[u], du - 2 * i);
        }
    }
    for (int i = 2; i <= n; i++) {
        cout << dis[i] << " \n"[i == n];
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
