摘要：并查集，分组，离线，双指针，排序

[传送门：https://www.luogu.com.cn/problem/CF2020D](https://www.luogu.com.cn/problem/CF2020D)

## 题意

给定一张有 $n$ 个点，一开始没有边的图。Alice 对这张图进行了 $m$ 次操作。每次操作，她会选择三个参数 $a, k, d$，将 $a, a + d, a + 2d, \dots, a + kd$ 这 $k + 1$ 个点两两连边。问经过了 $m$ 次操作之后，这张图一共有几个连通分量。

## 分析思路

首先观察到题目中的 $1 \le d \le 10$，再观察一下 $a, a + d, \dots, a + kd$ 这些点，可以发现他们模 $d$ 的值都是相等的。考虑对操作离线。

我们按照 $d$ 和 $a \bmod d$ 的值对操作进行分类。对于同一类操作，如果某两个操作的点有交集，那么这两个操作**等价于他们合并之后的操作**，因为这两个操作中的任意一个点都可以先走到交集中的某个点，再走到另一个点。于是这两个操作**并集中的所有点**最后都会在同一个连通分量中。

所以，对于每一类操作，我们可以排序后使用双指针进行集合求并，得到若干个极大的操作，然后再使用并查集进行暴力连边。这样可以保证每个点最多只会被暴力操作 $10$ 次。集合求并的复杂度为 $O(m \log m)$，暴力连边时间复杂度 $O(n \times \max d)$，总时间复杂度 $O(m \log m + n \times \max d_i)$，可以通过本题。
 
## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
struct DSU {
    std::vector<int> fa, siz;
    DSU(int n) : fa(n + 1), siz(n + 1, 1) {
        std::iota(fa.begin(), fa.end(), 0);
    }
    inline int find(int x) {
        return fa[x] == x ? x : fa[x] = find(fa[x]);
    }
    inline bool same(int x, int y) {
        return find(x) == find(y);
    }
    // true if x and y were not in the same set, false otherwise.
    inline bool merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        if (siz[fx] < siz[fy]) swap(fx, fy);
        fa[fy] = fx, siz[fx] += siz[fy], siz[fy] = 0;
        return true;
    }
    // x -> y, a.k.a let x be son of y (disable merge by rank).
    inline bool directed_merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        fa[fx] = fy, siz[fy] += siz[fx], siz[fx] = 0;
        return true;
    }
};
int t, n, m, a, k, d;
inline void solve(void) {
    cin >> n >> m;
    vector<pair<int, int>> segs[11][11];
    for (int i = 1; i <= m; i++) {
        cin >> a >> d >> k;
        segs[d][a % d].push_back({a, a + k * d});
    }
    DSU D(n);
    int ans = n;
    for (int i = 1; i <= 10; i++) {
        for (int rem = 0; rem < i; rem++) {
            auto &seg = segs[i][rem];
            const int siz = seg.size();
            sort(seg.begin(), seg.end());
            for (int l = 0, r = 0; l < siz; l = r) {
                int L = seg[l].first, R = seg[l].second;
                while (r < siz && seg[r].first <= R) {
                    R = max(R, seg[r].second), r++;
                }
                for (int j = L; j < R; j += i) {
                    ans -= D.merge(j, j + i);
                }
            }
        }
    }
    cout << ans << '\n';
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
