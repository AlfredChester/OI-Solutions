摘要：树的直径，树状 dp，贪心

[传送门：https://www.luogu.com.cn/problem/AT_abc361_e](https://www.luogu.com.cn/problem/AT_abc361_e)

## 题意

给定一颗 $n$ 个节点的带边权的树。求从某个点出发，遍历所有点的最小花费（注意不需要回到开始起点，只需要遍历完所有点即可）。

## 分析思路

如果我们从某个点出发，到达所有点并且返回到起点，很明显对于最优的情况，所有的边都恰好经过了两次，即答案为 $2 \sum C_i$。

然而，这题要求我们不需要返回起始点。这个条件等价于让我们选择某个起点 $u$ 和终点 $v$，将原先 $2 \sum C_i$ 的答案减去 $\mathrm{dis}(u, v)$。显然取 $\mathrm{dis}(u, v)$ 等于树的直径答案最小。

于是，我们使用树 dp 求出树的直径，将二倍边权和减去直径即为答案。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
using i64 = long long;
struct Edge {
    i64 to, w;
};
vector<Edge> G[N];
i64 n, u, v, w, mx, ans, dp[N][2];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void dfs(int x, int fa) {
    for (auto edge : G[x]) {
        if (edge.to == fa) continue;
        dfs(edge.to, x), v = dp[edge.to][0] + edge.w;
        if (v > dp[x][0]) {
            dp[x][1] = dp[x][0], dp[x][0] = v;
        } else if (v > dp[x][1]) {
            dp[x][1] = v;
        }
    }
    mx = max(mx, dp[x][0] + dp[x][1]);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i < n; i++) {
        cin >> u >> v >> w, ans += 2 * w;
        G[u].push_back({v, w});
        G[v].push_back({u, w});
    }
    // find diameter of tree.
    dfs(1, 0), cout << ans - mx << endl;
    return 0;
}

```
