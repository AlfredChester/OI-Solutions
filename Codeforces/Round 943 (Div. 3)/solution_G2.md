摘要：二分，字符串哈希，记忆化

[Easy version：https://www.luogu.com.cn/problem/CF1968G1](https://www.luogu.com.cn/problem/CF1968G1)

[Hard version：https://www.luogu.com.cn/problem/CF1968G2](https://www.luogu.com.cn/problem/CF1968G2)

本题解 G1，G2 虽然写在了一起，但是有 G1 向 G2 转化的过程，求管理大大通过 QAQ。

## 题意

给定一个字符串 $s$，对于一个正整数 $k$，我们考虑将 $s$ 分成 $k$ 个连续的子串 $w_1, \cdots, w_k$。定义 $f_k$ 是在所有分割方案中最大的 $LCP\left(w_1, \cdots, w_k\right)$，即 $w_1, \cdots, w_k$ 的最长公共前缀的长度。

现在，给定一个范围 $[l, r]$，求 $f_l, f_{l + 1}, \cdots, f_r$。

## 分析思路

本题解中 $s$ 的下标从 $1$ 开始。

### I. Easy version

首先我们来考察 `Easy version`，即 $k = l = r$ 的情况。

我们考虑枚举最长公共前缀的长度 $len$。注意到 $len$ 变大时，在 $s$ 中的出现次数是不增的。也就是说，为了使每一段都有长为 $len$ 的前缀，相应分割的段数就会减少。于是我们考虑在 $[0, \frac{n}{k}]$ 上二分 $len$。

那么，如何检验 $len$ 是否合法呢？我们需要贪心地从第一个位置开始匹配与 $s[1:len]$ 相等的不交的子串个数。考虑用字符串哈希进行快速判断两段子串是否相等。最后只要个数大于等于 $k$ 即可。

于是我们用 $O\left(n \log n\right)$ 的算法解决了 `Easy version`。代码见 [Easy version 题解](https://www.luogu.com.cn/article/52ytd5wy)。

### II. Hard version

接下来解决 $l \leq r $ 的情形。这里我们讨论 $l = 1, r = n$ 如何解决。

我们考虑对 $k$ 的大小进行分类讨论，分为 $k < \sqrt n$ 的情况与 $k \geq \sqrt n$ 的情况。（这一步是为了最终时间复杂度的正确）

#### 1. $k < \sqrt n$

此时我们沿用 `Easy version` 的做法，暴力枚举 $k$，并且进行二分即可，时间复杂度 $O\left(n \sqrt n \log n\right)$。

#### 2. $ k \geq \sqrt n$

由于 $k \geq \sqrt n, f_k \cdot k \leq n$，我们有 $f_k \leq \sqrt n$。考虑枚举最终 $LCP$ 的长度 $L$。对于每个 $L$，我们能沿用二分的 $check$，在 $O\left(n\right)$ 的时间复杂度内求得其对应的最大的 $k$。这一部分的总时间复杂度 $O\left(n \sqrt n\right)$。

因为在第二部分中，我们找到的是 $L$ 对应的最大的 $k$，所以我们还需要从后往前扫一遍更新答案为更大值。

综上，总时间复杂度为 $O\left(n \sqrt n \log n\right)$，可以通过 `Hard version`。

需要注意的是，字符串哈希有许多取模和减法操作，常数较大。考虑对 `check` 进行记忆化以及减小枚举的范围可以有效减小常数。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using i128 = __int128;
class HashedString {
private:
    // change M and B if you want
    static const i64 M = (1LL << 61) - 1;
    static const i64 B;
    // pow[i] contains B^i % M
    static vector<i64> pow;

    // p_hash[i] is the hash of the first i characters of the given string
    vector<i64> r_hash, p_hash;
    i128 mul(i64 a, i64 b) { return (i128)a * b; }
    i64 mod_mul(i64 a, i64 b) { return mul(a, b) % M; }

public:
    HashedString(const string &s) : r_hash(s.size() + 1), p_hash(s.size() + 1) {
        while (pow.size() < s.size()) { pow.push_back(mod_mul(pow.back(), B)); }
        p_hash[0] = 0;
        r_hash[0] = 0;
        for (size_t i = 0; i < s.size(); i++) {
            p_hash[i + 1] = (mul(p_hash[i], B) + s[i]) % M; // 1-based
        }
        i64 sz = s.size();
        for (int i = sz - 1, j = 0; i >= 0; i--, j++) {
            r_hash[j + 1] = (mul(r_hash[j], B) + s[i]) % M;
        }
    }
    i64 getHash(int start, int end) { // 0 based
        i64 raw_val = p_hash[end + 1] - mod_mul(p_hash[start], pow[end - start + 1]);
        return (raw_val + M) % M;
    }
    i64 getRHash(int start, int end) { // 0 based
        i64 raw_val = r_hash[end + 1] - mod_mul(r_hash[start], pow[end - start + 1]);
        return (raw_val + M) % M;
    }
};
mt19937 rng((uint32_t)chrono::steady_clock::now().time_since_epoch().count());
vector<i64> HashedString::pow = {1};
const i64 HashedString::B = uniform_int_distribution<i64>(0, M - 1)(rng);
int t, n, l, r;
inline void solve(void) {
    std::string S;
    cin >> n >> l >> r >> S;
    HashedString H(S);
    vector<int> ans(n + 1);
    vector<int> mem(n + 1, -1);
    const int SQ = ceil(sqrt(n));
    auto check = [&](int len) -> int {
        int cnt = 1;
        if (len == 0) return 1e9;
        if (mem[len] != -1) return mem[len];
        auto F = H.getHash(0, len - 1);
        for (int i = len; i + len - 1 < n; i++) {
            if (F == H.getHash(i, i + len - 1)) {
                ++cnt, i = i + len - 1;
            }
        }
        return mem[len] = cnt;
    };
    int lst = n;
    for (int k = l; k <= SQ; k++) {
        int L = 0, R = min(lst, n / k), mid;
        while (L < R) {
            mid = (L + R + 1) >> 1;
            check(mid) >= k ? L = mid : R = mid - 1;
        }
        ans[k] = L, lst = R;
    }
    for (int len = 1; len <= SQ; len++) {
        int k = check(len);
        if (k < l) break;
        ans[k] = max(ans[k], len);
    }
    for (int i = n - 1; i >= l; i--) {
        ans[i] = max(ans[i], ans[i + 1]);
    }
    for (int i = l; i <= r; i++) {
        cout << ans[i] << ' ';
    }
    cout << '\n';
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