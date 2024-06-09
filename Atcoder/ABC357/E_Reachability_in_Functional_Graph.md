摘要：图论，反图，拓扑排序，dfs

[传送门：https://www.luogu.com.cn/problem/AT_abc357_e](https://www.luogu.com.cn/problem/AT_abc357_e)

## 题意

有一个 $N$ 个点 $N$ 条边的有向图。每个点的出度为 $1$，第 $i$ 个点有一条指向 $a_i$ 的有向边。若从节点 $u$ 能走到节点 $v$，则称 $(u, v)$ 为可达点对。注意 $(u, u)$也是可达点对。求给定的有向图中可达点对的个数。

## 分析思路

注意到 $N$ 个点 $N$ 条边这一性质，可以想象这个图类似一个基环森林，比如样例二如下图。

```
5
2 4 3 1 2
```

![](https://cdn.luogu.com.cn/upload/image_hosting/dmrxrjbp.png)

容易发现，对于每个联通分量，都会有一个环，有一些点直接或者间接指向环上的点，每个环上的点对应了一颗树。如图可以看作这样几棵树。

![](https://cdn.luogu.com.cn/upload/image_hosting/ncv889eb.png)

观察上图，发现对于每棵树上的点（包括在环上的树根），都能到达：

- 根链上的所有点；
- 环上的所有点。

于是我们分几步处理此问题：

1. 使用拓扑排序，预处理出所有的环。
2. 建反图。对于环上的每个点所对应的树进行 `dfs`，求出树上每个点根链的长度，加入答案。对于每个树上的点还需要加上它所在联通分量的环的大小。

于是我们在 $O\left(n + m\right)$ 的时间复杂度内解决了此问题，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
using i64 = long long;
std::vector<int> fr[N];
i64 n, u, to[N], in[N], ans, onC[N], siz[N];
inline void dfs(int x, int fa, int dep) {
    ++siz[fa], ans += dep;
    for (auto y : fr[x]) {
        if (!onC[y]) dfs(y, fa, dep + 1);
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> to[i], in[to[i]]++;
        fr[to[i]].push_back(i);
    }
    std::queue<int> Q;
    for (int i = 1; i <= n; i++) {
        if (in[i] == 0) Q.push(i);
    }
    while (!Q.empty()) {
        u = Q.front(), Q.pop();
        if (--in[to[u]] == 0) Q.push(to[u]);
    }
    for (int i = 1; i <= n; i++) {
        std::vector<int> pos;
        for (int j = i; in[j]; j = to[j]) {
            in[j] = 0, pos.push_back(j), onC[j] = 1;
        }
        for (auto j : pos) {
            dfs(j, j, 0), ans += pos.size() * siz[j];
        }
    }
    cout << ans << endl;
    return 0;
}
```
