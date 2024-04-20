摘要：分块，树状数组，RMQ

[传送门：https://www.luogu.com.cn/problem/CF830B](https://www.luogu.com.cn/problem/CF830B)

## 题意

桌子上有 $n$ 张卡片排成一列，每张卡片上有一个数 $a_i$（$1\le a_i\le100000,a_i\in \mathbf{Z}$）。

有一个人每次从卡片的最左端开始，依次向右看过去，如果当前这张牌是现在的全局最小值，那么将它移除，如果不是，那么将它放到序列的最右端。持续到序列为空为止。求他进行的这两种操作的总次数。

## 分析思路

来一个特别~~逗比~~的分块解法。（其实或许还可以线段树上二分但是我不会 QAQ）

记当前正在看的数的位置为 $cur$。如果我们把这个序列看做一个环，这个问题等价于：找到环上第一个仍然未被删除的在 $cur$ 右边的值最小的新位置 $pos$，然后加上 $cur$ 到 $pos$ 之间的距离。（话说这定语真是有点长）

环问题很容易让我们想到破环为链，然后查找 $[cur, cur + n - 1]$ 中的下标最小的最小值的位置，这个问题可以使用分块解决。

但是我们还需要维护一个删除操作，分块本身很难直接支持显式的删除操作，于是我们可以通过将 $a_{pos}$ 赋值为极大值并且暴力重构块的方式来在 $O\left(\sqrt n\right)$ 的时间复杂度内解决找到并且维护最小值信息的操作。

最后就是查询两个位置之间的距离，我们构造数组 $del_i$ 表示 $i$ 这个位置是否已经被删除，查询 $[l, r]$ 之间的距离等价于查询 $del_l, \cdots, del_r$ 中 $1$ 的个数，这一操作可以通过树状数组轻易地动态维护。

时间复杂度 $O\left(n \sqrt n + n \log n\right)$，~~喜提最劣解~~。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
template <class _Tp>
struct Fenwick {
    std::vector<_Tp> c;
    inline int lowbit(int x) { return x & -x; }
    inline void merge(_Tp &x, _Tp y) { x = x + y; } // remember to modify
    inline _Tp subtract(_Tp x, _Tp y) { return x - y; }
    inline void update(size_t pos, _Tp x) {
        for (pos++; pos < c.size(); pos += lowbit(pos)) merge(c[pos], x);
    }
    inline void clear(void) {
        for (auto &x : c) x = _Tp();
    }
    inline _Tp query(size_t pos) {
        _Tp ans = _Tp();
        for (pos++; pos; pos ^= lowbit(pos)) merge(ans, c[pos]);
        return ans;
    }
    inline _Tp query(size_t l, size_t r) {
        return subtract(query(r), query(l - 1));
    }
    Fenwick(size_t len) : c(len + 2) {}
};
Fenwick<int> C(N);
int n, m, B, a[N], bel[N], mn[450];
inline void update(int &x, int y) {
    if (a[y] < a[x]) x = y;
}
inline void remake(int b) {
    mn[b] = b * B;
    for (int i = b * B; bel[i] == b; i++) {
        update(mn[b], i);
    }
}
// 找到 [l, r] 以内下标最小的最小值
inline int query(int l, int r) {
    int L = bel[l], R = bel[r], res = l;
    if (L == R) {
        for (int i = l; i <= r; i++) update(res, i);
        return res;
    }
    for (int i = l; bel[i] == L; i++) update(res, i);
    for (int i = L + 1; i < R; i++) update(res, mn[i]);
    for (int i = R * B; i <= r; i++) update(res, i);
    return res;
}
inline void remove(int x) {
    x %= n;
    for (int i = 0; i < 2; i++, x += n) {
        C.update(x, -1), a[x] = 1e9, remake(bel[x]);
    }
}
int main(int argc, char const *argv[]) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    cin >> n, m = 2 * n, B = sqrt(m);
    for (int i = 0; i < n; i++) {
        cin >> a[i], a[i + n] = a[i];
    }
    for (int i = 0; i < m; i++) {
        C.update(i, 1), bel[i] = i / B;
    }
    for (int i = 0; i <= (m - 1) / B; i++) {
        remake(i);
    }
    long long cur = 0, ans = 0;
    for (int i = 1; i <= n; i++) {
        int pos = query(cur, cur + n - 1);
        ans += C.query(cur, pos);
        remove(pos), cur = pos % n;
    }
    cout << ans << '\n';
    return 0;
}
```
