摘要：二维偏序，数据结构

[传送门：https://www.luogu.com.cn/problem/CF1915F](https://www.luogu.com.cn/problem/CF1915F)

## 题意

数轴上有 $n$ 个人。他们一开始在 $a_i$，然后以每秒 $1$ 个单位长度的时间向他们的终点 $b_i$ 移动（保证有 $ a_i < b_i $）。

如果某个时刻两个人在同一个位置，他们就会打招呼（注意，到了终点也可以和别人打招呼）。求最终打招呼发生了几次。

## 分析思路

如果一个人 $ i $ 最终会在他的运动过程中遇到 $ j $，则一定是 $a_i < a_j$，然后等到 $ j $ 到达其终点 $b_j$ 之后，$ i $ 仍在运动，并且他的终点 $b_i > b_j$。

简而言之，我们就是要求这样的点对 $(i, j)$，使得 $a_i < a_j < b_j < b_i$。考虑先将每个人按照起始位置从小到大排序，则只需要统计在第 $ i $ 个数后面且终点比 $ i $ 小的 $ j $ 的个数即可。

由于需要动态插入，动态统计比一个数小的数的个数，可以考虑树状数组。由于位置的值域太大，考虑使用离散化。注意离散化之后可能有 $2n$ 个不同的数，树状数组要开两倍空间（比赛的时候就挂这里了 QAQ ）。

## 代码

```cpp
#include <bits/stdc++.h>
#define lowbit(x) (x & (-x))
using namespace std;
const int N = 200010;
int t, n, a[N], b[N], c[2 * N]; // c 开 2 * n !!!
template <class _Tp>
struct Mess {
    vector<_Tp> v;
    inline void insert(_Tp x) { v.push_back(x); }
    inline void init(void) {
        sort(v.begin(), v.end());
        v.erase(unique(v.begin(), v.end()), v.end());
    }
    inline int query(_Tp x) {
        return lower_bound(v.begin(), v.end(), x) - v.begin() + 1;
    }
};  // 离散化模板
struct Node {
    int a, b;
    inline bool operator<(const Node &x) const { 
        return a < x.a; 
    }
} d[N];
inline void add(int x) {
    for (; x < 2 * N; x += lowbit(x)) c[x]++;
}
inline int query(int x) {
    int ans = 0;
    for (; x; x ^= lowbit(x)) ans += c[x];
    return ans;
}
inline void solve(void) {
    cin >> n;
    Mess<int> M;
    for (int i = 1; i <= n; i++) {
        cin >> a[i] >> b[i], M.insert(a[i]), M.insert(b[i]);
    }
    M.init(); // 离散化初始化，树状数组清空
    for (size_t i = 1; i <= M.v.size(); i++) c[i] = 0;
    for (int i = 1; i <= n; i++) {
        d[i] = {M.query(a[i]), M.query(b[i])};
    }
    long long ans = 0;
    sort(d + 1, d + 1 + n);
    for (int i = n; i >= 1; i--) {
        ans += query(d[i].b), add(d[i].b);
    }
    cout << ans << endl;
}
int main(int argc, char const *argv[]) {
    cin >> t;
    while (t--) solve();
    return 0;
}

```
