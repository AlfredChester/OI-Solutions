摘要：位运算，模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc356_c](https://www.luogu.com.cn/problem/AT_abc356_c)

## 题意

你有 $N$ 把钥匙，其中有一些真钥匙和假钥匙。现在有一扇神秘门，当且仅当插入了至少 $K$ 把真钥匙，这扇门才会打开。

现在给定了 $M$ 组钥匙，第 $i$ 组钥匙有 $C_i$ 个，分别编号为 $A_{i, 1}, A_{i, 2}, \cdots, A_{i, C_i}$。开门的结果为 $R_i$，它是一个字符，`x` 表示没有打开，`o` 表示打开。

现在问可能的真钥匙的组合有多少种？

## 分析思路

注意到 $N$ 很小，考虑直接枚举真钥匙的状态 $i$。对于第 $j$ 组钥匙我们可以压缩成二进制状态 $stat_j$。则 $i \& stat_j$ 就是在第 $j$ 组钥匙中真钥匙的状态。最后我们比较 $\mathrm{popcount}(i \& j)$ 与 $K$ 是否符合 $R_j$ 即可。

时间复杂度 $O\left(2^N M\right)$，可以通过本题。

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
