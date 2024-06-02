# Codeforces Round 925 (Div. 3) C - Make Equal Again 题解

## 题意

给你一个数组 $a_n$。你可以选择三个参数 $l, r, v$（$1\leq l \leq r \leq n$），将 $a_l, a_{l + 1}, \cdots, a_r$ 赋值为 $v$；亦或是直接不操作。定义赋值操作的代价为 $r-l+1$，不操作的代价为 $0$。问使得 $a_i$ 两两相同所需要的最小代价。

## 分析思路

考虑贪心。首先统计出最左边的连续段长度 $L$ 和最右边的连续段长度 $R$。例如对于 $a = [8, 8, 8, 1, 1, 4, 6, 6]$，有 $L = 3, R = 2$。接下来分情况讨论。

- 若 $L = R = n$，则说明整个数组一开始就完全相同，输出 $0$；
- 否则若 $a_1 = a_n$，则左右两端连续段可以共存，只需要更改 $a_{L+1}, a_{L+2}, \cdots, a_{R-1}$ 为 $a_1$（也就是$a_n$）即可，答案为 $n - L - R$；
- 否则，选择 $\max(L, R)$ 保留即可，答案为 $n - \max(L, R)$。

总时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000010;
typedef long long ll;
ll t, n, a[N];
inline ll cntLeft(void) {
    for (int i = 1; i <= n; i++) {
        if (a[i] != a[1]) return i - 1;
    }
    return n;
}
inline ll cntRight(void) {
    for (int i = n; i >= 1; i--) {
        if (a[i] != a[n]) return n - i;
    }
    return n;
}
inline void solve(void) {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    ll L = cntLeft(), R = cntRight();
    if (L == n) cout << "0\n";
    else if (a[1] == a[n]) {
        cout << n - L - R << '\n';
    } else {
        cout << n - max(L, R) << '\n';
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
