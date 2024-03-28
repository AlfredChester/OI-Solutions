摘要：数论、欧拉函数、最大公约数。

[传送门: https://www.luogu.com.cn/problem/P2303](https://www.luogu.com.cn/problem/P2303)

## 题意

给定整数 $n$，求 $\displaystyle \sum_{i=1}^n \gcd(i, n)$。

## 分析思路

注意到在 $n$ 以内不同的 $\gcd(i, n)$ 的个数为 $n$ 的约数的个数。考虑对原式进行转化。

$$
\begin{aligned}
\sum_{i=1}^n \gcd(i, n) &= \sum_{j \mid n} j\sum_{i=1}^{\frac{n}{j}} [\gcd (i, \frac{n}{j}) = 1] \\
&= \sum_{j \mid n} j \times \varphi (\frac{n}{j})
\end{aligned}
$$

单点 $O\left(\sqrt x\right)$ 求 $\varphi(x)$ 即可，时间复杂度的上界为 $O\left(\mathrm{d}(n) \times \sqrt n\right)$。

（好诶自己想出来的数论题，可惜求 $\varphi(x)$ 板子不大熟 QAQ）

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
inline ll phi(ll x) {
    vector<ll> p;
    ll i = 2, n = x;
    for (; i * i <= x; i++) {
        if (n % i == 0) p.push_back(i);
        while (n % i == 0) n /= i;
    }
    if (n != 1) p.push_back(n);
    for (auto y : p) x -= x / y;
    return x;
}
ll n, ans;
int main(int argc, char const *argv[]) {
    cin >> n;
    vector<ll> fact;
    for (ll i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            fact.push_back(i);
            if (n != i * i) fact.push_back(n / i);
        }
    }
    // 只需要统计在 n / f 范围内，与n / f 互素的数字的个数
    // 即 phi(n / f)
    for (auto f : fact) {
        ans += f * phi(n / f);
    }
    cout << ans << endl;
    return 0;
}

```