摘要：莫队，分块，懒标记

[传送门：https://www.luogu.com.cn/problem/CF1000F](https://www.luogu.com.cn/problem/CF1000F)

## 题意

给定长度为 $n$ 的序列 $a$，求出 $[l, r]$ 区间中的任意一个出现了一次的数，如果不存在则输出 $0$。

## 分析思路

考虑莫队，我们可以维护一个集合表示当前区间内的所有答案。

容易想到的方案是使用 `std::set` 进行动态的插入和删除。然而这会让时间复杂度变为 $O(n \sqrt n \log n)$，带入 $n$ 的最大值 $5 \times 10^5$ 计算得 $5.8 * 10^9$，无法通过此题，考虑去除 $O(\log n)$ 的复杂度。

我们可以使用懒标记表示队列里面的元素是否被删除过，单次插入删除时间复杂度变为 $O(1)$。

对于每次查询，由于单次移动最多删除 $O(\sqrt n)$ 个元素，我们可以暴力地，对于队首打了懒标记的元素出队，最多出队 $O(\sqrt n)$ 个。于是时间复杂度变为 $O(n \sqrt n)$。

此题带来的启示是，每次操作后我们不一定要马上得到结果，只要使得最终得到答案的时间复杂度不超过 $O(\sqrt n)$ 即可。

## 代码

```cpp
// CF由于本题时间限制问题，需要加火车头才能通过
#include <bits/stdc++.h>
using namespace std;
const int N = 500010;
struct Query {
    int l, r, idx;
} Q[N];
queue<int> cur;
int n, q, l, r, a[N];
int c[N], ans[N], lazy[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void insert(int x) {
    lazy[x] = 0, cur.push(x);
}
inline void erase(int x) { lazy[x] = 1; }
inline void add(int x) {
    if (c[a[x]] == 0) insert(a[x]);
    if (c[a[x]] == 1) erase(a[x]);
    c[a[x]]++;
}
inline void del(int x) {
    if (c[a[x]] == 2) insert(a[x]);
    if (c[a[x]] == 1) erase(a[x]);
    c[a[x]]--;
}
inline int get(void) {
    while (!cur.empty()) {
        if (lazy[cur.front()]) cur.pop();
        else return cur.front();
    }
    return 0;
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    cin >> q;
    for (int i = 1; i <= q; i++) {
        cin >> l >> r, Q[i] = {l, r, i};
    }
    const int B = sqrt(n);
    sort(Q + 1, Q + 1 + q, [=](Query &a, Query &b) {
        if (a.l / B != b.l / B) return a.l < b.l;
        return (a.l / B) & 1 ? a.r < b.r : a.r > b.r;
    });
    l = r = 1, add(1);
    for (int i = 1; i <= q; i++) {
        while (Q[i].l < l) add(--l);
        while (r < Q[i].r) add(++r);
        while (l < Q[i].l) del(l++);
        while (Q[i].r < r) del(r--);
        ans[Q[i].idx] = get();
    }
    for (int i = 1; i <= q; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}
```