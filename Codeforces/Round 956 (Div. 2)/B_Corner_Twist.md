摘要：数学

[传送门：https://www.luogu.com.cn/problem/CF1983B](https://www.luogu.com.cn/problem/CF1983B)

## 题意

给定两个长为 $n$，宽为 $m$ 的初始矩阵 $a$ 和目标矩阵 $b$（$0 \le a_{i, j}, b_{i, j} \le 2$）。你可以进行若干次操作。每次操作，你可以选择一个长宽都大于等于 $2$ 的一个 $a$ 的子矩阵，选择四个角中任意两个不相邻的角并将它们的值加 $1$ 模 $3$。同时对于剩下的两个角，将它们的值加 $2$ 模 $3$。问是否可能进行若干次操作，使得初始矩阵 $a$ 变为目标矩阵 $b$？若可以输出 `YES`，否则输出 `NO`。

## 分析思路

我们注意到无论怎么操作，每一行和每一列的和模 $3$ 的余数不变。所以 $a$ 和 $b$ 每一行和每一列的和相同是一个必要条件。下证这也是一个充分条件。

注意到任何大小的操作都可以由 $2 \times 2$ 的基础操作得到，如：

$$
\left[ \begin{array}{cc} 1 & 2 \\ 2 & 1  \end{array}\right] + \left[ \begin{array}{cc} 1 & 2 \\ 2 & 1  \end{array}\right] = \left[ \begin{array}{cc} 1 & 2 + 1 & 2 \\ 2 & 1 + 2 & 1 \end{array}\right] \equiv \left[ \begin{array}{cc} 1 & 0 & 2 \\ 2 & 0 & 1 \end{array}\right] \pmod 3
$$

那我们可以贪心地进行如下操作：

- 若 $a_{i, j} + 1 \equiv b_{i, j} \pmod 3$，则以 $a_{i, j}$ 为左上角叠加 $\left[ \begin{array}{cc} 1 & 2 \\ 2 & 1  \end{array}\right]$；
- 若 $a_{i, j} + 2 \equiv b_{i, j} \pmod 3$，则以 $a_{i, j}$ 为左上角叠加 $\left[ \begin{array}{cc} 2 & 1 \\ 1 & 2  \end{array}\right]$；
- 若 $a_{i, j} \equiv b_{i, j} \pmod 3$，不进行任何操作。

容易发现这样总能使得前 $n - 1$ 行中的前 $m - 1$ 列保持相等。由于上述操作之后，每一行和每一列的和模 $3$ 都不变，剩下 $n + m - 1$ 个元素的值可以通过之前每一行和每一列的和唯一确定，而它们要想等，所以这是一个充分条件。

时间复杂度 $O\left(nm\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 510;
char a[N][N], b[N][N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void solve(void) {
    int n, m;
    cin >> n >> m;
    vector<int> sal(n), sbl(n);
    vector<int> sac(m), sbc(m);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> a[i][j];
            sal[i] += a[i][j] ^ 48;
            sac[j] += a[i][j] ^ 48;
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> b[i][j];
            sbl[i] += b[i][j] ^ 48;
            sbc[j] += b[i][j] ^ 48;
        }
    }
    for (int i = 0; i < n; i++) {
        if (sal[i] % 3 != sbl[i] % 3) {
            cout << "NO\n";
            return;
        }
    }
    for (int i = 0; i < m; i++) {
        if (sac[i] % 3 != sbc[i] % 3) {
            cout << "NO\n";
            return;
        }
    }
    cout << "YES\n";
}
int main(int argc, char const *argv[]) {
    int t = 1;
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
