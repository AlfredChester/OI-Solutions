摘要：图论，简单博弈

[传送门：https://www.luogu.com.cn/problem/CF1968D](https://www.luogu.com.cn/problem/CF1968D)

## 题意

给定一个长度为 $n$ 的数列 $a_1, a_2, \cdots, a_n$ 与一个长度为 $n$ 的排列 $p$。`Bodya` 与 `Sasha` 两人玩一个游戏，游戏持续 $k$ 轮。`Bodya` 与 `Sasha` 两人分别一开始在 $P_B, P_S$ 两个位置，每一轮玩家都会做以下两件事情：

- 如果当前该玩家停留的位置为 $x$，则他的分数增加 $a_x$；
- 该玩家可以选择停留在当前位置 $x$ 或前往 $p_x$。

若 `Bodya` 与 `Sasha` 都采取最优策略，最终谁的分数更高？输出分数更高者的名字，若分数相等输出 `Draw`。

## 分析思路

我们以样例中的第四组数据为例：

```
8 10 4 1
5 1 4 3 2 8 6 7
1 1 2 1 2 100 101 102
```

由于两人在排列上移动，排列之间互相到达的关系又可以视作是一个有向图（基环森林），可以证明两个人最终能到达的点个数有限。例如样例可以看作如下的图：

![](https://cdn.luogu.com.cn/upload/image_hosting/xyqfnmqu.png)

`Bodya` 先后能到达的点是 $\{4, 3\}$，`Sasha` 先后能到达的点是 $\{1, 5, 2\}$。两人先后能到达的点我们可以使用 `dfs` 或其他方法求出。

然后我们考虑两人的最优策略。注意到两人最终应该会在某个点停止不动。我们可以枚举他们最终停下的点 $pos$。设从他们各自的起点 $P$ 到当前点 $pos$ 经历了 $x$ 步（$0 \leq x \leq k$），路径上经过的所有点的点权和为 $sum$，则最终的答案即为 $sum + (k - x) \times a_{pos}$。我们只要找到这个表达式的最大值，暴力枚举即可。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000010;
using i64 = long long;
using i128 = __int128;
i64 t, n, k, pb, ps, a[N], p[N];
inline void solve(void) {
    cin >> n >> k >> pb >> ps;
    for (int i = 1; i <= n; i++) {
        cin >> p[i];
    }
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    set<int> B, S;
    vector<int> BR, SR;
    for (; !B.count(pb); pb = p[pb]) {
        B.insert(pb), BR.push_back(pb);
    }
    for (; !S.count(ps); ps = p[ps]) {
        S.insert(ps), SR.push_back(ps);
    }
    i64 maxB = 0, maxS = 0, bSum = 0, sSum = 0;
    for (int i = 0; i <= min((size_t)k, BR.size() - 1); i++) {
        maxB = max(maxB, bSum + (k - i) * a[BR[i]]);
        bSum += a[BR[i]];
    }
    for (int i = 0; i <= min((size_t)k, SR.size() - 1); i++) {
        maxS = max(maxS, sSum + (k - i) * a[SR[i]]);
        sSum += a[SR[i]];
    }
    if (maxB > maxS) {
        cout << "Bodya\n";
    } else if (maxB == maxS) {
        cout << "Draw\n";
    } else {
        cout << "Sasha\n";
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
