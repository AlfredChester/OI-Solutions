摘要：构造

[传送门：https://www.luogu.com.cn/problem/CF1986E](https://www.luogu.com.cn/problem/CF1986E)

## 题意

给定一个参数 $n$。你有一个 $n \times n$ 的矩阵，试在该矩阵上构造 $n$ 个点 $(x_1, y_1), (x_2, y_2), \cdots, (x_n, y_n)$，使得他们两两的曼哈顿距离构成的集合 $H$ 最大。

## 分析思路

乍一看似乎没什么思路，[手玩](https://www.luogu.com.cn/paste/vxrbq433)几组样例试试看。

对于 $n = 3$，样例是一组构造，$H$ 的大小为 $4$，下文讨论 $n > 3$ 的情况。

注意到对于每个 $n$，$H$ 的大小为 $2 \times n - 1$，也就是能构造出在 $n \times n $ 的矩阵里所有的曼哈顿距离。

由于我们要构造出曼哈顿距离 $1$，则必须有两个贴在一起。不妨令他们是 $(1, 1)$ 与 $(2, 1)$。

剩余的点，经过暴力打表可以发现安排在 $y = x$ 的对角线上是合法的构造。

原理在于，对于 $n > 3$，至少有两个除 $(1, 1)$ 以外的点被安排在了对角线上，这些点之间可以构造出曼哈顿距离 $2$。

然后对于在第 $i$ （$i \geq 3$）行的被安排在对角线上的点 $(i, i)$，注意到它与 $(1, 1)$ 构造出了 $2i - 2$，与 $(2, 1)$ 构造出了 $2i - 3$。我们将其罗列可以发现会构造出 $\{3, 4, \cdots, 2\times n - 2\}$，再加上之前构造出的曼哈顿距离 $\{0, 1, 2\}$，最终构造出的 $H$ 的大小就是 $2 \times n - 1$，符合结论。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t, n;
inline void solve(void) {
    cin >> n, cout << "1 1\n2 1\n";
    for (int i = 3; i <= n; i++) {
        cout << i << ' ' << i << '\n';
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
