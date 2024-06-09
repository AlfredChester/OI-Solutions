摘要：位运算，数学

[传送门：https://www.luogu.com.cn/problem/AT_abc356_d](https://www.luogu.com.cn/problem/AT_abc356_d)

## 题意

给定两个整数 $N, M$，求 $\sum_{k=0}^N \mathrm{popcount (k\space\&\space M)} \bmod {998244353}$。

其中 $\mathrm{popcount}(x)$ 表示 $x$ 在二进制表示下 $1$ 的个数，$x \space \& \space y$ 表示二进制与运算。

## 分析思路



## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 110;
char res[N];
int n, m, k, ans, c[N], A, stat[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m >> k;
    for (int i = 1; i <= m; i++) {
        cin >> c[i];
        for (int j = 1; j <= c[i]; j++) {
            cin >> A, stat[i] |= 1 << (A - 1);
        }
        cin >> res[i];
    }
    auto check = [&](int i) -> bool {
        for (int j = 1; j <= m; j++) {
            int cur = __builtin_popcount(i & stat[j]);
            if ((cur >= k && res[j] == 'x') ||
                (cur < k && res[j] == 'o')) {
                return false;
            }
        }
        return true;
    };
    for (int i = 0; i < (1 << n); i++) {
        ans += check(i);
    }
    cout << ans << endl;
    return 0;
}

```
