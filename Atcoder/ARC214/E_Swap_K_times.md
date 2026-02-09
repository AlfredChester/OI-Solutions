摘要：贪心，调整法，二维数点

[传送门：https://www.luogu.com.cn/problem/AT_arc214_e](https://www.luogu.com.cn/problem/AT_arc214_e)

## 题意

给定两个长度为 $n$ 的数组 $a, b$ 和一个整数 $k$。每次操作，你要恰好做 $k$ 次对 $a$ 中相邻两个元素的交换。交换的元素可以自己选定，操作有后效性。问至少要几次操作能把 $a$ 变成 $b$，或报告不可能。题目保证 $a$ 与 $b$ 是相同元素的排列。  

## 分析思路

考虑一个排列 $p$，其中 $a_{p_i} = b_i$。要把 $a$ 变成 $b$，等价于把排列 $p$ 排序。对于 $k$ 次交换，我们其实可以通过连续交换两处相同的位置来浪费掉两个操作。设 $t$ 为 $p$ 的逆序对数，需要的操作次数 $c$ 只要满足以下两个条件：$ck \ge t, ck \equiv t \pmod 2$。

对于相同奇偶性的 $t$，显然找到使 $t$ 最小的 $p$ 一定不劣。一个简单的想法是把 $a$ 和 $b$ 中相同出现的元素从左往右匹配，这样满足 $a_{p_i} = a_{p_j}$ 的 $i, j$ 一定不是逆序对，在某个奇偶性下 $t$ 最小。

对于另一种奇偶性，假设最优的排列为 $q$，考虑从 $p$ 调整得到。进行一些分类讨论。注意到交换 $l < r, p_l > p_r$ 的 $l, r$ 并不会改变逆序对的奇偶性，所以我们只会交换 $l < r, p_l < p_r$ 的位置。设 $p_l = x, p_r = y$，交换后逆序对数变化为：

$$\Delta = 2 \times \left| \{z | l < z < r, p_z \in (x, y)\} \right| + 1$$

容易发现这是一个二维数点的形式。为了使矩形最小显然只会选择最近的 $p_l, p_r$ 使得 $A_{p_l} = A_{p_r}$。矩形数量是 $O(n)$ 的，直接数点计算即可。

时间复杂度 $O(n \log n)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
#include "alfred/data_structure/fenwick.hpp"
using namespace std;
const int N = 300010;
using i64 = long long;
const i64 inf = 9e18;
std::queue<int> Q[N];
int T, n, k, cnt, a[N], b[N], p[N];
inline i64 inv(int p[]) {
    i64 t = 0;
    Fenwick<int> C(n);
    for (int i = n; i >= 1; i--) {
        t += C.query(p[i]);
        C.update(p[i], 1);
    }
    return t;
}
inline i64 calc(i64 t) {
    i64 cmn = (t + k - 1) / k;
    if (cmn * k % 2 == t % 2) {
        return cmn;
    } else if ((cmn + 1) * k % 2 == t % 2) {
        return cmn + 1;
    } else {
        return inf;
    }
}
i64 ans[N];
vector<array<int, 4>> qry[N];
inline void solve(void) {
    cin >> n >> k, cnt = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], Q[a[i]].push(i);
    }
    for (int i = 1; i <= n; i++) {
        cin >> b[i], qry[i].clear();
    }
    for (int i = 1; i <= n; i++) {
        p[i] = Q[b[i]].front(), Q[b[i]].pop();
    }
    bool zero = false;
    i64 t = inv(p), res = calc(t), mn = inf;
    auto add_rect = [&](int l, int r) {
        if (l + 1 < r && p[l] + 1 < p[r]) {
            qry[l].push_back({p[l] + 1, p[r] - 1, -1, ++cnt});
            qry[r - 1].push_back({p[l] + 1, p[r] - 1, 1, cnt});
        } else {
            zero = true;
        }
    };
    for (int i = 1; i <= n; i++) Q[a[p[i]]].push(i);
    for (int v = 1; v <= n; v++) {
        int lst = -1, cur;
        while (!Q[v].empty()) {
            cur = Q[v].front(), Q[v].pop();
            if (lst != -1) {
                add_rect(lst, cur);
            }
            lst = cur;
        }
    }
    if (!zero) {
        Fenwick<int> C(n);
        for (int i = 1; i <= n; i++) {
            C.update(p[i], 1);
            for (auto &[l, r, v, j] : qry[i]) {
                ans[j] += v * C.query(l, r);
            }
        }
        for (int i = 1; i <= cnt; i++) {
            mn = min(mn, ans[i]), ans[i] = 0;
        }
    } else mn = 0;
    if (mn != inf) {
        res = min(res, calc(t + 2 * mn + 1));
    }
    cout << (res == inf ? -1 : res) << '\n';
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> T;
    while (T--) solve();
    return 0;
}

```
