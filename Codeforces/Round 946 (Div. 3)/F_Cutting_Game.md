摘要：双指针，二分，数据结构

[传送门：https://www.luogu.com.cn/problem/CF1974F](https://www.luogu.com.cn/problem/CF1974F)

好题。

## 题意

给定一个 $a \times b$ 的网格，上面有 $n$ 个位置两两不同的棋子。有两个人轮流进行 $m$ 次操作。每个操作分为四种类型：

- `U k` 删除当前棋盘最上面 $k$ 行；
- `D k` 删除当前棋盘最下面 $k$ 行；
- `L k` 删除当前棋盘最左面 $k$ 列；
- `R k` 删除当前棋盘最右面 $k$ 列。

保证每次操作后，棋盘不会被删除完。每次操作之后玩家获得的分数为删除的区域中棋子的个数，问最终两人的得分。

## 分析思路

注意到 $a, b$ 很大，但是最终我们最多删除的棋子只有 $n$ 个。考虑在不劣于 $O\left(n \log n\right)$ 的时间复杂度内解决此问题。

很容易地，我们可以维护四个变量 $rl, rr, cl, cr$，分别表示当前行的上下界，列的上下界。我们以 `U` 操作为例，将问题拆分成两个子问题来解决。

我们可以发现 `U k` 操作删除的行的范围为 $[rl, rl + k)$，下文所说的 `删除范围` 即指这一范围。下文所说的 `有效行` 指有棋子的行。

### I. 找到在删除范围内的有效行有哪些

注意到本质不同的有效行不超过 $n$ 个，考虑在这一点上做文章。

我们可以维护有序队列 $rows$，表示现在还存在于棋盘上的有效行有哪些。初始我们可以用 $O\left(n \log n\right)$ 的时间复杂度处理出这个队列。

对于 `U k` 操作，我们只要每次检查队首是否在删除范围内，如果是的话即取出队首进行进一步操作即可。

这一步对于**所有 $m$ 个操作**的总时间复杂度为 $O\left(n\right)$。

### II. 统计真正的删除的棋子个数

我们考虑定义 $row_i$ 表示**在第 $i$ 行上**的所有棋子的列所构成的集合。

设我们在上一部分中所取出的队首行为 $x$。我们的任务变为统计 $row_x$ 中**值**在 $[cl, cr]$ 范围内的数的个数。我们考虑二分，对于每一行，可以在 $O\left(\log n \right)$ 的时间复杂度内得到答案。最终累积答案即可。

对于 `D, L, R` 操作，类似地可以得到快速的统计方法，不再赘述。

最终的时间复杂度为 $O\left(n \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, a, b, n, m, x, y, k;
inline void solve(void) {
    cin >> a >> b >> n >> m;
    std::deque<int> rows, cols;
    map<int, vector<int>> row, col;
    for (int i = 1; i <= n; i++) {
        cin >> x >> y;
        row[x].push_back(y);
        col[y].push_back(x);
    }
    int rl = 1, rr = a, cl = 1, cr = b;
    for (auto &p : row) {
        rows.push_back(p.first);
        sort(p.second.begin(), p.second.end());
    }
    for (auto &p : col) {
        cols.push_back(p.first);
        sort(p.second.begin(), p.second.end());
    }
    // v 中值在 [l, r] 以内的有多少
    auto count = [&](vector<int> &v, int l, int r) -> int {
        if (l > r) return 0;
        auto L = lower_bound(v.begin(), v.end(), l);
        auto R = upper_bound(v.begin(), v.end(), r);
        return R - L;
    };
    char opt;
    int p1 = 0, p2 = 0;
    for (int i = 1, cur; i <= m; i++) {
        cin >> opt >> k, cur = 0;
        if (opt == 'U') {
            while (!rows.empty() && rows.front() < rl + k) {
                x = rows.front(), rows.pop_front();
                cur += count(row[x], cl, cr);
            }
            rl += k;
        } else if (opt == 'D') {
            while (!rows.empty() && rows.back() > rr - k) {
                x = rows.back(), rows.pop_back();
                cur += count(row[x], cl, cr);
            }
            rr -= k;
        } else if (opt == 'L') {
            while (!cols.empty() && cols.front() < cl + k) {
                x = cols.front(), cols.pop_front();
                cur += count(col[x], rl, rr);
            }
            cl += k;
        } else {
            while (!cols.empty() && cols.back() > cr - k) {
                x = cols.back(), cols.pop_back();
                cur += count(col[x], rl, rr);
            }
            cr -= k;
        }
        i & 1 ? p1 += cur : p2 += cur;
    }
    cout << p1 << ' ' << p2 << '\n';
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
