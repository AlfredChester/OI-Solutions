摘要：构造

[传送门：https://www.luogu.com.cn/problem/CF1974D](https://www.luogu.com.cn/problem/CF1974D)

## 题意

你有两个机器人，分别是 `Rover (R)` 和 `Helicopter (H)`。它们初始都在同一平面直角坐标系 $xOy$ 的 $(0, 0)$ 处。定义北为 $y$ 轴正方向，东为 $x$ 轴正方向。

现有一串由以下四个字符指令组成的指令串 $s$：

- `N` 向北移动一步，即 $(x, y) \to (x, y + 1)$；
- `S` 向南移动一步，即 $(x, y) \to (x, y - 1)$；
- `E` 向东移动一步，即 $(x, y) \to (x + 1, y)$；
- `W` 向西移动一步，即 $(x, y) \to (x - 1, y)$；

问是否存在一种将每个指令都分配给两个机器人的方法，使得它们最终停在同一个位置？如果存在，给出一组构造；如果不存在，报告无解。

## 分析思路

我们分别分析两者在 $x$ 方向的增量 $\Delta x_R, \Delta x_H$ 和两者在 $y$ 方向的增量 $\Delta y_R, \Delta y_H$。

因为最终有 $\Delta x_R = \Delta x_H, \Delta y_R = \Delta y_H$，则两者在两个方向上增量的奇偶性相同（感性理解下）。换句话说，我们有 $\Delta x_R + \Delta x_H \equiv 0 \pmod 2; \Delta y_R + \Delta y_H \equiv 0 \pmod 2$。

于是，首先我们统计两个方向上总的增量个数，依据奇偶性可以初步判断无解。

然后我们考虑构造，现在以两个 $x$ 方向的操作 `E, W` 为例。我们记两个指令的个数分别为 $cnt_E, cnt_W$。

### 1. $cnt_E \equiv 1 \pmod 2$

此时有 $cnt_W \equiv 1 \pmod 2$。我们考虑从 `E` 中拿出一部分 $\frac{cnt_E - 1}{2}$，从 `S` 中拿出一部分 $\frac{cnt_W - 1}{2}$。此时我们有：

$$
\frac{cnt_E - 1}{2} - \frac{cnt_W - 1}{2} = \left(cnt_E - \frac{cnt_E - 1}{2}\right) - \left(cnt_W - \frac{cnt_W - 1}{2}\right)
$$

所以把这两部分分别分给两人即可。

### 2. $cnt_E \equiv 0 \pmod 2$

此时有 $cnt_W \equiv 0 \pmod 2$。我们考虑从 `E` 中拿出一部分 $\frac{cnt_E}{2}$，从 `S` 中拿出一部分 $\frac{cnt_W}{2}$。此时我们有：

$$
\frac{cnt_E}{2} - \frac{cnt_W}{2} = \left(cnt_E - \frac{cnt_E}{2}\right) - \left(cnt_W - \frac{cnt_W}{2}\right)
$$

所以把这两部分分别分给两人即可。最后判断一下是否都分给了一个人即可

$y$ 方向上的分配同理。注意“分别”的两人在 $x$ 轴和 $y$ 轴的分配中要调换一下，否则可能出现误判为无解的情况。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const string d = "EWNS";
int t, n;
inline void solve(void) {
    string s;
    cin >> n >> s;
    vector<int> pos[4];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 4; j++) {
            if (s[i] == d[j]) {
                pos[j].push_back(i);
            }
        }
    }
    if ((pos[0].size() + pos[1].size()) & 1 ||
        (pos[2].size() + pos[3].size()) & 1) {
        cout << "NO\n";
        return;
    }
    string ans(n, 'R');
    for (int i = 0; i < 4; i += 2) {
        if (pos[i].size() > pos[i + 1].size()) {
            std::swap(pos[i], pos[i + 1]);
        }
        int half1 = pos[i].size() >> 1;
        int half2 = pos[i + 1].size() >> 1;
        for (int j = 0; j < half1; j++) {
            ans[pos[i][j]] = (i ? 'H' : 'R');
        }
        for (int j = half1; j < pos[i].size(); j++) {
            ans[pos[i][j]] = (i ? 'R' : 'H');
        }
        for (int j = 0; j < half2; j++) {
            ans[pos[i + 1][j]] = (i ? 'H' : 'R');
        }
        for (int j = half2; j < pos[i + 1].size(); j++) {
            ans[pos[i + 1][j]] = (i ? 'R' : 'H');
        }
    }
    int cnt = count(ans.begin(), ans.end(), 'R');
    if (cnt == 0 || cnt == n) {
        cout << "NO\n";
    } else {
        cout << ans << '\n';
    }
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
