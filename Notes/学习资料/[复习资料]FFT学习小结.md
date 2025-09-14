# [复习资料]FFT学习小结


<s>看了半天的FFT，总算是看懂了？</s>

FFT（快速傅里叶变换）是用来解决多项式乘法的算法，时间复杂度为$O(nlog_2n)$。

<h3 id="多项式的系数表示法和点值表示法">多项式的系数表示法和点值表示法</h3>

如何可以准确地表达一个$n$次的多项式？一种方法是<strong>系数表示法</strong>，直接给出$n+1$项的系数；另一种方法是<strong>点值表示法</strong>，给出$n+1$个点，用这些点唯一确定一个多项式。

在进行多项式乘法的时候，如果是用系数表示法，似乎不可避免地要用$O(n^2)$的时间复杂度来获得答案；但是如果使用点值表示法，如果各个点的横坐标取值都相同，就可以$O(n)$实现多项式乘法（只要把纵坐标相乘就可以得到所要的多项式）。

可以看出，用点值表示法可以很快地计算两个多项式的乘积，但是问题又来了，一般情况下，我们都是用系数表示法来表示一个多项式，系数表示法和点值表示法的转化似乎也是要$O(n^2)$（$O(n^3)$？），<strong>有没有一种方法可以很快地实现两种表示方法的转化呢？</strong>

<h3 id="复数的引入">复数的引入</h3>

在实数中，并不存在$\sqrt{-1}$，扩展到复数集$C$后我们定义$i^2=-1$，那么$i=\sqrt{-1}$，一个复数$z$可以表示为$a+bi$，其中$a.b\in R$。

可以用平面直角坐标系的一个点$(x,y)$表示一个复数$z=x+yi$，准确来说，这个平面直角坐标系应该就是<strong>复</strong>平面直角坐标系。当然我们可以用另一种方法表示平面直角坐标系上的一个点：$(r,\theta)$，表示该点距原点的距离为$r$，其与原点连线和$x$轴正方向夹角为$\theta$。同样的，我们也可以用这种方法表示一个复数。

复数的运算类似实数，把$i$当成一个普通的数就好了。

复数相乘的一个小性质：


$$
(r_1,\theta_1)(r_2,\theta_2)=(r_1r_2,\theta_1+\theta_2)

$$

（<s>不会证呜呜呜</s>，其实如果知道了三角函数的一些公式很容易证明，可以参考<a href="https://www.luogu.com.cn/blog/LSQ147/san-jiao-han-shuo-gong-shi" target="_blank" rel="noopener nofollow">this</a>）

<h3 id="fft快速傅里叶变化">FFT快速傅里叶变化</h3>

<s>DFT（离散傅里叶变换）就直接跳了吧。</s>

先假设$n$为2的整数次幂。

回到之前的问题，如何快速地在系数表示法和点值表示法之间进行转换。

如果我们取$x_0,x_1,\dots,x_{n-1}$这些数作为横坐标的话，多项式为$f(x)=a_0x^0+a_1x^1+a_2x^2+\dots+a_{n-1}x^{n-1}$，那么有：


$$
\begin{bmatrix}x_0^0&\ x_0^1&\ \cdots&\ x_0^{n-1}\\ x_1^0&\ x_1^1&\ \cdots&\ x_1^{n-1}\\ \vdots&\ \vdots&\ \ddots&\ \vdots \\x_{n-1}^0&\ x_{n-1}^1&\ \cdots&\ x_{n-1}^{n-1} \end{bmatrix}\cdot\begin{bmatrix}a_0\\a_1\\ \vdots\\a_{n-1}\end{bmatrix}=\begin{bmatrix}y_0\\y_1\\ \vdots\\y_{n-1}\end{bmatrix}

$$

FFT相当于就是要快速求出这个的值，如果第一个矩阵有逆矩阵的话，也可以通过这个式子将点值表示法转化为系数表示法。

接下来看如何选取$x$的值来减小计算量，可以选取在复平面直角坐标系中单位圆上的点（没学过复数，可能表述不准确，就是那个意思），把在复平面直角坐标系中单位圆平均分成$n$等份。

借的一张图：

<img src="https://images.cnblogs.com/cnblogs_com/lsq147/1842294/o_210104131513FFT.png" alt="" loading="lazy">

设$w_n$为图中的1号点，那么这$n$个等分点就分别是$w_n^0,w_n^1,\dots,w_n^{n-1}$，根据之前复数乘法的性质，可以得到：


$$
w_n^k=\cos\dfrac{2k\pi}{n}+i\sin\dfrac{2k\pi}{n}

$$

$w_n$称作$n$次单位根，把这些点作为横坐标的话，可以减小计算量。

先看看单位根的几个性质吧：

$w_n^0=w_n^n=1,w_n^{n/2}=-1,w_n^k=w_{2n}^{2k},w_n^k=w_n^{k\bmod n}$

直接带入公式就可以得到了。

此时我们只需要求$w_n$的0到$n-1$次幂就可以求出所有$x$的幂的值了，减小了许多计算量。

然而复杂度还是$O(n^2)$。

怎么办呢，考虑分治，原多项式为：


$$
f(x)=a_0x^0+a_1x^1+a_2x^2+\dots+a_{n-1}x^{n-1}

$$


$$
=(a_0x^0+a_2x^2+a_4x^4+\cdots +a_{n-2}x^{n-2})+x(a_1x^0+a_3x^2+a_5x^4+\cdots +a_{n-1}x^{n-2})

$$

设：

$f_1(x)=a_0x^0+a_2x^1+a_4x^2+\cdots +a_{n-2}x^\frac{n-2}{2}$

$f_2(x)=a_1x^0+a_3x^1+a_5x^2+\cdots +a_{n-1}x^\frac{n-2}{2}$

不难发现$f(x)=f_1(x^2)+x\cdot f_2(x^2)$

将$w_n^k$带入，得到$f(w_n^k)=f_1(w_n^{2k})+w_n^k\cdot f_2(w_n^{2k})$

又：$w_{n}^{k}=w_{n/2}^{k}$


$$
\therefore f(w_n^k)=f_1(w_{n/2}^k)+w_n^k\cdot f_2(w_{n/2}^k)

$$

看到这个方程，应该是可以直接推出递归是怎么写的了，首先拆系数，然后递归处理两组系数的解，得到$f_1(w_{n/2}^{0\dots n/2-1})$和$f_2(w_{n/2}^{0\dots n/2-1})$，根据这两组的解得出$f(w_n^{0\dots n-1})$。当递归到$n=1$时，只需要求$f(w_1^0)=a_0(w_1^0)^0=a_0$，解就是唯一的系数。

有没有可以优化计算的地方呢？实际上是有的：


$$
f(w_n^{k+n/2})=f_1(w_{n/2}^{k+n/2})+w_{n}^{k+n/2}\cdot f_2(w_{n/2}^{k+n/2})=f_1(w_n^k)-w_{n}^k\cdot f_2(w_{n/2}^k)

$$

可以发现，它和$f(w_n^k)$好像差别不大，所以可以在算$f(w_n^k)$的时候顺便把它也给算了（<s>好像没区别？（雾</s>）。

然后还有一个特殊的地方，就是最后求出来的$y$值可以存在和系数相同的数组里面，然后可以节省空间复杂度。

但是别忘了还有一个问题，就是FFT的逆运算IFFT，那么如果把单位根作为$x$，那个矩阵有逆矩阵吗？

相当于接下来我们是要对下面这个矩阵求逆：


$$
\begin{bmatrix}(w_n^0)^0&\ (w_n^0)^1&\ \cdots&\ (w_n^0)^{n-1}\\ (w_n^1)^0&\ (w_n^1)^1&\ \cdots&\ (w_n^1)^{n-1}\\ \vdots&\ \vdots&\ \ddots&\ \vdots \\(w_n^{n-1})^0&\ (w_n^{n-1})^1&\ \cdots&\ (w_{n-1}^{n-1})^{n-1} \end{bmatrix}

$$

矩阵求逆后是这样的（干脆记住这个结论算了）：


$$
\begin{bmatrix}\frac{1}{n}(w_n^{-0})^0&\ \frac{1}{n}(w_n^{-0})^1&\ \cdots&\ \frac{1}{n}(w_n^{-0})^{n-1}\\ \frac{1}{n}(w_n^{-1})^0&\ \frac{1}{n}(w_n^{-1})^1&\ \cdots&\ \frac{1}{n}(w_n^{-1})^{n-1}\\ \vdots&\ \vdots&\ \ddots&\ \vdots \\\frac{1}{n}(w_n^{-(n-1)})^0&\ \frac{1}{n}(w_n^{-(n-1)})^1&\ \cdots&\ \frac{1}{n}(w_{n-1}^{-(n-1)})^{n-1} \end{bmatrix}

$$

代码大概是这样的：

```cpp
void fft(complex*a,int n,int inv){
	if(n==1)return;
	for(int i=0;i<n;i+=2){
		tmp0[i/2]=a[i];
		tmp1[i/2]=a[i+1];
	}
	for(int i=0;i<n/2;++i)
		a[i]=tmp0[i],a[i+n/2]=tmp1[i];
	fft(a,n/2);fft(a+n/2,n/2);
	const complex BASE(cos(pi*2/n),inv*sin(pi*2/n));
	complex w(1,0);
	for(int i=0;i<n/2;++i){
		complex fir=a[i],sec=a[i+n/2]*w;
		a[i]=fir+sec,a[i+n/2]=fir-sec;w=w*BASE;
	}
}

```

然后连板子都过不去（没有亲测，听说的）。

复杂度明明是对的啊，但是递归就是慢，还有爆栈的风险，怎么办？写非递归板的，模拟一遍得出系数$a_i$最终所在的位置，直接枚举合并长度，一层层地合并就可以了。

不想打模拟？有一个结论：$i$变换后的最终位置为${i\text{的二进制倒置}}$。

如何证明？从低位往高位考虑的话，如果是0，就往左走，如果是1，就往右走，然后从下层往上层考虑，如果是1，贡献为2的当前经过层数次幂，如果是0，贡献为0；仔细想想，不就是二进制的倒置吗？所以可以先把每个数的最终位置算出来。

如何求二进制倒置？如下：

```cpp
	for(int i=1;i<=n;++i)
		rev[i]=(rev[i>>1]>>1)|((i&1)<<maxbit);

```

然后就可以愉快地切模板了。

```cpp
void fft(complex*a,int n,int inv){
	for(int i=0;i<n;++i)
		if(i<rev[i])swap(a[i],a[rev[i]]);
	for(int len=2;len<=n;len<<=1){
		int mid=len>>1;
		const complex BASE(cos(pi/mid),sin(pi/mid)*inv);
		for(int i=0;i<n;i+=len){
			cpx w(1,0);
			for(int j=0;j<mid;++j,w*=BASE){
				cpx fir=a[i+j],sec=a[i+j+mid]*w;
				a[i+j]=fir+sec,a[i+j+mid]=fir-sec;
			}
		}
	}
}

```

