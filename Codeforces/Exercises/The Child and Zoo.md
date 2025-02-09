摘要：差分，并查集，数学

[传送门：https://www.luogu.com.cn/problem/CF437D](https://www.luogu.com.cn/problem/CF437D)

## 题意

给定一张 $n$ 个点 $m$ 条边的无向图，点有点权。定义 $f(p,q)$ 为所有 $p$ 到 $q$ 的简单路径的最小值的最大值，求 $\frac{\sum_{p, q, p\ne q}f(p, q)}{n(n - 1)}$。

## 分析思路

来补充一个和同学 duel 想到的差分做法。

考虑对点权从大到小排序。如果我们能统计出大于等于某个值 $w$ 的 $f(p, q)$ 的对数 $cnt_w$，那么就可以做到差分统计答案，即为 $\sum_{w = 1}^{\max a - 1} (cnt_{w + 1} - cnt_w) \cdot w$。

考虑怎么算出 $cnt$。把所有点按照点权离线下来，枚举 $w$ 的时候顺便加入所有点权小于等于 $w$ 的点之间的临边，用并查集维护连通性。可以发现有 $cnt_w = \sum siz_u^2$，其中 $u$ 是每个连通块的根。合并的同时维护 $cnt$ 即可。

时间复杂度 $O(n + m)$，可以通过。这类题目可以把两个点的点权 merge 转化为边权做也可以单独对于点权考虑，[相似例题链接](https://codeforces.com/gym/105692/problem/F)。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
const int N = 100010;
std::vector<int> G[N];
long long lst, tot, ans;
int n, m, a[N], b[N], fa[N], siz[N];
inline int find(int x) {
    return fa[x] == x ? x : fa[x] = find(fa[x]);
}
inline i64 comb(i64 x) { return x * x; }
inline void merge(int u, int v) {
    int fu = find(u), fv = find(v);
    if (fu == fv) return;
    tot -= comb(siz[fu]) + comb(siz[fv]);
    fa[fu] = fv, siz[fv] += siz[fu];
    tot += comb(siz[fv]);
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    deque<array<i64, 2>> Q;
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], b[i] = a[i];
        Q.push_back({a[i], i});
        fa[i] = i, siz[i] = 1;
    }
    for (int i = 1, u, v; i <= m; i++) {
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    tot = lst = n;
    sort(Q.begin(), Q.end(), greater<>());
    sort(b + 1, b + 1 + n, greater<>());
    for (int i = 1; i <= n; i++) {
        while (!Q.empty() && Q.front()[0] >= b[i]) {
            for (auto &v : G[Q.front()[1]]) {
                // 不用担心漏连，如果这次没连证明 a[v] < b[i], 
                // 枚举到较小的 i 之后总会会从 v 连接到 Q.front()[1] 的
                if (a[v] < b[i]) continue; 
                merge(v, Q.front()[1]);
            }
            Q.pop_front();
        }
        ans += (tot - lst) * b[i], lst = tot;
    }
    cout << fixed << setprecision(6);
    cout << 1.0 * ans / n / (n - 1) << endl;
    return 0;
}

```
