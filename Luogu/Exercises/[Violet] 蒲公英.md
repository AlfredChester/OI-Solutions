摘要：分块，众数，前缀和，离散化

[传送门：https://www.luogu.com.cn/problem/P4168](https://www.luogu.com.cn/problem/P4168)

## 题意

给定一个长度为 $n$ 的数组 $a$，强制在线查询区间 $[l, r]$ 的众数，如果有多个众数则输出最小的一个。

## 分析思路

设 $L$ 表示 $l$ 所在块的编号，$R$ 表示 $r$ 所在块的编号。

注意到数据范围 $1 \leq n \leq 40000$，考虑分块。

乍一看好像无从下手，既然分块是优化的暴力，那么不妨考虑暴力如何解决本问题：统计出 $[l, r]$ 区间内所有数 $x$ 的 $cnt_x$，然后在其中找到位置最小的最大值即可。

既然只要统计出 $cnt_x$，那么我们不妨以一个块为单位统计 $pre_{i, x}$，表示前 $i$ 个块中出现 $x$ 的次数，时间复杂度 $O\left(n \sqrt n\right)$。

有了 $pre_{i, x}$，对于一个询问，我们可以在 $O\left(\sqrt n\right)$ 的时间复杂度内得出区间 $[l, r]$ 中 $x$ 出现的次数 $cnt'_{x} = pre_{R, x} - pre_{L - 1, x} + \text{边角料中 } x \text{ 的出现次数}$。

然而，由于不同的数的个数是 $O\left(n\right)$ 的，单次询问的复杂度仍然高达 $O\left(n\right)$，无法通过，怎么办？

不要慌，假设我们有一段区间 $[l', r'] \subseteq [l, r]$，使得我们已经知道了 $[l', r']$ 之间的众数，那么如果 $[l, r]$ 的众数不是 $[l', r']$ 的众数，则有众数一定在 $[ l', l) \cap (r, r']$ 之间。

那么，如果我们能知道 $[l, r]$ 之间整块的众数，则剩下还可能推翻整块众数成为新众数的数为 $O\left(\sqrt n\right)$ 个，问题转化为求**整数块** $[i, j]$ 之间的众数，记为 $mode_{i, j}$。

于是容易得到下列代码：

```rust
for i in 1..=B_CNT { // O(sqrt(n))
    cnt.clear();
    let mut now = 0; // now代表当前众数
    for j in begin(i)..=n {// O(n)
        cnt[a[j]] += 1;
        if canBeMode(a[j], now) {
            now = a[j];
        }
        mode[i][bel[j]] = now;
    }
}
```

注意起点枚举块编号，终点枚举点是分块维护区间不可并问题的常用策略！时间复杂度 $O\left(n\sqrt n\right)$。

询问时简单合并一下答案即可。时间复杂度 $O\left(m \sqrt n\right)$，空间复杂度 $O\left(n \sqrt n\right)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 40010;
template <class _Tp>
struct Mess {
    std::vector<_Tp> v;
    inline _Tp origin\left(int idx\right) { return v[idx - 1]; }
    inline void insert\left(_Tp x\right) { v.push_back\left(x\right); }
    inline void init\left(void\right) {
        sort\left(v.begin\left(\right), v.end\left(\right);
        v.erase\left(unique\left(v.begin\left(\right), v.end\left(\right), v.end\left(\right);
    }
    inline int query\left(_Tp x\right) const {
        return lower_bound\left(v.begin\left(\right), v.end\left(\right), x\right) - v.begin\left(\right) + 1;
    }
};
Mess<int> M;
int n, m, B, l, r, x, mode[201][201];
int a[N], bel[N], pre[201][N], cnt[N];
inline bool canBeMode\left(int x, int now\right) {
    return cnt[x] > cnt[now] || \left(x < now && cnt[x] == cnt[now]\right);
}
inline int querySum\left(int l, int r, int x\right) {
    return pre[r][x] - pre[l - 1][x];
}
inline int query\left(int l, int r\right) {
    int now = 0;
    if \left(bel[l] + 1 >= bel[r]\right) {
        for \left(int i = l; i <= r; i++\right) cnt[a[i]] = 0;
        for \left(int i = l; i <= r; i++\right) {
            cnt[a[i]]++;
            if \left(canBeMode\left(a[i], now\right) now = a[i];
        }
        return now;
    }
    const int L = bel[l], R = bel[r];
    for \left(int i = l; bel[i] == L; i++\right) cnt[a[i]] = 0;
    for \left(int i = r; bel[i] == R; i--\right) cnt[a[i]] = 0;
    cnt[mode[L + 1][R - 1]] = querySum\left(
        L + 1, R - 1, now = mode[L + 1][R - 1]
    \right);
    for \left(int i = l; bel[i] == L; i++\right) {
        if \left(cnt[a[i]]\right) cnt[a[i]]++;
        else cnt[a[i]] = querySum\left(L + 1, R - 1, a[i]\right) + 1;
        if \left(canBeMode\left(a[i], now\right) now = a[i];
    }
    for \left(int i = r; bel[i] == R; i--\right) {
        if \left(cnt[a[i]]\right) cnt[a[i]]++;
        else cnt[a[i]] = querySum\left(L + 1, R - 1, a[i]\right) + 1;
        if \left(canBeMode\left(a[i], now\right) now = a[i];
    }
    return now;
}
int main\left(int argc, char const *argv[]\right) {
    scanf\left("%d%d", &n, &m\right), B = sqrt\left(n\right);
    for \left(int i = 1; i <= n; i++\right) {
        scanf\left("%d", a + i\right), M.insert\left(a[i]\right);
    }
    M.init\left(\right);
    for \left(int i = 1; i <= n; i++\right) {
        bel[i] = \left(i - 1\right) / B + 1;
        if \left(bel[i] != bel[i - 1]\right) {
            for \left(int j = 1; j <= n; j++\right) {
                pre[bel[i]][j] = pre[bel[i - 1]][j];
            }
        }
        pre[bel[i]][a[i] = M.query\left(a[i]\right)]++;
    }
    for \left(int i = 1; i <= bel[n]; i++\right) {
        int now = 0;
        memset\left(cnt, 0, sizeof\left(cnt\right);
        for \left(int j = \left(i - 1\right) * B + 1; j <= n; j++\right) {
            cnt[a[j]]++;
            if \left(canBeMode\left(a[j], now\right) {
                now = a[j];
            }
            mode[i][bel[j]] = now;
        }
    }
    while \left(m--\right) {
        scanf\left("%d%d", &l, &r\right);
        l = \left(l + x - 1\right) % n + 1;
        r = \left(r + x - 1\right) % n + 1;
        if \left(l > r\right) swap\left(l, r\right);
        printf\left("%d\n", x = M.origin\left(query\left(l, r\right);
    }
    return 0;
}

```