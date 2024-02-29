# Codeforces Round 925 (Div. 3) F - Chat Screenshots 题解

## 题意

有一个排行榜 $A$，其中 $A_i$ 表示第 $i$ 名的用户编号 ( $1 \leq A_i \leq n$ )。用户 $i$ 看到的排行榜会把他本身放在第一位，其余用户按照在原排行榜中的相对顺序排列。

现在给定 $k$ 个用户看到的排行榜，问是否存在至少一个满足每位用户看到的排行榜的原排行榜。

## 分析思路

记第 $i$ 个看到的排行榜的第 $j$ 位为 $a_{i, j}$ ( $1 \leq i \leq k, 0 \leq j < n$ )。则 $a_{i, 1}, a_{i, 2}, \cdots, a_{i, n - 1}$ 为有效信息，可以得到在原排行榜中的相对顺序有 $ a_{i, 1} < a_{i, 2} < \cdots < a_{i, n - 1} $。

考虑对问题进行图论建模。若可以得到 $x$ 在原排行榜中的排名相较于 $y$ 比较靠前，则连接一条 $y \to x$ 的有向边。若图中有环，即出现矛盾。所以建立图之后判断环即可。拓扑排序或 `dfs` 均可解决有向图判环问题。

时间复杂度 $O(\sum {n \cdot k})$，可以通过本题。

## 代码

`dfs` 版：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
vector<int> G[N];
int t, n, k, a[N], vis[N], col[N];
inline bool judge(int x) {
    col[x] = 0;
    for (auto y : G[x]) {
        if (col[y] == 0) return false;
        else if (col[y] == -1) {
            if (!judge(y)) return false;
        }
    }
    return col[x] = 1, true;
}
inline bool check(void) {
    for (int i = 1; i <= n; i++) {
        if (col[i] == -1 && !judge(i)) {
            return false;
        }
    }
    return true;
}
inline void solve(void) {
    cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        vis[i] = 0, G[i].clear(), col[i] = -1;
    }
    for (int i = 1; i <= k; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> a[j];
            if (j >= 3) G[a[j]].push_back(a[j - 1]);
        }
    }
    cout << (check() ? "YES" : "NO") << '\n';
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

拓扑排序版：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
vector<int> G[N];
int t, n, k, u, a[N], in[N], vis[N];
inline bool check(void) {
    int cnt = 0;
    queue<int> Q;
    for (int i = 1; i <= n; i++) {
        if (in[i] == 0) Q.push(i);
    }
    while (!Q.empty()) {
        u = Q.front(), Q.pop();
        if (!vis[u]) vis[u] = 1, ++cnt;
        for (auto v : G[u]) {
            if (--in[v] == 0) Q.push(v);
        }
    }
    return cnt == n;
}
inline void solve(void) {
    cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        G[i].clear(), in[i] = vis[i] = 0;
    }
    for (int i = 1; i <= k; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> a[j];
            if (j >= 3) {
                G[a[j]].push_back(a[j - 1]), ++in[a[j - 1]];
            }
        }
    }
    cout << (check() ? "YES" : "NO") << '\n';
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
