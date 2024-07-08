摘要：数据结构，逆序对

[传送门：https://www.luogu.com.cn/problem/CF1983D](https://www.luogu.com.cn/problem/CF1983D)

## 题意

给定两个长度为 $n$ 的，由互不相同的数组成的数列 $a_n, b_n$。每次操作你能选择满足 $r - l = q - p, 1 \leq l \leq r \leq n, 1 \le p \le q \le n$ 的四个下标 $l, r, p, q$，交换 $a_l$ 和 $a_r$，$b_p$ 和 $b_q$。问是否能经过若干次操作使得 $a = b$？若可以，输出 `YES`，否则输出 `NO`。

## 分析思路

$23 \min$ 切这题，上大分。

若 $a$ 和 $b$ 的元素不同，直接输出 `NO`。

否则，我们可以构造出一个长度为 $n$ 的排列 $p_n$，使得 $a_{p_i} = b_i$。我们最终的目的是使得 $p_i = i$。注意到每次操作，我们都能减少（或增加） $p_i$ 中的两个（或零个）逆序对。所以如果一开始 $p_n$ 的逆序对个数为奇数，则一定为 `NO`。否则使用类似冒泡排序的方法我们总能找到一种使得 $p_n$ 有序的操作方案。

使用树状数组求出逆序对。时间复杂度 $O\left(n\log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
using i64 = long long;
template <class T>
struct Fenwick {
    std::vector<T> c;
    inline int lowbit(int x) { return x & -x; }
    inline void merge(T &x, T &y) { x = x + y; }
    inline T subtract(T x, T y) { return x - y; }
    inline void update(size_t pos, T x) {
        for (pos++; pos < c.size(); pos += lowbit(pos)) merge(c[pos], x);
    }
    inline void clear(void) {
        for (auto &x : c) x = T();
    }
    inline T query(size_t pos) {
        T ans = T();
        for (pos++; pos; pos ^= lowbit(pos)) merge(ans, c[pos]);
        return ans;
    }
    inline T query(size_t l, size_t r) {
        return subtract(query(r), query(l - 1));
    }
    Fenwick(size_t len) : c(len + 2) {}
};
Fenwick<i64> C(N);
int n, a[N], b[N];
inline void YES(void) { cout << "YES\n"; }
inline void Yes(void) { cout << "Yes\n"; }
inline void NO(void) { cout << "NO\n"; }
inline void No(void) { cout << "No\n"; }
inline void YES_NO(bool cond) { cond ? YES() : NO(); }
inline void Yes_No(bool cond) { cond ? Yes() : No(); }
inline void solve(void) {
    std::cin >> n;
    std::map<int, int> pos;
    for (int i = 1; i <= n; i++) {
        std::cin >> a[i], pos[a[i]] = i;
    }
    std::vector<int> vec = {0};
    for (int i = 1; i <= n; i++) std::cin >> b[i];
    for (int i = 1; i <= n; i++) {
        if (!pos.count(b[i])) return NO();
        vec.push_back(pos[b[i]]);
    }
    i64 inv = 0;
    Fenwick<int> C(2 * n);
    for (int i = 1; i <= n; i++) {
        inv += i - 1 - C.query(vec[i]);
        C.update(vec[i], 1);
    }
    YES_NO(inv % 2 == 0);
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int t = 1;
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
