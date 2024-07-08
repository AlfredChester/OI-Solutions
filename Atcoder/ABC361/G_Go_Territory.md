摘要：并查集，双指针

[传送门：https://www.luogu.com.cn/problem/AT_abc361_g](https://www.luogu.com.cn/problem/AT_abc361_g)

## 题意

平面上有 $n$ 个障碍点 $(x_1, y_1), (x_2, y_2), \dots (x_n, y_n)$。如果从一个**非障碍点**出发，经历若干次上、下、左、右移动而不经过障碍点无法到达 $(-1, -1)$，那么称这个点是不自由的。求出平面上不自由的点的个数。

## 分析思路

如果我们将每一行没有被障碍覆盖的点的连续段预处理出来，容易发现这些连续段上的所有点是否自由是一定的，且上下临接的连续段的关系可以构成一张图。考虑对第 $-1$ 行单独建立一个点 $0$。则某个连续段是否自由等价于在图上是是否能到达 $0$。即最终的答案是非自由的连续段的长度之和。

第一步，我们先处理出每一行的连续段。考虑用 $(x, y_a, y_b)$ 描述在第 $x$ 行，纵坐标从 $y_a$ 到 $y_b$ 的一个连续段。使用 `map<int, vector<int>>` 记录每一行分别有哪些障碍点纵坐标。对于每一行，先对其横坐标进行排序。随后对于横坐标差距大于 $1$ 的两个相邻的点记录连续段。特别地我们插入 $(x, -inf, y_0 - 1)$ 与 $(x, y_{last} + 1, inf)$，方便后续图论建模。同时对于输入的每个障碍点，需要预留它的上一行和下一行为空 `vector`（这样才能更方便连接到点 $0$），在处理线段时若仍然为空视作 $(x, -inf, inf)$。时间复杂度 $O\left(n \log n\right)$。

第二步，考虑建图。首先对于 $y_a = -inf$ 或 $y_b = inf$ 的连续段，一定能先向左右移动到非常远的列，随后往下移动到第 $-1$ 行，最终移动到 $(-1, -1)$。所以对于这些线段，直接与点 $0$ 相连。然后要处理临接的连续段。考虑使用双指针算法求出在 $(x, y_a, y_b)$ 的上一行（即第 $x + 1$ 行），且就纵坐标来说，与 $(y_a, y_b)$ 有交的那些线段。容易发现符合条件的线段的编号不减。时间复杂度 $O\left(n\right)$。

最终统计出答案即可。总时间复杂度 $O\left(n \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
struct DSU {
    std::vector<int> fa, siz;
    DSU(int n) : fa(n + 1), siz(n + 1, 1) {
        iota(fa.begin(), fa.end(), 0);
    }
    inline int find(int x) {
        return fa[x] == x ? x : fa[x] = find(fa[x]);
    }
    inline bool merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        if (siz[fx] < siz[fy]) swap(fx, fy);
        fa[fy] = fx, siz[fx] += siz[fy], siz[fy] = 0;
        return true;
    }
};
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int n, x, y;
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    map<int, vector<int>> mp;
    for (int i = 1; i <= n; i++) {
        cin >> x >> y, mp[x].push_back(y);
        mp[x - 1], mp[x + 1]; // 为空行预留，map 访问下标就会创建新对象。
    }
    const int inf = 1e9;
    vector<array<int, 3>> segs;
    // 非障碍点构成的连续段。以 (x, ya, yb) 描述，至多 3n 个。
    for (auto &[x, ys] : mp) {
        if (ys.empty()) {
            segs.push_back({x, -inf, inf});
        } else {
            const int sz = ys.size();
            std::sort(ys.begin(), ys.end());
            segs.push_back({x, -inf, ys[0] - 1});
            for (int i = 0; i < sz - 1; i++) {
                if (ys[i + 1] - ys[i] > 1) {
                    segs.push_back({x, ys[i] + 1, ys[i + 1] - 1});
                }
            }
            segs.push_back({x, ys.back() + 1, inf});
        }
    }
    DSU D(segs.size());
    const int siz = segs.size();
    // 用点 0 代表自由。我们可以知道 segs[0] = {-1, -inf, inf}，因为 -1 一定是空行。
    for (int i = 0; i < siz; i++) {
        auto [x, ya, yb] = segs[i];
        if (ya == -inf || yb == inf) {
            D.merge(0, i); // 如果某个段左侧或右侧没有任何障碍点，那它一定自由。
        }
    }
    // 双指针
    for (int i = 0, j = 0; i < siz; i++) {
        // 找到 segs[i] 的上一行中，就 y 坐标而言与 segs[i] 有交的 segs[j]。
        while (j < siz && (segs[j][0] < segs[i][0] + 1 || segs[j][2] < segs[i][1])) j++;
        while (j < siz && segs[j][0] == segs[i][0] + 1 && segs[j][1] <= segs[i][2]) {
            D.merge(i, j++);
        }
        j--; // 最后一个 j 不满足，需要撤回以保证下次还能用到这个 j。
    }
    i64 ans = 0;
    for (int i = 0; i < siz; i++) {
        if (D.find(i) != D.find(0)) {
            // 对于非自由连续段，答案加上它们的长度。
            ans += segs[i][2] - segs[i][1] + 1;
        }
    }
    cout << ans << endl;
    return 0;
}

```
