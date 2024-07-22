摘要：种类并查集

[传送门：https://www.luogu.com.cn/problem/CF1044D](https://www.luogu.com.cn/problem/CF1044D)

## 题意

有一个长度为 $2^{30}$ 的数列 $a_0, a_1, \dots, a_n$。初始并不知道它们分别是多少。给定 $q$ 次操作，每次操作分为以下两种类型：

- `1 l r x` 表示已经知道了 $a_l \oplus a_{l + 1} \oplus \dots \oplus a_r = x$，若与之前某条件冲突则忽略该条件；
- `2 l r` 询问 $a_l \oplus a_{l + 1} \oplus \dots \oplus a_r$，若无法确定输出 `-1`。

## 分析思路

下文记 $x_d$ 为 $x$ 在二进制表示下的第 $d$ 位。

首先，我们设原数组的异或前缀和为 $s_i = a_0 \oplus a_1 \oplus \dots \oplus a_i$。对于操作一，我们知道 $s_r \oplus s_{l - 1} = x$。考虑对 $x$ 进行拆位，依据异或运算的定义，当 ${s_r}_d \neq {s_{l-1}}_{d}$ 时，$x_d = 1$，否则 $x_d = 0$。于是每个操作一就转化为了动态得知某两个变量相等或不相等，判断是否冲突——即种类并查集解决的基本问题。所以，我们对于每一位开一个种类并查集，即可支持操作一的判断冲突。

那么如何支持操作 $2$ 呢？注意到我们之前使用的关于异或的性质是充要的。也就是说，如果我们枚举每一位 $d$，若可以**确定** ${s_r}_d \neq {s_{l-1}}_{d}$ 或 ${s_r}_d ={s_{l-1}}_{d}$，则第 $d$ 位对答案的贡献是确定的。最终答案即为 $\sum_{d = 0}^{29} [{s_r}_d \neq {s_{l-1}}_{d}] \times 2^d$。否则，如果相等或不相等都不能确定，则答案为 $-1$。

最后的小细节就是数组下标过大，注意到最多只有 $2q$ 个不同的下标，所以我们可以使用 `std::map` 动态进行离散化。使用按秩合并的并查集，时间复杂度 $O\left(q \log V \right)$，其中 $V$ 为值域大小，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 400010;
struct DSU {
    std::vector<int> fa, siz;
    DSU(int n) : fa(n + 1), siz(n + 1, 1) {
        std::iota(fa.begin(), fa.end(), 0);
    }
    inline int find(int x) {
        return fa[x] == x ? x : fa[x] = find(fa[x]);
    }
    // true if x and y were not in the same set, false otherwise.
    inline bool merge(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        if (siz[fx] < siz[fy]) swap(fx, fy);
        fa[fy] = fx, siz[fx] += siz[fy], siz[fy] = 0;
        return true;
    }
};
template <class T>
struct OnlineMess { // we just want index.
    std::map<T, int> mp;
    inline int query(T x) {
        if (!mp.count(x)) mp[x] = mp.size() + 1;
        return mp[x];
    }
};
OnlineMess<int> M;
int n, opt, last, l, r, x;
vector<DSU> D(30, DSU(N * 2));
inline void read(int &x) { cin >> x, x ^= last; }
inline void write(int x) { cout << x << '\n', last = (x == -1 ? 1 : x); }
inline bool conflict(int l, int r, int x) {
    for (int i = 0; i < 30; i++) {
        if ((x >> i & 1) && D[i].find(r) == D[i].find(l)) return true;
        if (!(x >> i & 1) && D[i].find(r + N) == D[i].find(l)) return true;
    }
    return false;
}
inline void modify(int l, int r, int x) {
    if (l > r) std::swap(l, r);
    l = M.query(l - 1), r = M.query(r);
    if (conflict(l, r, x)) return;
    for (int i = 0; i < 30; i++) {
        if (x >> i & 1) {
            D[i].merge(r + N, l), D[i].merge(r, l + N);
        } else {
            D[i].merge(r, l), D[i].merge(r + N, l + N);
        }
    }
}
inline int query(int l, int r) {
    int ans = 0;
    if (l > r) std::swap(l, r);
    l = M.query(l - 1), r = M.query(r);
    for (int i = 0; i < 30; i++) {
        bool same = D[i].find(r) == D[i].find(l);
        bool diff = D[i].find(r) == D[i].find(l + N);
        if (!same && !diff) {
            return -1;
        } else if (diff) {
            ans |= 1 << i;
        }
    }
    return ans;
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> opt;
        if (opt == 1) {
            read(l), read(r), read(x), modify(l, r, x);
        } else {
            read(l), read(r), write(query(l, r));
        }
    }
    return 0;
}

```
