摘要：计算几何，求图形的交

[传送门：https://www.luogu.com.cn/problem/AT_abc361_b](https://www.luogu.com.cn/problem/AT_abc361_b)

## 题意

给定两个正方体，求它们是否有正体积交。

## 分析思路

我们记第一个立方体以 $P(x_p, y_p, z_p)$ 为**前左下角**，$Q(x_q, y_q, z_q)$ 为**后右上角**。第二个立方体以 $A(x_a, y_a, z_a)$ 为**前左下角**，$B(x_b, y_b, z_b)$ 为**后右上角**，即如图（以立方体 $1$ 为例）。

![](https://cdn.luogu.com.cn/upload/image_hosting/m6nnsu0h.png)

求立方体的交乍一看很复杂，我们先来回忆一下如何判断数轴上的线段 $(x, y)$ 与线段 $(a, b)$ 是否有正长度交 $(x < y, a < b)$。我们从判断它们无正长度交入手。容易注意到无正长度交等价于某条线段在另一条线段的一侧，即 $y \leq a$ 或 $b \leq x$。于是这两条线段有正长度交的充要条件为 $a < y$ 且 $x < b$。

我们再把这一结论扩展到矩形 $ABCD$ 与 $MNPQ$ 是否有交（$A, M$ 为左下角，$C, P$ 为右上角）。如果我们沿水平方向平移 $MNPQ$ 到 $M'N'P'Q'$，使得 $M'Q'$ 与 $BC$ 在同一直线上，则 $ABCD$ 与 $MNPQ$ 有正面积交的必要条件是 $M'Q'$ 与 $BC$ 有正长度交。同理若沿竖直方向移动，也有必要条件 $M'N'$ 与 $AB$ 有正长度交。不难证明这两个条件也是原命题的充分条件。

最后，我们便可以得出原本要求的问题的答案了。原命题成立的充分条件是 $(x_p, x_q)$ 与 $(x_a, x_b)$、$(y_p, y_q)$ 与 $(y_a, y_b)$、$(z_p, z_q)$ 与 $(z_a, z_b)$ 分别有正长度交即可。

时间复杂度 $O\left(1\right)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int px, py, pz, qx, qy, qz, ax, ay, az, bx, by, bz;
inline bool segment_cross(int x, int y, int a, int b) {
    return a < y && x < b; // 求线段是否有正长度交
}
inline bool cube_cross(void) { // 见题解
    return segment_cross(px, qx, ax, bx) &&
           segment_cross(py, qy, ay, by) &&
           segment_cross(pz, qz, az, bz);
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO();
    cin >> px >> py >> pz >> qx >> qy >> qz;
    cin >> ax >> ay >> az >> bx >> by >> bz;
    cout << (cube_cross() ? "Yes" : "No") << '\n';
    return 0;
}

```
