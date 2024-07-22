摘要：双指针（扩展），前缀和

[传送门：https://www.luogu.com.cn/problem/CF1983C](https://www.luogu.com.cn/problem/CF1983C)

## 题意

给定长度为 $n$ 的序列 $a_n, b_n, c_n$，保证 $tot = \sum a_i = \sum b_i = \sum c_i$。现在求是否存在三个不交的子段 $[l_a, r_a], [l_b, r_b], [l_c, r_c]$，使得 $\sum_{i = l_a} ^ {r_a} a_i, \sum_{i = l_b} ^ {r_b} b_i, \sum_{i = l_c} ^ {r_c} c_i \ge \lceil \frac{tot}{3} \rceil$？若存在，输出一组符合条件的构造。若不存在，输出 $-1$。

## 分析思路

对于每个数列 $A = a, b, c$，我们可以使用双指针和前缀和，对每个 $l$ 求出满足 $\sum_{i = l}^r A_i \ge \lceil \frac{tot}{3} \rceil$ 的最小的 $r$（为了尽可能保证不交）。由此对每个序列，我们可以处理出 $O\left(n\right)$ 个区间。最后的任务即为在则三个序列处理出的区间中选出三个，满足它们不交。

我们使用**扩展双指针**。假定我们已经知道 $l_a \le r_a < l_b \le r_b < l_c \le r_c$。从预处理的结果中枚举 $(l_a, r_a)$，再在预处理出的区间中找出最小的 $l_b$ 和最小的 $l_c$ 满足上述先后关系。若存在解即可在 $O\left(n\right)$ 的时间复杂度内解决本问题。

然而我们并不知道 $l_a, l_b, l_c$ 分别的先后顺序，于是可以枚举长度为 $3$ 的一个排列表示先后。若对于所有的先后顺序都无解，则输出 `-1`。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
const int PEOPLE = 3;
using i64 = long long;
template <class T>
class Sum {
private:
    size_t n;
    std::vector<T> sum;

public:
    Sum(void) : n(0) {}
    template <class InitT>
    Sum(std::vector<InitT> &init) { _init(init); }
    template <class InitT>
    inline void _init(std::vector<InitT> &init) {
        if (init.empty()) return;
        sum.resize(n = init.size()), sum[0] = init[0];
        for (size_t i = 1; i < n; i++) {
            sum[i] = sum[i - 1] + init[i];
        }
    }
    inline T query(size_t l, size_t r) {
        return l == 0 ? sum[r] : sum[r] - sum[l - 1];
    }
};
inline void solve(void) {
    int n;
    array<vector<int>, PEOPLE> a;
    array<vector<pair<int, int>>, PEOPLE> segs;
    cin >> n, a.fill(vector<int>(n + 1, 0));
    for (int i = 0; i < PEOPLE; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> a[i][j];
        }
        Sum<i64> S(a[i]);
        i64 req = (S.query(1, n) + PEOPLE - 1) / PEOPLE;
        for (int l = 1, r = 1; l <= n; l++, r = max(l, r)) {
            while (r <= n && S.query(l, r) < req) r++;
            if (S.query(l, min(n, r)) >= req) {
                segs[i].push_back({l, min(n, r)});
            } else {
                break;
            }
        }
    }
    vector<int> p(PEOPLE);
    iota(p.begin(), p.end(), 0);
    // 找到三段不交的区间，使得 A[la, ra], B[lb, rb], C[lc, rc] >= (tot + 2) / 3
    do {
        vector<size_t> it(PEOPLE, 0);
        auto Seg = [&](int i) { return segs[p[i]][it[i]]; };
        for (; it[0] < segs[p[0]].size(); it[0]++) {
            bool ok = true;
            for (int i = 1; i < PEOPLE; i++) {
                while (it[i] < segs[p[i]].size() && Seg(i).first <= Seg(i - 1).second) it[i]++;
                if (it[i] == segs[p[i]].size() || Seg(i).first <= Seg(i - 1).second) {
                    ok = false;
                    break;
                }
            }
            if (!ok) break;
            vector<pair<int, int>> pp;
            for (int i = 0; i < PEOPLE; i++) {
                pp.push_back({p[i], it[i]});
            }
            sort(pp.begin(), pp.end());
            for (auto &x : pp) {
                cout << segs[x.first][x.second].first << ' '
                     << segs[x.first][x.second].second << " \n"[x == pp.back()];
            }
            return;
        }
    } while (std::next_permutation(p.begin(), p.end()));
    cout << "-1\n";
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int t = 1;
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
