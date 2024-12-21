摘要：欧拉筛，欧拉函数，前缀和

[传送门：https://www.luogu.com.cn/problem/P2398](https://www.luogu.com.cn/problem/P2398)

## 题意

给定 $n$，求：

$$
S = \sum_{i = 1}^n \sum_{j = 1}^n \gcd(i, j)
$$

的值。

## 分析思路

$$
\begin{aligned}
S &= \sum_{i = 1}^n \sum_{j = 1}^n \gcd(i, j) \\
&= \sum_{g = 1}^{n} g \sum_{p = 1}^{\lfloor \frac{n}{g} \rfloor} \sum_{q = 1}^{\lfloor \frac{n}{g} \rfloor} \left[ p \bot q \right] \\
&= \sum_{g = 1}^{n} 2g \sum_{p = 1}^{\lfloor \frac{n}{g} \rfloor} \varphi(p) - 1
\end{aligned}
$$

其中最后的 $-1$ 是因为 $(1, 1)$ 被重复计算。

欧拉筛可以 $O(n)$ 求欧拉函数前缀和，于是本题时间复杂度 $O(n)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
vector<int> primes, minp;
long long n, phi[N], s[N];
inline void sieve(void) {
    minp.assign(N, 0), phi[1] = 1;
    for (int i = 2; i < N; i++) {
        if (minp[i] == 0) {
            primes.push_back(i);
            minp[i] = i, phi[i] = i - 1;
        }
        for (auto &p : primes) {
            if (i * p >= N) break;
            minp[i * p] = p, phi[i * p] = phi[i] * phi[p];
            if (i % p == 0) {
                phi[i * p] = phi[i] * p;
                break;
            }
        }
    }
}
int main(int argc, char const *argv[]) {
    sieve(), cin >> n, s[1] = 1;
    for (int i = 2; i <= n; i++) {
        s[i] = s[i - 1] + 2 * phi[i];
    }
    long long ans = 0;
    for (int g = 1; g <= n; g++) {
        ans += g * s[n / g];
    }
    cout << ans << endl;
    return 0;
}

```
