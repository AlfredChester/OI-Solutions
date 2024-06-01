摘要：反悔贪心

[传送门：https://www.luogu.com.cn/problem/CF1974G](https://www.luogu.com.cn/problem/CF1974G)

## 题意

你是一个物理学家。一开始你没有钱，每个月的末尾你会得到 $x$ 英镑。在第 $i$ 个月里，你可以付出 $c_i$ 英镑，获取 $1$ 个单位的幸福。

在任何时刻你都不能欠钱，问你在 $m$ 个月过后最多能获得多少幸福。

## 分析思路

反悔贪心板子题。

我们维护当前有的钱 $sum$ 和一个大根堆，记录用了哪些 $c_i$。

每次先试图获得当前月的幸福，$sum \gets sum - c_i$，并把 $c_i$ 放入堆中。如果当前 $sum < 0$，则需要进行反悔操作。我们希望剩下的钱尽可能多，而且每个月获得的幸福值都是一样的，所以我们只要从大根堆中取出最大的 $c_j$，并 $sum \gets sum + c_j$。容易发现只需要取出一个值就能保证 $sum \geq 0$。

在每个月的末尾，我们使 $sum \gets sum + x$。

最终堆的大小即为答案，总时间复杂度 $O\left(m \log m\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t, m, x, c, sum;
inline void solve(void) {
    cin >> m >> x, sum = 0;
    priority_queue<i64> heap;
    for (int i = 1; i <= m; i++) {
        cin >> c, heap.push(c), sum -= c;
        if (sum < 0) {
            sum += heap.top(), heap.pop();
        }
        sum += x;
    }
    cout << heap.size() << '\n';
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