摘要：贪心，模拟

[传送门：https://www.luogu.com.cn/problem/CF1931B](https://www.luogu.com.cn/problem/CF1931B)

## 题意

给定一排 $n$ 个水桶，从左到右依次编号 $1$ 到 $n$。初始第 $i$ 个水桶有 $a_i$ 升水（$n \mid \sum^n_{i=1} a_i$），每次你可以选择两个编号为 $i$、$j$（$1 \leq i < j \leq n$）的容器，将第 $i$ 个容器里的任意多的水倒到第 $j$ 个容器中，问最终能否使得每个桶里都有相同多的水。

## 分析思路

此题类似[[NOIP2002 提高组] 均分纸牌](https://www.luogu.com.cn/problem/P1031)。注意到最终每桶水的多少是确定的，即 $\overline{a}$，考虑贪心。从左到右对 $a_i$ 进行扫描。

如果当前的 $a_i$ 大于等于 $\overline{a}$，则将多出来的部分（即 $a_i - \overline{a}$）加给 $a_{i+1}$；

否则，在 $i$ 之前所有的 $a_j$ 均已变为 $\overline{a}$，无法再使得 $a_i$ 变为 $\overline{a}$，则无解。

注意此处的 $i$、$j$ 与题面中的意义不同。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000010;
typedef long long ll;
ll t, n, sum, a[N];
inline void solve(void) {
    cin >> n, sum = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], sum += a[i];
    }
    sum /= n;
    for (int i = 1; i < n; i++) {
        if (a[i] < sum) {
            cout << "NO\n";
            return;
        }
        a[i + 1] += a[i] - sum;
    }
    cout << "YES\n";
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