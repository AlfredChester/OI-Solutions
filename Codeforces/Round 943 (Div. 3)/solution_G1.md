摘要：二分，字符串哈希，记忆化

[Easy version：https://www.luogu.com.cn/problem/CF1968G1](https://www.luogu.com.cn/problem/CF1968G1)

[Hard version：https://www.luogu.com.cn/problem/CF1968G2](https://www.luogu.com.cn/problem/CF1968G2)

## 题意

给定一个字符串 $s$，对于一个正整数 $k$，我们考虑将 $s$ 分成 $k$ 个连续的子串 $w_1, \cdots, w_k$。定义 $f_k$ 是在所有分割方案中最大的 $LCP\left(w_1, \cdots, w_k\right)$，即 $w_1, \cdots, w_k$ 的最长公共前缀的长度。

现在，给定一个范围 $[l, r]$，求 $f_l, f_{l + 1}, \cdots, f_r$。

## 分析思路

本题解中 $s$ 的下标从 $1$ 开始。

### I. Easy version

首先我们来考察 `Easy version`，即 $k = l = r$ 的情况。

我们考虑枚举最长公共前缀的长度 $len$。注意到 $len$ 变大时，在 $s$ 中的出现次数是不增的。也就是说，为了使每一段都有长为 $len$ 的前缀，相应分割的段数就会减少。于是我们考虑在 $[0, \frac{n}{k}]$ 上二分 $len$。

那么，如何检验 $len$ 是否合法呢？我们需要贪心地从第一个位置开始匹配与 $s[1:len]$ 相等的不交的子串个数。考虑用字符串哈希进行快速判断两段子串是否相等。最后只要个数大于等于 $k$ 即可。

于是我们用 $O\left(n \log n\right)$ 的算法解决了 `Easy version`。 

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
int t = 1, n, l, r;
inline void solve(void) {
    std::string S;
    cin >> n >> l >> r >> S;
    HashedString H(S);
    auto check = [&](int len, int k) {
        int cnt = 1;
        for (int i = len; i + len - 1 < n; i++) {
            if (H.getHash(0, len - 1) == H.getHash(i, i + len - 1)) {
                ++cnt, i = i + len - 1;
            }
        }
        return cnt >= k;
    };
    int L = 0, R = n / l, mid;
    while (L < R) {
        mid = (L + R + 1) >> 1;
        if (check(mid, l)) {
            L = mid;
        } else {
            R = mid - 1;
        }
    }
    cout << L << '\n';
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