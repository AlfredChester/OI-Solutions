摘要：贪心，启发式合并

[传送门：https://www.luogu.com.cn/problem/P11322](https://www.luogu.com.cn/problem/P11322)

## 题意

给定一颗带边权的树，你需要选择最少的点作为关键点，使得每个点到关键点的距离不超过 $k$，并构造方案。

## 分析思路

对于一个点我们考虑它子树内的点，只有在不添加它作为关键点，子树内一定有一个点到关键点的距离大于 $k$ 时，我们才需要把它作为关键点。可以通过归纳法证明最优性。

这么做朴素实现是 $O(n^2)$ 的。考虑优化。

我们维护子树内坏点的深度的集合 $B_u$。具体来说先使用启发式合并从子树合并上来。然而子树内可能已经选了一些关键点，这时候 $u$ 就是子树内路径的最近公共祖先的祖先。两个点 $a, b$ 的距离小于等于 $d_a + d_b - 2d_u$。那么针对一个坏点 $a$ 我们肯定希望已经选的点 $b$ 的深度尽量小。于是我们再维护一下子树内深度最浅的已选点深度 $sub_u$ 即可轻松维护 $B_u$。

$u$ 必须加入关键点当且仅当 $\min_{x \in B_u} d_x - d_u > k$。对应清空 $B_u$，更新 $sub_u$ 即可。

关于删除，每个点最多被删除一次，因此均摊复杂度正确。最终时间复杂度为 $O(n \log^2 n)$。瓶颈在于启发式合并，可以用左偏树优化到 $O(n \log n)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 300010;
using i64 = long long;
vector<int> ans;
multiset<i64> B[N];
i64 k, d[N], sub[N];
vector<pair<int, int>> G[N];
inline void dfs(int x, int fa) {
    for (auto &[y, w] : G[x]) {
        if (y == fa) continue;
        d[y] = d[x] + w, dfs(y, x);
        sub[x] = min(sub[x], sub[y]);
        if (B[x].size() < B[y].size()) swap(B[x], B[y]);
        B[x].merge(B[y]), B[y].clear();
    }
    // dc + db - 2dx <= k
    B[x].insert(d[x]);
    i64 p = sub[x] - 2 * d[x];
    while (!B[x].empty() && p + *B[x].begin() <= k) {
        B[x].erase(B[x].begin());
    }
    if (!B[x].empty() && *B[x].rbegin() - d[fa] > k) {
        ans.push_back(x);
        sub[x] = d[x], B[x].clear();
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int n, u, v, w;
    optimizeIO(), cin >> n >> k;
    memset(sub, 0x3f, sizeof(sub));
    for (int i = 1; i < n; i++) {
        cin >> u >> v >> w;
        G[u].push_back({v, w});
        G[v].push_back({u, w});
    }
    d[0] = -1e18, dfs(1, 0);
    cout << ans.size() << '\n';
    for (auto &v : ans) {
        cout << v << ' ';
    }
    return 0;
}

```
