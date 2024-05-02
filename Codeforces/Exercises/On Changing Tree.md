摘要：树的序列化，其他技巧

[传送门：https://www.luogu.com.cn/problem/CF396C](https://www.luogu.com.cn/problem/CF396C)

## 题意

有一颗 $n$ 个节点的有根树，根节点的编号为 $1$，每个点有点权，初始都为 $0$，需要维护两个操作：

- `1 v x k` 对于 $v$ 的子树上的每个节点 $u$，把它的权值加上 $x - dis_{u, v} \times k$；
- `2 v` 查询节点 $v$ 的权值。

## 分析思路

这题本身并不难，树链剖分显然可以解决这一问题，然而 ~~我太懒了~~ 有时间复杂度和代码实现难度上都优于树剖的一种方法解决该问题——树的序列化。

具体来说，我们首先使用 `dfs` 序把树拍扁。对于操作 $1$ 中 $v$ 的子树上的节点 $u$，它权值的增加量为：

$$
\begin{align*}
\Delta u &= x - dis_{u, v} \times k \\
&= x - (dep_u - dep_v) \times k \\
&= (x + dep_v \times k) - dep_u \times k
\end{align*}
$$

对于第一项，我们发现其实是给整个子树加上一个常数，我们用树状数组 $A$ 维护区间加单点查这一信息。

对于第二项，我们发现 $dep_u$ 是节点 $u$ 的属性，**我们只要知道它增加的 $k$ 的总量是多少就可以了**，所以我们使用树状数组 $B$ 维护区间加单点查**某个点增加的 $k$ 的总量**。

询问的答案即为 `A.query(dfn[v]) - dep[v] * B.query(dfn[v])`。

算法的总时间复杂度为 $O\left(n \log n\right)$，优于树链剖分。

**启示：维护的信息与深度有关时，不妨对原式进行变换，然后将深度与其他常数分开处理。**

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 300010;
template <int mod>
inline unsigned down(unsigned x) { return x >= mod ? x - mod : x; }
template <int mod>
struct ModInt {
    unsigned int x;
    ModInt() = default;
    ModInt(unsigned int x) : x(x) {}
    friend istream &operator>>(istream &in, ModInt &a) { return in >> a.x; }
    friend ostream &operator<<(ostream &out, ModInt a) { return out << a.x; }
    friend ModInt operator+(ModInt a, ModInt b) { return down<mod>(a.x + b.x); }
    friend ModInt operator-(ModInt a, ModInt b) { return down<mod>(a.x - b.x + mod); }
    friend ModInt operator*(ModInt a, ModInt b) { return 1ULL * a.x * b.x % mod; }
    friend ModInt operator/(ModInt a, ModInt b) { return a * ~b; }
    friend ModInt operator^(ModInt a, int b) {
        ModInt ans = 1;
        for (; b; b >>= 1, a *= a)
            if (b & 1) ans *= a;
        return ans;
    }
    friend ModInt operator~(ModInt a) { return a ^ (mod - 2); }
    friend ModInt operator-(ModInt a) { return down<mod>(mod - a.x); }
    friend ModInt &operator+=(ModInt &a, ModInt b) { return a = a + b; }
    friend ModInt &operator-=(ModInt &a, ModInt b) { return a = a - b; }
    friend ModInt &operator*=(ModInt &a, ModInt b) { return a = a * b; }
    friend ModInt &operator/=(ModInt &a, ModInt b) { return a = a / b; }
    friend ModInt &operator^=(ModInt &a, int b) { return a = a ^ b; }
    friend ModInt &operator++(ModInt &a) { return a += 1; }
    friend ModInt operator++(ModInt &a, int) {
        ModInt x = a;
        a += 1;
        return x;
    }
    friend ModInt &operator--(ModInt &a) { return a -= 1; }
    friend ModInt operator--(ModInt &a, int) {
        ModInt x = a;
        a -= 1;
        return x;
    }
    friend bool operator==(ModInt a, ModInt b) { return a.x == b.x; }
    friend bool operator!=(ModInt a, ModInt b) { return !(a == b); }
};
using mint = ModInt<1000000007>;
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
vector<int> G[N];
Fenwick<mint> A(N), B(N);
int siz[N], dfn[N], d[N];
int n, p, q, opt, x, k, cnt;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void dfs(int x, int fa) {
    dfn[x] = ++cnt, d[x] = d[fa] + 1, siz[x] = 1;
    for (auto y : G[x]) {
        dfs(y, x), siz[x] += siz[y];
    }
}
inline void subAdd(Fenwick<mint> &C, int p, mint v) {
    C.update(dfn[p], v), C.update(dfn[p] + siz[p], -v);
}
inline void modify(int p, mint x, mint k) {
    subAdd(A, p, x + k * d[p]), subAdd(B, p, k);
}
inline mint query(int p) {
    return A.query(dfn[p]) - d[p] * B.query(dfn[p]);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 2; i <= n; i++) {
        cin >> p, G[p].push_back(i);
    }
    dfs(1, 0), cin >> q;
    while (q--) {
        cin >> opt >> p;
        if (opt == 1) {
            cin >> x >> k, modify(p, x, k);
        } else {
            cout << query(p) << '\n';
        }
    }
    return 0;
}
```
