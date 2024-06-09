摘要：模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc357_a](https://www.luogu.com.cn/problem/AT_abc357_a)

## 题意

有 $N$ 个外星人依次来洗手，第 $i$ 个外星人有 $H_i$ 只手。现在有足够能洗 $M$ 只手的洗手液，求有多少外星人能把它的手全部洗一遍。

## 分析思路

依照题意模拟即可。若当前洗手液的余量不足以支持第 $i$ 个外星人洗手，输出 $i - 1$。不要忘记最后如果全部能吸收要输出 $N$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, m, h[110];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> h[i];
        if (m - h[i] < 0) {
            cout << i - 1 << '\n';
            return 0;
        }
        m -= h[i];
    }
    cout << n << '\n';
    return 0;
}

```
