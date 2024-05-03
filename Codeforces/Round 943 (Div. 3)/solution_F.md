摘要：异或，贪心，莫队

[传送门：https://www.luogu.com.cn/problem/CF1968F](https://www.luogu.com.cn/problem/CF1968F)

本题解介绍一种本题的莫队做法。~~是谁赛时打了个贼长的莫队还挂了我不说。~~ 

## 题意

我们称一个数列 $x_1, x_2, \cdots, x_m$ 有意思，当且仅当它可以被分成 $k > 1$ 个部分，使得每个部分的异或和都相等。

现在给定一个长度为 $n$ 的数列 $a$，$q$ 次询问 $a$ 的子段 $a_l, a_{l + 1}, \cdots, a_r$ 是不是有意思。

## 分析思路

首先我们注意到一个性质：对于 $k > 3$ 的拆分，一定能够转化为 $k = 2$ 或 $k = 3$ 的拆分（读者可以自行思考原因）。

我们记 $s_i = a_1 \oplus a_2 \oplus \cdots \oplus a_i$，即异或前缀和。

对于 $k = 2$ 的拆分，因为两段的异或和相等，则最终两段异或起来是 $0$，有 $s_r = s_{l - 1}$，相反若 $s_r = s_{l - 1}$ 则 $[l, r]$ 一定能有一个 $k = 2$ 的拆分，对于这种情况我们特判直接输出 `YES` 即可。 

剩下的情况 $k = 3$。注意到在 $s$ 中我们一定需要找到两个位置 $l \leq x_1 \leq x_2 < r$，将原序列分为 $[l, x_1], [x_1 + 1, x_2], [x_2 + 1, r]$ 三部分，且这三部分的异或和相等。因为这三段总的异或起来是 $s_r \oplus s_{l - 1}$，中间有两段相等的抵消了，那么这三段的异或和也都是 $s_r \oplus s_{l - 1}$。

总结一下，我们有：

$$
s_{x_1} \oplus s_{l - 1} = s_{x_2} \oplus s_{x_1} = s_r \oplus s_{x_2} = s_{r} \oplus s_{l - 1} 
$$

于是我们可以得到：$s_{x_1} = s_r, s_{x_2} = s_{l - 1}$。

我们贪心地寻找在 $[l, r]$ 内最小的 $x_1$ 与 最大的 $x_2$，并且判断是否满足 $x_1 \leq x_2$ 即可。

找到这样两个位置的过程可以通过莫队完成。具体来说，我们用 `std::deque` 记录当前段内每个 $s$ 出现的位置的集合，取其 `front/back` 即可。

$s$ 数组可以提前离散化一下，减少一个 `std::map` 带来的 $O\left(\log n\right)$ 的复杂度。

注意可能最多有 $n + 1$ 个不同的 $s$，清空的时候注意多清空一个位置。

总时间复杂度 $O\left(n \log n + n \sqrt n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
template <class _Tp>
struct Mess {
    std::vector<_Tp> v;
    bool initialized = false;
    inline _Tp origin(int idx) { return v[idx - 1]; }
    inline void insert(_Tp x) { v.push_back(x); }
    template <typename T, typename... V>
    inline void insert(T x, V... v) { insert(x), insert(v...); }
    inline void init(void) {
        sort(v.begin(), v.end()), initialized = true;
        v.erase(unique(v.begin(), v.end()), v.end());
    }
    inline int query(_Tp x) {
        if (!initialized) init();
        return lower_bound(v.begin(), v.end(), x) - v.begin() + 1;
    }
};
struct Query {
    int l, r, idx;
} Q[N];
deque<int> pos[N];
int a[N], s[N], ms[N];
int t, n, m, l, r, ans1[N], ans2[N];
inline void solve(void) {
    Mess<int> M;
    cin >> n >> m, M.insert(0);
    for (int i = 1; i <= n; i++) {
        cin >> a[i], pos[i].clear();
        M.insert(s[i] = s[i - 1] ^ a[i]);
    }
    pos[n + 1].clear(); // 1. M 中可能有 n + 1 个元素！！！
    for (int i = 0; i <= n; i++) {
        ms[i] = M.query(s[i]);
    }
    for (int i = 1; i <= m; i++) {
        cin >> l >> r, Q[i] = {l, r, i};
    }
    const int B = sqrt(n);
    auto cmp = [=](Query a, Query b) -> bool {
        // 2. a.l / b != b.l / B！！ 不是等于！！
        if (a.l / B != b.l / B) return a.l < b.l;
        return (a.l / B) & 1 ? a.r < b.r : a.r > b.r;
    };
    l = 1, r = 0, sort(Q + 1, Q + 1 + m, cmp);
    auto add = [&](int x) -> void {
        if (pos[ms[x]].empty() || x < pos[ms[x]][0]) {
            pos[ms[x]].push_front(x);
        } else {
            pos[ms[x]].push_back(x);
        }
    };
    auto del = [&](int x) -> void {
        if (pos[ms[x]][0] == x) {
            pos[ms[x]].pop_front();
        } else {
            pos[ms[x]].pop_back();
        }
    };
    auto query1 = [&](int val) -> int { // last pos
        return pos[val].empty() ? -1 : pos[val].back();
    };
    auto query2 = [&](int val) -> int { // first pos
        return pos[val].empty() ? -1 : pos[val][0];
    };
    for (int i = 1; i <= m; i++) {
        while (Q[i].l < l) add(--l);
        while (r < Q[i].r) add(++r);
        while (l < Q[i].l) del(l++);
        while (Q[i].r < r) del(r--);
        ans1[Q[i].idx] = query1(ms[Q[i].l - 1]);
        ans2[Q[i].idx] = query2(ms[Q[i].r]);
    }
    for (int i = 1; i <= m; i++) {
        if (ans1[i] == -1 || ans2[i] == -1) {
            cout << "NO\n";
            continue;
        }
        cout << (ans2[i] <= ans1[i] ? "YES" : "NO") << '\n';
    }
    cout << '\n';
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
