摘要：模拟

[传送门：https://www.luogu.com.cn/problem/CF1974B](https://www.luogu.com.cn/problem/CF1974B)

## 题意

定义一种对字符串 $s$ 的加密方法。

1. 定义一个辅助字符串 $r$，由字符串 $s$ 内所有不同的字母组成，按照字母顺序排序。
2. 将 $s$ 内的每个字符替换为其在 $r$ 中出现位置对称的字符。

现在已知加密后的字符串 $s'$，求原字符串 $s$。

## 分析思路

注意到该加密方法的一个性质：出现的字符种类不变，即如果对加密后的字符串 $s'$ 构造辅助串 $r'$，有 $r = r'$。

由此，我们注意到，对 $s'$ 再进行一次加密，在 $r'$ 中对称的字符组没有变化。原先交换的字符会被交换回来，所以只要模拟对 $s'$ 进行一次加密即可。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, n;
inline void solve(void) {
    string b, r;
    cin >> n >> b;
    vector<int> vis(26, -1);
    for (int i = n - 1; i >= 0; i--) {
        if (vis[b[i] - 'a'] == -1) {
            r += b[i], vis[b[i] - 'a'] = 1;
        }
    }
    sort(r.begin(), r.end());
    for (size_t i = 0; i < r.size(); i++) {
        vis[r[i] - 'a'] = i;
    }
    auto get = [&](char c) {
        return r[r.size() - 1 - vis[c - 'a']];
    };
    for (auto c : b) cout << get(c);
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
