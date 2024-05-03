摘要：贪心，字符串匹配

[传送门：https://www.luogu.com.cn/problem/CF1968B](https://www.luogu.com.cn/problem/CF1968B)

## 题意

给定两个由 `0` 和 `1` 组成的字符串 $a, b$。找到最大的 $k$，使得长度为 $k$ 的 $a$ 的前缀为 $b$ 的子序列。

子序列的定义：若序列 $a$ 删除任意多个（可能删除 $0$ 个或删除全部）元素之后变为了序列 $b$，则 $b$ 是 $a$ 的子序列。

## 分析思路

考虑贪心匹配。我们使用两个指针 $x$ 和 $y$ 分别表示 $a$ 与 $b$ 已经匹配到的位置。对于枚举到的每个位置 $x$，一直右移 $y$ 直到 $a_x$ 与 $b_y$ 匹配或 $y$ 越界。最终 $x$ 的值即为答案。

时间复杂度 $O\left(n + m\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t = 1, n, m;
inline void solve(void) {
    string a, b;
    cin >> n >> m >> a >> b;
    int k = 0, beg = 0;
    while (beg < m) {
        if (k < n && a[k] == b[beg]) k++;
        beg++;
    }
    cout << k << '\n';
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
