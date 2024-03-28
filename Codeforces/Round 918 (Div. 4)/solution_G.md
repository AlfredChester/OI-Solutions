# Codeforces Round 918 (Div. 4) G - Bicycles 题解

## 题意

给定一个 $ n $ 个点（城市）， $ m $ 条边的无向联通图。每到一个城市可以买一辆自行车。第 $ i $ 个城市购买的自行车有一个慢度系数 $ s_i $。如果骑着这辆自行车通过边权为 $ w $ 的边，则需要 $ w \times s_i $ 的时间。每到一个城市可以任意选择是否更换自行车。问从 $ 1 $ 号城市到第 $ n $ 号城市所要的最短时间。

## 分析思路

分层图板题。

注意到数据范围比较小，考虑拆点。将第 $ i $ 个点根据当前自行车的慢度系数拆成 $ 1000n $ 个点，然后对于每条边，考虑是否换自行车，跑 `Dijkstra` 即可。

时间复杂度 $O\left(tm \log kn \right)$，其中 $k$ 为拆点带来的常数 $1000$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1010;
typedef long long ll;
struct Edge {
    ll to, w;
};
struct Node {
    ll pos, w, s;
    inline bool operator<(const Node &x) const {
        return w > x.w;
    }
} last;
vector<Edge> G[N];
priority_queue<Node> heap;
ll t, n, m, u, v, w, s[N], dis[N][N], vis[N][N];
inline ll Dijkstra(void) {
    heap.push({1, 0, s[1]}), dis[1][s[1]] = 0;
    while (!heap.empty()) {
        last = heap.top(), heap.pop();
        if (vis[last.pos][last.s]) {
            continue;
        }
        vis[last.pos][last.s] = 1;
        for (auto edge : G[last.pos]) {
            const ll ns = last.s, u = edge.to;
            if (dis[u][ns] > last.w + edge.w * ns) {
                dis[u][ns] = last.w + edge.w * ns;
                heap.push({u, dis[u][ns], ns});
                heap.push({u, dis[u][ns], s[u]});
            }
        }
    }
    ll minn = LLONG_MAX;
    for (int i = 1; i <= 1000; i++) {
        minn = min(minn, dis[n][i]);
    }
    return minn;
}
inline void solve(void) {
    cin >> n >> m;
    memset(dis, 0x3f, sizeof(dis));
    memset(vis, 0x00, sizeof(vis));
    for (int i = 1; i <= n; i++) G[i].clear();
    for (int i = 1; i <= m; i++) {
        cin >> u >> v >> w;
        G[u].push_back({v, w});
        G[v].push_back({u, w});
    }
    for (int i = 1; i <= n; i++) {
        cin >> s[i];
    }
    cout << Dijkstra() << '\n';
}
int main(int argc, char const *argv[]) {
    cin >> t;
    while (t--) solve();
    return 0;
}

```
