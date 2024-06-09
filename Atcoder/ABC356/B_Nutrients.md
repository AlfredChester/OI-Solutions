摘要：模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc356_b](https://www.luogu.com.cn/problem/AT_abc356_b)

## 题意

小明要摄入 $M$ 种营养成分，第 $i$ 种营养成分需要总共输入 $A_i$ 个单位。今天他吃了 $N$ 种食物，第 $i$ 种食物可以提供 $X_{i, j}$ 个单位的营养成分 $j$。问小明是否达成了他摄入营养成分的目标？

## 分析思路

按题意模拟即可。我们累积某种食物小明实际摄入了多少个单位，再与需求比较即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 n, m, a[110], sum[110], X;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= m; i++) cin >> a[i];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> X, sum[j] += X;
        }
    }
    for (int i = 1; i <= m; i++) {
        if (sum[i] < a[i]) {
            cout << "No\n";
            return 0;
        }
    }
    cout << "Yes\n";
    return 0;
}

```
