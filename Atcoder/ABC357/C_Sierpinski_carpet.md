摘要：分治，模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc357_c](https://www.luogu.com.cn/problem/AT_abc357_c)

## 题意

对于自然数 $K$，我们定义 $K$ 阶地毯如下：

- $0$ 阶地毯仅包含一个黑色单元格，用 `#` 代表；
- $K$（$K$ 为正整数）阶地毯四周由 $K - 1$ 阶地毯组成，中间填充白色单元格，用 `.` 代表。

给定正整数 $N$，输出 $N$ 阶地毯。  

## 分析思路

典型的分治问题。我们定义 `solve(x, y, k)` 可以以 $(x, y)$ 为左上角，填充 $k$ 阶地毯。显然 $k=0$ 时是一个边界情况，直接填充黑色即可。 

$k > 0$ 时，我们可以拆分为八个以不同的点为左上角，填充 $k - 1$ 阶地毯的问题，中间模拟填充白色即可。

时间复杂度 $O\left(3^{2N} \times N\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int n;
char ans[1010][1010];
inline void fill(int x, int y, int k) {
    int len = pow(3, k);
    for (int i = 0; i < len; i++) {
        for (int j = 0; j < len; j++) {
            ans[x + i][y + j] = '.';
        }
    }
}
inline void solve(int x, int y, int k) {
    if (k == 0) {
        ans[x][y] = '#';
        return;
    }
    int len = pow(3, k - 1);
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (i == 1 && j == 1) {
                fill(x + len, y + len, k - 1);
            } else {
                solve(x + i * len, y + j * len, k - 1);
            }
        }
    }
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    solve(1, 1, n);
    const int x = pow(3, n);
    for (int i = 1; i <= x; i++) {
        for (int j = 1; j <= x; j++) {
            cout << ans[i][j];
        }
        cout << '\n';
    }
    return 0;
}
```
