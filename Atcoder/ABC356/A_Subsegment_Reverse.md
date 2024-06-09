摘要：模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc356_a](https://www.luogu.com.cn/problem/AT_abc356_a)

## 题意

给定一个 $1$ 到 $N$ 的排列 $A_N$，现在请你反转 $A_L, A_{L+1}, \cdots, A_R$，并输出 $A$。

## 分析思路

按题意模拟即可，使用 `std::iota` 和 `std::reverse` 可以比较便捷地实现。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, l, r, a[110];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> l >> r;
    iota(a + 1, a + 1 + n, 1);
    reverse(a + l, a + r + 1);
    for (int i = 1; i <= n; i++) {
        cout << a[i] << " \n"[i == n];
    }
    return 0;
}

```
