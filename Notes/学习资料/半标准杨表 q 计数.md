# 半标准杨表 q 计数


// final version on 23.06.29


<div class="toc"><div class="toc-container-header">目录</div><ul><li><a href="#半标准杨表-q-计数" rel="noopener nofollow">半标准杨表 q 计数</a></li></ul></div>


<h2 id="半标准杨表-q-计数">半标准杨表 q 计数</h2>

半标准杨表，每行单调不降，每列单调递增。记行数为 $r$，每行长度分别为 $\lambda_1,\lambda_2,\cdots,\lambda_r$；值域为 $[0,m]$，并 <strong>要求 $m\geq r$</strong> 。设占位符为 $q$ 。

为了刻画每一行的数，并限制列是单调递增的，考虑 LGV 引理：对于第 $i$ 行画 $(-i,0)$ 到 $(-i+\lambda_i,m)$ 的折线，横着走就记 $q^y$，竖着走就记 $1$ 。于是，记 $h_k=[x^k]\prod\limits_{0\leq i\leq m}\frac{1}{1-q^ix}$，答案就是 $\det(h_{i+\lambda_j-j})_{1\leq i,j\leq r}$ 。

为了方便，令 $\lambda_{r+1},\cdots,\lambda_{m+1}$ 均为 $0$，并将方阵的阶数扩充至 $m+1$，这样做显然不会改变答案。记 $m+1$ 阶方阵 $A=(h_{\lambda_i-i+j})_{1\leq i,j\leq m+1}$（注意转置了一下）。为了处理 $\det A$，记 $e_{k}^{(j)}=[x^k]\prod\limits_{0\leq i\leq m,i\not=k}(1-q^ix)$，并考虑 $B=(e_{m+1-i}^{(j-1)})_{1\leq i,j\leq m+1}$ 和 $A$ 相乘的结果（这样构造是为了使 $\det B\not=0$）：


$$
(AB)_{i,j}=\sum_{k=1}^{m+1}h_{\lambda_i-i+k}e_{m+1-k}^{(j-1)}=(q^{j-1})^{\lambda_i-i+m+1}

$$

而 $\det B$，考虑将 $\lambda_{1},\cdots,\lambda_{m+1}$ 置为 $0$，此时 $\det A=1$ ，因此 $\det B=\det\left((q^{j-1})^{m+1-i}\right)_{1\leq i,j\leq m+1}$ 。于是：


$$
\begin{aligned}
\det A&amp;=\det\left((q^{j-1})^{\lambda_i-i+m+1}\right)_{1\leq i,j\leq m+1}/\det\left((q^{j-1})^{m+1-i}\right)_{1\leq i,j\leq m+1}\\
&amp;=\prod_{1\leq i

现在看看这个题：<a href="https://sy.hhwdd.com/new/ViewGProblem.page?gpid=Dcp6o" target="_blank" rel="noopener nofollow">C</a> 。$\lambda_i$ 是右数第 $i$ 个 $1$ 左侧的 $0$ 个数要求和不超过 $n$，值域无所谓。将第 $i$ 行增加 $(i-1)$ 构成半标准杨表，于是答案为（取 $m=n+r-1$）：


$$
[q^n]\prod_{1\leq i

即 $[q^n]\prod\limits_{i}(1-q^i)^{k_i}$ 。$\ln\left(\prod\limits_{i}(1-q^i)^{k_i}\right)=-\sum\limits_{i}k_i\sum\limits_{j\geq 1}\frac{q^{ij}}{j}$，再 exp 回去即可。提交记录：<a href="https://sy.hhwdd.com/new/DetailResult.page?submitid=n8icf7n" target="_blank" rel="noopener nofollow">评测详情 - 清橙网络自动评测系统 (hhwdd.com)</a> 。

