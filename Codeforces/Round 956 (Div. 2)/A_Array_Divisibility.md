摘要：构造，数学

[传送门：https://www.luogu.com.cn/problem/CF1983A](https://www.luogu.com.cn/problem/CF1983A)

感谢这场让我 $+149$。

## 题意

如果一个长度为 $n$ 的整数数列 $a_n$ 满足 $\forall k \in \{1, 2, \dots, n\}, \sum_{k|i} a_i \equiv 0 \pmod k$，那么称这个数列是美丽的。给定数列的长度 $n$，请你构造出每个元素都不超过 $10^5$ 的一个美丽的数列。

## 分析思路

容易注意到一个简单的构造，即 $a_i = i$。可以发现符合题目描述。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, n;
inline void solve(void) {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cout << i << " \n"[i == n];
    }
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
