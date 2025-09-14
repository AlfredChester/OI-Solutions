# zxy的思维技巧


<strong>如果觉得这篇博客写得好的话不妨点一个推荐</strong>（<s>我怎么沦落到求赞的地步了</s>

<h1 id="0更新日志">0.更新日志</h1>

我尽量在本篇博客总结我遇到的所有思维技巧，希望深入理解之后能形成思维网络。

本篇博客的选题很多都是 $\tt CF\ 3000+$ 的题，具有很高的思维难度而非代码难度。

$2021/8/10$，把 $dp$ 开了个头。

$2021/9/30$，继续更新 $dp$ 部分。

$2021/10/8$，继续更新 $dp$ 部分。

$2021/10/14$，继续更新 $dp$ 部分。

$2021/10/29$，感觉复习慢一点也没关系，一定要尽量自己推出来，懂得完整的逻辑链条然后从中提取我所缺失的精华，这才是复习的目的，继续更新 $dp$ 部分。

$2021/11/8$，终于开始复习图论啦，<s>现在看来退役之前肯定更不完了</s>，把这个技巧性强的复习完就满足了吧 $qwq$

$2021/11/15$，每次找判定性条件，一定要找<strong>必要条件</strong>，我已经因为没有往这方面想受到重创了。

$2021/11/19$，真他吗退役之前更不完了，$zxy$ 啊 $zxy$ 之前暑假定下的不断更目标你忘了吗？

$2021/12/21$，退役一个月之后回归，以后养成好习惯，新写一道题的题解之后就放上来。

$2022/2/9$，怎么马上就要省选了啊，省选之前争取基本完工吧，这东西真的就是我的遗产了。

$2022/3/30$，开始涉足一些以前没有整理过的板块。

$2022/4/10$，字数上万了，这篇博客称为 $\rm 3A$ 大作不过分吧？

$2022/7/19$，正式开始 $\tt NOI$ 之前为期一个月的复习计划。

$2022/7/27$，稳步进行复习中，大概在 $8/6$ 结束第一阶段的复习，在 $8/10$ 结束第二阶段的复习（整理贪心$\&$数据结构）。然后重读这篇博客，找出遗忘的地方再次复习，多推一推思路总是好的。

<h1 id="1-dp">1 dp</h1>

<h2 id="11-常规dp的思维过程">1.1 常规dp的思维过程</h2>

<h3 id="111-问题转化">1.1.1 问题转化</h3>

<ul>
<li>比如你要让所有点被覆盖，那么状态可以设计成覆盖一段前缀，并且中间不允许出现断点：<a href="https://www.cnblogs.com/C202044zxy/p/15054641.html" target="_blank">CF1476F</a></li>
<li>序列上的路径问题，可以转化成起点和终点的匹配问题，$dp$ 匹配的权值，记录匹配的标记就可做：<a href="https://www.cnblogs.com/C202044zxy/p/15017449.html" target="_blank">The Karting</a></li>
<li>多过程的题，不妨考虑末状态具有什么性质，直接对末状态进行计算。比如一类期望题中，某一种方案的定义方式最后导出等概率出现，就可以直接对此方案计数了：<a href="https://www.cnblogs.com/C202044zxy/p/15008841.html" target="_blank">绿宝石之岛</a></li>
<li>高维的问题，可以通过技巧拆分成不相关的低维问题，比如 $45$ 度旋转：<a href="https://www.cnblogs.com/C202044zxy/p/15367129.html" target="_blank">Jumping sequence</a></li>
<li>尽可能的简化问题也是问题转化的一部分，比如把具有平行关系的点缩在一起：<a href="https://www.cnblogs.com/C202044zxy/p/15122096.html" target="_blank">Black, White and Grey Tree</a></li>
<li>计数问题中任何性限制原则上优于存在性限制，可以通过切换限制主体来完成转化：<a href="https://www.cnblogs.com/C202044zxy/p/14939540.html" target="_blank">Reunion</a></li>
<li>子序列问题可以通过证明不交转化成区间问题，要向简单的 $dp$ 模型靠拢：<a href="https://www.cnblogs.com/C202044zxy/p/15645050.html" target="_blank">AmShZ Wins a Bet</a></li>
</ul>

<h3 id="112-发掘性质">1.1.2 发掘性质</h3>

理清思路真的很重要，拿到题你可以先想想什么东西在什么情况下合法。

<ul>
<li>什么时候需要你去发掘性质？当你发现直接 $dp$ 需要考虑的情况太多了，你可以手玩找一些最优解需要满足的必要条件，才能让你的 $dp$ 有的放矢，例题：<a href="https://www.cnblogs.com/C202044zxy/p/15124279.html" target="_blank">CF1368H1</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15564013.html" target="_blank">Division into Two</a></li>
<li>性质是针对限制而来的，在限制较少的题目中可以去往考虑更少情况的方向猜性质：<a href="https://www.cnblogs.com/C202044zxy/p/15054014.html" target="_blank">CF573D</a></li>
<li>在具有强烈过程性的题目可以往结果的方向猜性质：<a href="https://www.cnblogs.com/C202044zxy/p/15083470.html" target="_blank">To Make 1</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15837755.html" target="_blank">模拟赛2-A</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15886771.html" target="_blank">Eternal Average</a></li>
<li>很多时候讨论特殊情况可以得到很好的性质：<a href="https://www.cnblogs.com/C202044zxy/p/16216231.html" target="_blank">泳池</a>（讨论 $0$ 的情况可以得到调和级数的性质）</li>
<li>如果限制的主体数量级巨大（比如集合、子序列），那么可以考虑归纳、递归的方法描述限制。并且使用这种方法还有一种好处，就是递归子问题很容易拓展到 $dp$ 的形式：<a href="https://www.cnblogs.com/C202044zxy/p/16299969.html" target="_blank">Density of subarrays</a></li>
</ul>

<h3 id="113-思考决策状态">1.1.3 思考决策状态</h3>

<ul>
<li>可以先使用枚举法帮助思考我们需要决策的状态，然后用 $dp$ 加速枚举的过程：<a href="https://www.cnblogs.com/C202044zxy/p/14924802.html" target="_blank">Sorting Books</a>；<a href="https://www.cnblogs.com/C202044zxy/p/14782799.html" target="_blank">Insertion Sort</a></li>
<li>计数题可以想一想需要知道什么量可以用计数原理快速算方案，然后我们用 $dp$ 决策这些量即可：<a href="https://www.cnblogs.com/C202044zxy/p/15390544.html" target="_blank">一拳超人</a></li>
<li>可以选取一种基状态，其他状态可以由基状态修改而来，这时候尽量把问题改成多个量选$/$不选的问题，也尽量把他们的影响独立开来，然后用 $dp$ 决策这个过程：<a href="https://www.cnblogs.com/C202044zxy/p/15398834.html" target="_blank">New Year and Binary Tree Paths</a></li>
<li>如果是决策的最终状态，且有多种方式可以到达同一种最终状态，那么强制只用其中一种方式：<a href="https://www.cnblogs.com/C202044zxy/p/15505807.html" target="_blank">Mr. Kitayuta's Gift</a></li>
</ul>

<h3 id="114-选定dp主体">1.1.4 选定dp主体</h3>

我们知道，$dp$ 的本质是枚举所有状态，但是这些状态必然有一个载体，所以我对 $dp$ 主体的理解是他只是一种状态的表示方法，我们用它来描述状态，但是<strong>状态是最重要的</strong>。

很多时候你会遇到很多奇怪的 $dp$ 主体，但判断它们的唯一原则就是是否能考虑所有状态，选 $dp$ 主体的时候容易被思维定式局限，所以思考可以从哪些方面来描述状态是重要的，这里有一些关于选定 $dp$ 主体的例子：

<ul>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15113113.html" target="_blank">CF1474F</a>：选 $x$ 轴为 $dp$ 主体是不容易的，但是因为我们通常是按顺序考虑子序列所以很容易陷入这个误区，选 $y$ 轴为 $dp$ 主体问题就迎刃而解了，所以对偏序关系更高级的理解是若干个限制，不同的题需要从不同的限制入手。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15075334.html" target="_blank">卡农</a>：并不一定以小处为主体，比如这题主体为数不好做，但是主体为集合就可以直接转移了。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15074110.html" target="_blank">CF1463F</a>：遇到集合最优化问题时，可以考虑在值域上规划，选值域为 $dp$ 主体。</li>
</ul>

<h3 id="115-设计dp状态">1.1.5 设计dp状态</h3>

<ul>
<li>只保留和代价计算有关的量，比如如果代价只和数量有关，并且如果可以通过数量还原出集合，我们可以把状压的一维改成线性的：<a href="https://www.cnblogs.com/C202044zxy/p/15062207.html" target="_blank">CF1188D</a></li>
<li>当状态很大的时候可以考虑通过枚举来把某些量拿出状态里面：<a href="https://www.cnblogs.com/C202044zxy/p/14916218.html" target="_blank">Game with Cards</a></li>
<li>当问题前后相互影响时，可以考虑把全局变量定义到局部状态里面：<a href="https://www.cnblogs.com/C202044zxy/p/14616589.html" target="_blank">密室逃脱</a>；如果操作是全局的，上面的方法也很好用：<a href="https://www.cnblogs.com/C202044zxy/p/15073475.html" target="_blank">Red and Black Tree</a></li>
<li>把和限制紧密贴合的量设计到状态里面，阴间出题人可能会用题目描述来引导你设计非常慢的 $dp$ 状态，做一些基本的题意转化即可：<a href="https://www.cnblogs.com/C202044zxy/p/14211607.html" target="_blank">皮配</a></li>
<li>可以设计相反的状态，比如最小花费步数转化为最大减少步数，可能转化后更贴合题设：<a href="https://www.cnblogs.com/C202044zxy/p/15350456.html" target="_blank">Paint</a></li>
</ul>

<h3 id="116-确定dp顺序">1.1.6 确定dp顺序</h3>

听着，所谓 $dp$，最重要的是顺序。无论是考虑限制的顺序，还是计算贡献的顺序，一定要着重思考，我觉得它和结论题中的必要条件有着相同的地位。

<ul>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15013140.html" target="_blank">治疗计划</a>：如果操作是有时间顺序的话，我们很容易被局限于时间中，另一种思路是考虑操作的主体，考虑主体的关系时把时间的影响考虑进去，所以 $dp$ 不一定按时间。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15390544.html" target="_blank">一拳超人</a>：如果一个东西对另一个东西有单向的影响，那么先处理前面那个有助于考虑影响。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15037366.html" target="_blank">Kyoya and Train</a>：图问题中，要注意转移顺序，通常这一维时单调的就可以作为顺序维，比如时间。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/15763995.html" target="_blank">集合选数</a>：$dp$ 顺序尽量让限制紧凑一些，我们也许可以在独立的前提下篡改顺序。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/16323248.html" target="_blank">Singer House</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16083932.html" target="_blank">环</a>：注意 $dp$ 的顺序和方案的顺序是有区别的。比如对于路径计数问题，如果我们按照序列从左到右 $/$ 树形自底向上的 $dp$ 顺序计数，那么达到的效果就是若干条有向链合并的过程。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/16335876.html" target="_blank">MEX counting</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16365240.html" target="_blank">遗迹</a>：可能需要根据限制的特性，提前$/$延迟确定一些元素的过程。</li>
<li><a href="https://www.cnblogs.com/C202044zxy/p/16394109.html" target="_blank">The Hanged Man</a>：选取合适的 $dp$ 顺序，可以减少 $dp$ 状态。</li>
</ul>

<h3 id="117-寻找子问题">1.1.7 寻找子问题</h3>

<ul>
<li>这部分我觉得可以总结一些核心的思想：<strong>寻找子问题最重要的是找状态之间的相似性</strong>，所谓相似性的含义就是信息记录在子问题中的一部分的占比，相似性越大你写转移就越容易，这需要你强大的观察能力：<a href="https://www.cnblogs.com/C202044zxy/p/15850310.html" target="_blank">stars</a>；要去构造问题之间的相似性，比如染色问题中可以通过钦定不变色来获得相似性：<a href="https://www.cnblogs.com/C202044zxy/p/16326908.html" target="_blank">Shrinking Tree</a></li>
<li>寻找子问题要考虑消除后面操作的影响，这样才能保证没有后效性：如果某个元素对于前面的影响是长远的，但又只能考虑一小步转移，那么<strong>寻找当前问题的等效子问题</strong>，就可以把这个影响传递下去，完成转移：<a href="https://www.cnblogs.com/C202044zxy/p/15850310.html" target="_blank">stars</a>；或者可以通过枚举法来消除所谓影响：<a href="https://www.cnblogs.com/C202044zxy/p/15054641.html" target="_blank">Lanterns</a></li>
<li>当考虑一小步不能很好的归纳到子问题时，可以定义一个辅助数组来解决这个中介状态：<a href="https://www.cnblogs.com/C202044zxy/p/15031099.html" target="_blank">矩阵</a>、统计问题中若当前不能计算代价，可以设计辅助数组把代价存下来：<a href="https://www.cnblogs.com/C202044zxy/p/15367043.html" target="_blank">消失的运算符</a></li>
<li>枚举法制造限制来划分子问题，比如顺序匹配问题中可以枚举空位：<a href="https://www.cnblogs.com/C202044zxy/p/14992391.html" target="_blank">CF1439D</a></li>
</ul>

<h3 id="118-考虑如何转移">1.1.8 考虑如何转移</h3>

<ul>
<li>
转移的大步小步。小步适于考虑转移，但是可能会消耗更多时间；大步常常会很复杂，但是可能起到加速的效果。在大步小步之间切换，才能写出合适的转移：<a href="https://www.cnblogs.com/C202044zxy/p/16281427.html" target="_blank">Appleman and a Game</a>
</li>
<li>
转移中存在的限制很烦，在一些计数问题中，对于多个元素的限制可以考虑某些元素任意选，另一些元素为了满足限制而具有确定的选法，<strong>计数 $dp$ 中选择的顺序是十分重要的</strong>：<a href="https://www.cnblogs.com/C202044zxy/p/15075334.html" target="_blank">卡农</a>
</li>
<li>
如果确定转移顺序之后前后会相互影响，那么在后影响前的时候可以通过枚举来解决这个影响，再归纳到子问题：<a href="https://www.cnblogs.com/C202044zxy/p/15054641.html" target="_blank">CF1476F</a>
</li>
<li>
多个对象的决策问题中，通常只考虑最后一个对象的决策：<a href="https://www.cnblogs.com/C202044zxy/p/14992391.html" target="_blank">CF1439D</a>
</li>
<li>
正难则反是很重要的技巧，比如在计算<code>第一次到达的方案数</code>的题目中，容斥掉的项就是子问题：<a href="https://www.cnblogs.com/C202044zxy/p/15570479.html" target="_blank">逃跑</a>
</li>
<li>
转移的时候适当的算错，可能会让转移简洁很多：常见的模型是算错但是一定不会作为转移点 <a href="https://www.cnblogs.com/C202044zxy/p/15872166.html" target="_blank">link</a>；还有是算少但是最优解可以被统计到：<a href="https://www.cnblogs.com/C202044zxy/p/15869034.html" target="_blank">Gerald and Path</a>
</li>
<li>
复合 $dp$，设计多个 $dp$ 状态，互相转移的方法是很值得思考的。其实我感觉它的原理还是<strong>分步思想</strong>，把一个较为复杂的问题拆分成若干个部分解决，这些部分中可能又蕴含子问题：<a href="https://www.cnblogs.com/C202044zxy/p/15887623.html" target="_blank">模拟赛6-A</a>；或者是多个 $dp$，一个 $dp$ 用于拆分，一个 $dp$ 用于解决被弱化过的问题，通常可以得到很好的性质：<a href="https://www.cnblogs.com/C202044zxy/p/14992391.html" target="_blank">CF1439D</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16216231.html" target="_blank">泳池</a>
</li>
<li>
可以把限制的判断拆分到转移中进行：<a href="https://www.cnblogs.com/C202044zxy/p/16216231.html" target="_blank">泳池</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16365240.html" target="_blank">遗迹</a>
</li>
</ul>

<h2 id="12-常见dp优化方式">1.2 常见dp优化方式</h2>

<h3 id="121-斜率优化">1.2.1 斜率优化</h3>

<ul>
<li>有些题很难看出需要用斜率优化，把和状态有关的量当成常数，把只和转移点有关的量当成纵坐标，把交叉项系数当成横坐标，把转移点的某个量当成直线去切即可：<a href="https://www.cnblogs.com/C202044zxy/p/15055437.html" target="_blank">CF1067D</a></li>
<li>另一类不常见的斜率优化是，变形出斜率的形式，维护凸包：<a href="https://www.cnblogs.com/C202044zxy/p/16209723.html" target="_blank">国王饮水记</a></li>
</ul>

<h3 id="122-决策单调性">1.2.2 决策单调性</h3>

定义：对于 $a<b<c<d$，若 $f(c)$ 从 $b$ 转移比 $a$ 优，那么 $f(d)$ 从 $b$ 转移也比 $a$ 优。

判断式：$w(j,i+1)+w(j+1,i)\geq w(j,i)+w(j+1,i+1)$

通用实现：把决策点拿去分治，每次更新中间位置的状态，然后把决策点划分到两边。

<ul>
<li>一些最值问题可以套用决策单调性模型：<a href="https://www.cnblogs.com/C202044zxy/p/15411864.html" target="_blank">课桌</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16435769.html" target="_blank">Evacuation</a></li>
</ul>

<h3 id="123-考虑有效转移状态">1.2.3 考虑有效转移/状态</h3>

<ul>
<li>只去计算和答案有关的状态（也就是减少状态的数量），当前前提是转移不出问题：<a href="https://www.cnblogs.com/C202044zxy/p/15202140.html" target="_blank">Rescue Niwen!</a>；如果答案是和式，那么可以把状态也定义成和式：<a href="https://www.cnblogs.com/C202044zxy/p/15994415.html" target="_blank">Student's Camp</a></li>
<li>在某一种特殊情况下一种转移的方式会变少，可能达到复杂度级别的优化。序列上可以考虑区间交$/$不交情况下的有效转移：<a href="https://www.cnblogs.com/C202044zxy/p/15002096.html" target="_blank">Communism</a></li>
<li>两种转移的时候选一种为主体，另一种在它的基础上对答案有影响的时候就是有效转移；并且如果一种状态明显超过了需要考虑的范围（就算在价值上它是更优的），你也不需要去考虑它：<a href="https://www.cnblogs.com/C202044zxy/p/14999074.html" target="_blank">CF771E</a></li>
<li>从某个点转移一定比另一个点转移要优，看起来很朴素的结论却能在很多时候有奇效。它可以让转移点的数量大大减少：<a href="https://www.cnblogs.com/C202044zxy/p/15863869.html" target="_blank">Bank Security Unification</a>；也可以忽略到一些恶心的限制（不合法的点一定不优）：<a href="https://www.cnblogs.com/C202044zxy/p/15872166.html" target="_blank">模拟赛4-A</a>；也可以利用贪心大致筛选一些有效转移：<a href="https://www.cnblogs.com/C202044zxy/p/16272780.html" target="_blank">Professional layer</a></li>
</ul>

<h3 id="124-讨论法">1.2.4 讨论法</h3>

<ul>
<li>整个问题重要的量可能很多，都需要用状态表示出来，但某些情况下有用的量可能会减少，这时候可以分类讨论，互相转移：<a href="https://www.cnblogs.com/C202044zxy/p/14842025.html" target="_blank">Favorite Game</a></li>
<li>当 $dp$ 的取值很少且是分层转移的，可以讨论并用数据结构维护状态：<a href="https://www.cnblogs.com/C202044zxy/p/14770466.html" target="_blank">Split</a></li>
<li>图论问题可以讨论掉一些较小的环来降低复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/15072516.html" target="_blank">Abandoning Roads</a></li>
<li>讨论法很适合处理环形 $dp$ 问题。环形 $dp$ 的关键是：断开哪一条环边（我们想要环边具有怎样的特殊性质）；断开环边局部的状态是怎样的（简单讨论可以解决）：<a href="https://www.cnblogs.com/C202044zxy/p/16303607.html" target="_blank">Sonya Partymaker</a></li>
</ul>

<h3 id="125-等价类">1.2.5 等价类</h3>

<ul>
<li>在很多图论问题中状压环的时候，如果转移只和环的大小有关，那么可以把相同大小的环看成一个等价类：<a href="https://www.cnblogs.com/C202044zxy/p/15116570.html" target="_blank">CF1466H</a></li>
<li>只和数量有关而和顺序无关可以按照数量划分等价类：<a href="https://www.cnblogs.com/C202044zxy/p/15505807.html" target="_blank">Mr. Kitayuta's Gift</a></li>
<li>乘积不超过某个数的题目可以考虑通过逆向整除分块来划分等价类：<a href="https://www.cnblogs.com/C202044zxy/p/15527778.html" target="_blank">Ridiculous Netizens</a></li>
<li>转移方式相同而且易于计算总方案数，可以把它们划分为等价类：<a href="https://www.cnblogs.com/C202044zxy/p/15570479.html" target="_blank">逃跑</a></li>
</ul>

<h3 id="126-数据结构优化">1.2.6 数据结构优化</h3>

<ul>
<li>对于数列覆盖问题，常有的结论是<strong>两个点的覆盖范围要么包含、要么相离</strong>，这时候可以选择用单调数据结构维护（因为覆盖范围单调），而不是带 $\tt log$ 的数据结构：<a href="https://www.cnblogs.com/C202044zxy/p/15009805.html" target="_blank">CF1131G</a></li>
<li>发现题目有不可解决的维度时，要敢于使用数据结构。但是此时空间消耗特别重要，注意处理 $dp$ 的顺序，才能时数据结构的使用简单化，并且减少常数的消耗：<a href="https://www.cnblogs.com/C202044zxy/p/16251174.html" target="_blank">购票</a></li>
</ul>

<h3 id="127-解决巨大数据范围">1.2.7 解决巨大数据范围</h3>

<ul>
<li>离散化是解决较大数据范围的一种方式，如果某一段内转移相同，可以离散化出段然后矩阵加速：<a href="https://www.cnblogs.com/C202044zxy/p/15113113.html" target="_blank">CF1474F</a>；或者记录离散化段内的信息转移：<a href="https://www.cnblogs.com/C202044zxy/p/15009289.html" target="_blank">划艇</a>；离散化还可以把连续型问题转化为离散型问题：<a href="https://www.cnblogs.com/C202044zxy/p/16308106.html" target="_blank">Random Ranking</a></li>
<li>可以找循环节，只要不同的循环节之间没有多的限制就行，然后每个循环内部都取最优解。证明的关键在于没有构成完整循环的那部分，可以考虑调整法证明（具体思路见例题）：<a href="https://www.cnblogs.com/C202044zxy/p/15074110.html" target="_blank">CF1463F</a></li>
<li>对于最优化的 $dp$ 问题，可以去找转移点的结论，知道每个转移点转移多少次就可以矩阵加速了：<a href="https://www.cnblogs.com/C202044zxy/p/15055437.html" target="_blank">CF1067D</a></li>
</ul>

<h3 id="128-考虑转移路径">1.2.8 考虑转移路径</h3>

<ul>
<li>转移路径可以形象化地表示出来，那么可以把简单的 $dp$ 转化成计数问题，比如这里二维简单 $dp$ 转网格图计数：<a href="https://www.cnblogs.com/C202044zxy/p/15105407.html" target="_blank">[JLOI2015]骗我呢</a></li>
<li>考虑 $dp$ 的组合意义可以建出图论模型，快速计算时直接枚举其中的某个量：<a href="https://www.cnblogs.com/C202044zxy/p/15230886.html" target="_blank">匹配字符串</a></li>
</ul>

<h3 id="129-费用计算优化">1.2.9 费用计算优化</h3>

<ul>
<li>如果当前状态不好知道，但你清楚代价的变化规则时，可以费用提前计算：<a href="https://www.cnblogs.com/C202044zxy/p/15310401.html" target="_blank">Candles</a></li>
<li>如果代价是全局的，那么可以费用延后计算（但一定要注意有无后效性啊??）：<a href="https://www.cnblogs.com/C202044zxy/p/15073475.html" target="_blank">Red and Black Tree</a></li>
</ul>

<h3 id="1210-分治优化">1.2.10 分治优化</h3>

<ul>
<li>背包问题中如果只有一种元素会特殊，那么可以用分治优化，每次保留本层最优解传递上去即可，复杂度会优化的原因是分治会通过在部分中竞争保留最优解：<a href="https://www.cnblogs.com/C202044zxy/p/14433290.html" target="_blank">篮球</a>；</li>
<li>只要是对于一段区间只有加入就可以尝试线段树分治，不一定要是序列上的区间，可以是先建立树形结构之后，对于一段 $\tt dfn$ 区间的修改：<a href="https://www.cnblogs.com/C202044zxy/p/14433290.html" target="_blank">序列</a></li>
<li>如果只有一个位置不加入，也可以直接分治优化：<a href="https://www.cnblogs.com/C202044zxy/p/16308106.html" target="_blank">Random Ranking</a></li>
</ul>

<h3 id="1211-神奇优化">1.2.11 神奇优化</h3>

<ul>
<li>当转移代价无法优化时，可以考虑转移点数量的限制，然后快速找到转移点：<a href="https://www.cnblogs.com/C202044zxy/p/15232519.html" target="_blank">绯色IOI</a></li>
<li>不支持取 $\max$ 操作时，可以通过考虑转移点的性质来转贪心，通常是权值单向影响的题目：<a href="https://www.cnblogs.com/C202044zxy/p/15136200.html" target="_blank">CF878E</a></li>
<li>如果 $dp$ 值单调，并且需要支持合并和取最值，那么可以用 $\tt set$ 维护差分标记：<a href="https://www.cnblogs.com/C202044zxy/p/15022180.html" target="_blank">魔法树</a></li>
<li>如果 $dp$ 代价为正并且只存在于单点上，那么可以考虑成类最短路，每次拿到最小的那个能扩展就扩展，用数据结构维护最容易被扩展的点即可（通常是线段树维护最值）：<a href="https://www.cnblogs.com/C202044zxy/p/15013140.html" target="_blank">治疗计划</a></li>
<li>整体 $dp$，如果多个 $dp$ 转移方式完全相同，那么可以考虑一次 $dp$ 求出答案，比如序列上可以通过端点初始化和端点统计完成：<a href="https://www.cnblogs.com/C202044zxy/p/15492846.html" target="_blank">Extreme Extension</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16394109.html" target="_blank">Foreigner</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16463153.html" target="_blank">星图</a>；更加灵活的形式是找问题之间的关系：<a href="https://www.cnblogs.com/C202044zxy/p/15309579.html" target="_blank">Palindromic Hamiltonian Path</a>；其他例子：<a href="https://www.cnblogs.com/C202044zxy/p/15505807.html" target="_blank">Mr. Kitayuta's Gift</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16348877.html" target="_blank">山河重整</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16492266.html" target="_blank">线段树</a></li>
</ul>

<h2 id="13-常见dp模型">1.3 常见dp模型</h2>

<h3 id="131-树形dp">1.3.1 树形dp</h3>

<ul>
<li>树背包真正的复杂度是第一维大小乘上第二维大小，特别是第二维很小的情况：<a href="https://www.cnblogs.com/C202044zxy/p/15042761.html" target="_blank">CF1097G</a></li>
<li>建立一些使用于特殊性质的树形结构，并在结构上规划：<a href="https://www.cnblogs.com/C202044zxy/p/15216199.html" target="_blank">浅谈笛卡尔树结构的应用</a></li>
<li>树重排规划问题可以考虑转区间 $dp$，本质上就是枚举根的过程：<a href="https://www.cnblogs.com/C202044zxy/p/15201894.html" target="_blank">Tree Tweaking</a></li>
<li>路径规划问题，可以枚举起点，然后使用换根 $dp$ 来获取最优的起点：<a href="https://www.cnblogs.com/C202044zxy/p/14934381.html" target="_blank">Svjetlo</a></li>
<li>合并问题可以从叶子开始考虑：<a href="https://www.cnblogs.com/C202044zxy/p/14843171.html" target="_blank">Logical Operations on Tree</a></li>
<li>树上路径的限制问题，可以只保留最深的$/$最浅的记为状态转移：<a href="https://www.cnblogs.com/C202044zxy/p/14567842.html" target="_blank">命运</a></li>
<li>不支持合并的树形 $dp$ 问题可以考虑转成 $\tt dfn$ 序上 $dp$（树上依赖背包）：<a href="https://www.cnblogs.com/C202044zxy/p/15527778.html" target="_blank">Ridiculous Netizens</a></li>
</ul>

<h3 id="132-状压dp">1.3.2 状压dp</h3>

<ul>
<li>图计数问题，通常是考虑导出子图，常用的技巧是正难则反：<a href="https://www.cnblogs.com/C202044zxy/p/15120848.html" target="_blank">浅谈状压dp在图计数问题上的应用</a></li>
<li>如果只需要知道大小关系，状压 $dp$ 也可用于确定序列问题上，需要多消耗 $O($值域$)$ 的复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/15205953.html" target="_blank">Random Robots</a></li>
<li>状压套折半搜索具有神奇的复杂度 $O((1+\sqrt 2)^n)$：<a href="https://www.cnblogs.com/C202044zxy/p/15134937.html" target="_blank">Harry The Potter</a></li>
</ul>

<h3 id="133-连续段dp">1.3.3 连续段dp</h3>

<ul>
<li>连续段 $dp$ 可以计算端点产生的贡献，这是和连续段数正相关的：<a href="https://www.cnblogs.com/C202044zxy/p/15216300.html" target="_blank">摩天大楼</a></li>
<li>写转移的时候可以用<strong>两段之间任意长</strong>这个条件，方案数就便于计算了：<a href="https://www.cnblogs.com/C202044zxy/p/14728361.html" target="_blank">Phoenix and Computers</a></li>
</ul>

<h3 id="134-背包">1.3.4 背包</h3>

<ul>
<li>如果装入的总物品不是很多（$\sqrt n$ 级别）并且连续，可以考虑柱状图 $dp$，转移分为增加一个柱子和把所有柱子整体升高：<a href="https://www.cnblogs.com/C202044zxy/p/15117964.html" target="_blank">逆序对</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16348877.html" target="_blank">山河重整</a>；而且这种 $dp$ 天然带有顺序，如果你想求前 $k$ 大的话是特别方便的：<a href="https://www.cnblogs.com/C202044zxy/p/15008841.html" target="_blank">绿宝石之岛</a></li>
<li>多重背包如果只需要判定存在性，可以维护还剩下的物品数量，转完全背包：<a href="https://www.cnblogs.com/C202044zxy/p/14924802.html" target="_blank">AB tree</a></li>
<li>如果背包支持快速合并（有凸性），那么可以直接用分治优化：<a href="https://www.cnblogs.com/C202044zxy/p/15367355.html" target="_blank">妹妹卡组</a></li>
<li>要有一个总体是时间观，很多巧妙的背包优化只能把 $n$ 变成 $\sqrt n$，如果需要本质复杂度的下降，那么可以尝试改变维护的东西，降维是常用的技巧：<a href="https://www.cnblogs.com/C202044zxy/p/15175343.html" target="_blank">Tree Degree Subset Sum</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15367129.html" target="_blank">Jumping sequence</a>，降维的关键就是找独立性之后拆分：<a href="https://www.cnblogs.com/C202044zxy/p/14211607.html" target="_blank">皮配</a></li>
<li>有一个物品是特殊的，它的选取方式和其他物品不一样，特殊考虑它就能让问题变简单：<a href="https://www.cnblogs.com/C202044zxy/p/15089204.html" target="_blank">Lucky Numbers</a>；少量特殊物品时可以考虑分别计算然后合并背包：<a href="https://www.cnblogs.com/C202044zxy/p/14211607.html" target="_blank">皮配</a></li>
<li>很多时候背包会存在（隐藏的）拓扑关系，这时候的结论可能是选了小价值物品就必须选大价值物品：<a href="https://www.cnblogs.com/C202044zxy/p/15084565.html" target="_blank">Turtle</a></li>
<li>总容量大，但是物品重量很小的背包，可以按二进制位考虑压缩有效状态数：<a href="https://www.cnblogs.com/C202044zxy/p/16537351.html" target="_blank">物品</a></li>
<li>前后缀背包，把这个思想搬到树上就是求出 $\tt dfn$ 正序背包和 $\tt dfn$ 倒序背包，然后再合并：<a href="https://www.cnblogs.com/C202044zxy/p/16394109.html" target="_blank">苹果树</a></li>
</ul>

<h3 id="135-计数dp">1.3.5 计数dp</h3>

计数 $dp$ 的原则是：初始有一些基础方案，然后我们逐步添加可以区分出方案的东西（这东西是根据方案不同的定义来的），转移到不同的地方就代表这一步产生了不同的方案：<a href="https://www.cnblogs.com/C202044zxy/p/15947433.html" target="_blank">高维游走</a>

<ul>
<li>组合意义的嵌套的重要的方法，也就是我们给我们的代价函数确定一个组合意义，那么 $dp$ 就需要同时完成确定局面和组合计数的功能，常用技巧是拆分：<a href="https://www.cnblogs.com/C202044zxy/p/15067652.html" target="_blank">Pass to Next</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14623770.html" target="_blank">Stranger Trees</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14237662.html" target="_blank">Crash 的文明世界</a></li>
<li>巧妙的枚举可以达到合并两个子问题方案的目的，比如本题枚举可以合并组合数：<a href="https://www.cnblogs.com/C202044zxy/p/15042761.html" target="_blank">CF1097G</a></li>
<li>如果判断合法需要使用 $dp$，而原问题又是一个计数问题时，可以使用 $dp$ 套 $dp$，其关键还是建出 $\tt DFA$：<a href="https://www.cnblogs.com/C202044zxy/p/15895541.html" target="_blank">模拟赛7-A</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15790173.html" target="_blank">游园会</a></li>
</ul>

<h3 id="136-杂项">1.3.6 杂项</h3>

<ul>
<li>$dp$ 维护直线函数：<a href="https://www.cnblogs.com/C202044zxy/p/15082636.html" target="_blank">CF889E</a></li>
<li>$\tt slope\ trick$ 优化 $dp$，主要处理代价带绝对值的规划题目：<a href="https://www.cnblogs.com/C202044zxy/p/14908532.html" target="_blank">折线算法</a>；可以维护很多跟斜率有关的操作，比如含有 $\tt max$ 的函数：<a href="https://www.cnblogs.com/C202044zxy/p/16435769.html" target="_blank">Increment Decrement</a></li>
<li>$dp$ 维护容斥系数，在大小关系中的计数题中十分常见：<a href="https://www.cnblogs.com/C202044zxy/p/14433290.html" target="_blank">小D与随机</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14403490.html" target="_blank">不等关系</a>；一些需要考虑集合的毒瘤题中，暴力指数级容斥出奇迹，再转 $dp$ 维护即可：<a href="https://www.cnblogs.com/C202044zxy/p/14629132.html" target="_blank">猎人杀</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16475708.html" target="_blank">百鸽笼</a></li>
</ul>

<h1 id="2-图论">2 图论</h1>

<h2 id="21-图论问题的思维过程">2.1 图论问题的思维过程</h2>

<h3 id="211-图论模型的建立">2.1.1 图论模型的建立</h3>

首先考虑对于原问题的什么对象建立模型（主体的选取），然后尝试用图论的各种意义去表示原问题中的元素（比如点权、边权、路径、连通块$...$），这两步都不要被定式思维所限制。

以题目中的一个限制为基础建立模型，这一步不要被思维定式所局限。

<ul>
<li>遇到难以处理的全局限制时可以考虑图论：<a href="https://www.cnblogs.com/C202044zxy/p/15758201.html" target="_blank">Maximum Adjacent Pairs</a></li>
<li>矩形中类似推箱子的问题，可以把箱子的移动转化成空格的移动，建立关于空格移动路径的图：<a href="https://www.cnblogs.com/C202044zxy/p/15423742.html" target="_blank">Shifting Dominoes</a></li>
<li>区间元素和的判定问题可以把前缀和建成点，原图中的元素建成连接前缀和的边：<a href="https://www.cnblogs.com/C202044zxy/p/14967601.html" target="_blank">Flip and Reverse</a></li>
</ul>

<h3 id="212-图结构的分析">2.1.2 图结构的分析</h3>

<ul>
<li>树形结构具有很多优美的性质，可以通过分析度数与环来证明树形结构：<a href="https://www.cnblogs.com/C202044zxy/p/15423742.html" target="_blank">Shifting Dominoes</a></li>
<li>$dfs$ 树是很重要的思维方式，通常我们分析树边和非树边能得到很多有意思的性质。如果是有向图还要考虑哪个点为根更好分析：<a href="https://www.cnblogs.com/C202044zxy/p/15158524.html" target="_blank">James and the Chase</a>；或者可以通过 $\tt dfs$ 树直接给出构造：<a href="https://www.cnblogs.com/C202044zxy/p/15814404.html" target="_blank">Nezzar and Hidden Permutations</a>；<a href="https://www.cnblogs.com/C202044zxy/p/15811525.html" target="_blank">Weighting a Tree</a></li>
<li>$\tt DAG$ 具有很好的性质，所以就算是遇到一般图时，也可以先思考在 $\tt DAG$ 的环境下如何处理：<a href="https://www.cnblogs.com/C202044zxy/p/16260889.html" target="_blank">Sergey's problem</a></li>
</ul>

<h3 id="213-问题转化">2.1.3 问题转化</h3>

<ul>
<li>度数和连通性常常可以互化，但度数描述单点限制，连通性描述整体限制：<a href="https://www.cnblogs.com/C202044zxy/p/15017682.html" target="_blank">电报</a></li>
<li>图论中拆分思维也很重要，把总贡献拆分到点上$/$边上：<a href="https://www.cnblogs.com/C202044zxy/p/15014634.html" target="_blank">Keys</a></li>
<li>如果不同的限制具有拓扑关系，根据题目特性可能可以忽略一些限制：<a href="https://www.cnblogs.com/C202044zxy/p/14908361.html" target="_blank">Falling sand</a></li>
<li>一类东西只贡献一次的问题可以考虑转化成匹配问题：<a href="https://www.cnblogs.com/C202044zxy/p/15758201.html" target="_blank">Maximum Adjacent Pairs</a></li>
<li>可达性问题可以考虑转化成连通性问题，此时注意考察连通的双向性和等价性：<a href="https://www.cnblogs.com/C202044zxy/p/15763162.html" target="_blank">Cow and Vacation</a></li>
</ul>

<h3 id="214-寻找结论">2.1.4 寻找结论</h3>

<ul>
<li>如果点权集中可以获得更多贡献，那么可以思考答案会不会是原图的最大团：<a href="https://www.cnblogs.com/C202044zxy/p/15481322.html" target="_blank">七负我</a></li>
<li>从小处入手思考结论（比如叶子），那么你可能发现原来复杂的问题变简单了：<a href="https://www.cnblogs.com/C202044zxy/p/15018539.html" target="_blank">绯色 IOI（抵达）</a></li>
<li>拓扑覆盖问题可以考虑在原序列上的区间性质：<a href="https://www.cnblogs.com/C202044zxy/p/14908361.html" target="_blank">Falling sand</a>；连通性问题也可能有区间性质：<a href="https://www.cnblogs.com/C202044zxy/p/15244536.html" target="_blank">Number of Components</a></li>
<li>通过双图策略寻找性质：<a href="https://www.cnblogs.com/C202044zxy/p/15213540.html" target="_blank">String Transformation 2</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15865910.html" target="_blank">Next or Nextnext</a></li>
<li>生成树的应用是广泛的，关注性质 <code>答案一定在满足某种性质的生成树上</code>：<a href="https://www.cnblogs.com/C202044zxy/p/15887623.html" target="_blank">模拟赛6-B</a></li>
</ul>

<h2 id="22-常见算法应用">2.2 常见算法应用</h2>

<h3 id="221-最短路">2.2.1 最短路</h3>

<ul>
<li>$\tt dijk$ 的<strong>遍历思想</strong>在很多题中有大用处，如果每个点只需要遍历一次那么维护最有可能遍历的点：<a href="https://www.cnblogs.com/C202044zxy/p/15194017.html" target="_blank">Mike and code of a permutation</a>；<a href="https://www.cnblogs.com/C202044zxy/p/15013140.html" target="_blank">治疗计划</a></li>
<li>动态加边可以解决到达时间最小的限制，它的本质是如果具有连通性就可以说明最早到达：<a href="https://www.cnblogs.com/C202044zxy/p/15066059.html" target="_blank">Dirty Arkady's Kitchen</a>；动态加边还可以直接维护强连通性，对正反图动态 $\tt bfs$ 即可：<a href="https://www.cnblogs.com/C202044zxy/p/15024721.html" target="_blank">图函数</a>；动态加边还可以完成版本之间的转化，结合 $\tt spfa$ 可以做到某些情况下的动态最短路：<a href="https://www.cnblogs.com/C202044zxy/p/16063441.html" target="_blank">道路堵塞</a></li>
<li>用 $\tt dijk$ 可以保证一些特殊的转移顺序：<a href="https://www.cnblogs.com/C202044zxy/p/15966496.html" target="_blank">Intergalaxy Trips</a></li>
<li>环上的最短路，如果数据范围极大，先考虑重构环之后再扫环：<a href="https://www.cnblogs.com/C202044zxy/p/15848141.html" target="_blank">Drazil and His Happy Friends</a></li>
<li>删点后求最短路的问题有固定套路，就是考虑有一条边一定会跨过这个点，可以拼凑出路径来：<a href="https://www.cnblogs.com/C202044zxy/p/15569391.html" target="_blank">风之轨迹</a>；<a href="https://www.cnblogs.com/C202044zxy/p/16063441.html" target="_blank">道路堵塞</a></li>
</ul>

<h3 id="222-连通性">2.2.2 连通性</h3>

<ul>
<li>互达问题可以思考连通性，连通性重要的一点是传递性，有些问题可能只有特殊点具有连通性：<a href="https://www.cnblogs.com/C202044zxy/p/15763162.html" target="_blank">Cow and Vacation</a></li>
<li>$\tt lct$ 可以维护动态边双连通分量，考虑把非树边的影响转化成赋值标记即可：<a href="https://www.cnblogs.com/C202044zxy/p/15553514.html" target="_blank">Bear and Chemistry</a></li>
<li>无向图路径的最值问题，可以将边权排序之后转化为维护连通性（最小生成树只是特例）：<a href="https://www.cnblogs.com/C202044zxy/p/15427287.html" target="_blank">路径查询</a></li>
<li>对于带修改的问题来说，可以有一个统一的固定结构来处理两点连通的时刻（边起作用的时刻）：<a href="https://www.cnblogs.com/C202044zxy/p/15357002.html" target="_blank">Gates to Another World</a>；维护强连通性，可以用整体二分求出每条边强连通的时间，然后转化成维护无向图的连通性：<a href="https://www.cnblogs.com/C202044zxy/p/15955558.html" target="_blank">WD与地图</a></li>
</ul>

<h3 id="223-拓扑排序">2.2.3 拓扑排序</h3>

<ul>
<li>拓扑排序可以提供一种解构图的顺序，注意拓扑排序中天然带有的可达性：<a href="https://www.cnblogs.com/C202044zxy/p/16287298.html" target="_blank">Pink Floyd</a>；如果需要一些出现顺序的关系，可以先建立图之后考虑其拓扑序：<a href="https://www.cnblogs.com/C202044zxy/p/16358646.html" target="_blank">Insider's Information</a></li>
<li>拓扑排序的性质，同在队列里的点没有任何到达关系，可以通过它来筛选合法点：<a href="https://www.cnblogs.com/C202044zxy/p/16326908.html" target="_blank">Upgrading Cities</a></li>
<li>拓扑排序的另类形式：按 $1\sim n$ 的顺序在反图上 $\tt dfs$，最后回溯的顺序就是拓扑序：<a href="https://www.cnblogs.com/C202044zxy/p/15194017.html" target="_blank">Mike and code of a permutation</a></li>
</ul>

<h3 id="224-将问题转化到图论算法">2.2.4 将问题转化到图论算法</h3>

<ul>
<li>调整法可以帮助建出差分约束限制，矩阵问题可以以行和列为待定变量：<a href="https://www.cnblogs.com/C202044zxy/p/15026223.html" target="_blank">矩阵游戏</a></li>
<li>拓扑排序可以解决带平局的博弈问题，首先全部设置为平局，按照定义对反图跑拓扑即可：<a href="https://www.cnblogs.com/C202044zxy/p/15004334.html" target="_blank">Shiritori</a></li>
<li>差分约束的反向应用，如果要求是不出现负环，那么等价于有差分约束的合法解，那么可以把图上的边都写成不等式：<a href="https://www.cnblogs.com/C202044zxy/p/16091974.html" target="_blank">Negative Cycle</a></li>
<li>二分图染色问题，如果需要优化连边，得到同色的性质可以缩成一个点，然后套用并查集路径压缩的时间复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/16367385.html" target="_blank">港口设施</a></li>
</ul>

<h2 id="23-树问题">2.3 树问题</h2>

知识点：树的直径（邻域理论）、树的中心、链剖分、虚树、树分治、树合并、树哈希。

<h3 id="231-问题转化">2.3.1 问题转化</h3>

<ul>
<li>无根树问题定根（比如路径、删叶子）是一种重要的思维方法：<a href="https://www.cnblogs.com/C202044zxy/p/15645050.html" target="_blank">Squid Game</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14934381.html" target="_blank">D</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15759821.html" target="_blank">Matches Are Not a Child's Play</a>；考虑重心可以去掉一些关于子树大小的 $\min$ 式子：<a href="https://www.cnblogs.com/C202044zxy/p/15440762.html" target="_blank">Distance Matching</a></li>
<li>树上最远点问题可以转直径端点：<a href="https://www.cnblogs.com/C202044zxy/p/15845858.html" target="_blank">CF516D</a>、广义的直径问题是带权匹配，是支持点权的：<a href="https://www.cnblogs.com/C202044zxy/p/15900959.html" target="_blank">情报中心</a>；最长链问题也可能和直径有联系：<a href="https://www.cnblogs.com/C202044zxy/p/15895541.html" target="_blank">模拟赛7-B</a>；以直径中点为根建树可能是不错的选择：<a href="https://www.cnblogs.com/C202044zxy/p/15845858.html" target="_blank">Drazil and Morning Exercise</a>；以直径端点建树也可能有很好的性质：<a href="https://www.cnblogs.com/C202044zxy/p/15572977.html" target="_blank">Spiders Evil Plan</a></li>
</ul>

<h3 id="232-树上算法">2.3.2 树上算法</h3>

<ul>
<li>$\tt lct$ 有着特别重要的染色模型，也就是用实边代表同色，虚边代表异色，修改就是把一个点到根的路径染色，然后用数据结构维护颜色的方法：<a href="https://www.cnblogs.com/C202044zxy/p/15759821.html" target="_blank">Matches Are Not a Child's Play</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14549454.html" target="_blank">树点涂色</a></li>
<li>延迟贪心，即能不放置关键点则不放置，这样能把关键点放置在最浅的位置：<a href="https://www.cnblogs.com/C202044zxy/p/15645050.html" target="_blank">Squid Game</a></li>
<li>要快速计算虚树的大小时，可以考虑下标为 $\tt dfn$ 序的线段树来维护：<a href="https://www.cnblogs.com/C202044zxy/p/14725359.html" target="_blank">语言</a>；这种用线段树去重的方法还可以应用于字符串中（若干后缀的本质不同前缀，$\tt sam$ 上启发式合并）：<a href="https://www.cnblogs.com/C202044zxy/p/14851541.html" target="_blank">Asterisk Substrings</a>；这类线段树合并的本质是用线段树的结构提供了一个合并顺序：<a href="https://www.cnblogs.com/C202044zxy/p/16448895.html" target="_blank">三角形</a></li>
<li>最大$/$最小边权问题考虑 $\tt kruskal$ 重构树。比如想要求虚树的最大边权，可以转化成重构树上最浅的祖先，即 $\tt dfn$ 序最大点和最小点的 $\tt lca$：<a href="https://www.cnblogs.com/C202044zxy/p/16263777.html" target="_blank">Groceries in Meteor Town</a></li>
<li>路径问题考虑点分治，不要被复杂的形式给诈骗了：<a href="https://www.cnblogs.com/C202044zxy/p/16365571.html" target="_blank">Network</a></li>
</ul>

<h3 id="233-常见模型">2.3.3 常见模型</h3>

<ul>
<li>连通块问题的常用解决方法：可以在连通块的最浅点统计这个连通块的贡献，那么用树形 $dp$ 来规划这个最浅点即可：<a href="https://www.cnblogs.com/C202044zxy/p/15900959.html" target="_blank">切树游戏</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15527778.html" target="_blank">Ridiculous Netizens</a>；还可以用 <code>点数-边数=1</code> 的经典容斥：<a href="https://www.cnblogs.com/C202044zxy/p/15900959.html" target="_blank">完美的集合</a>；选取连通块的代表点时，可以套用点分树这种分治结构：<a href="https://www.cnblogs.com/C202044zxy/p/16458890.html" target="_blank">成都七中</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15527778.html" target="_blank">Ridiculous Netizens</a></li>
<li>还原树的问题中，可以分步还原，也就是先还原特殊点的结构，再还原整体的结构：<a href="https://www.cnblogs.com/C202044zxy/p/15906197.html" target="_blank">Restoring Map</a>；<a href="https://www.cnblogs.com/C202044zxy/p/15224854.html" target="_blank">New Year and Forgotten Tree</a></li>
<li>边权和贡献最大问题可以往长链剖分这个角度考虑，如果是路径问题可以通过 $2k$ 个叶子的构造性结论转化成最长链问题（前提是需要定根）：<a href="https://www.cnblogs.com/C202044zxy/p/15572977.html" target="_blank">Spiders Evil Plan</a></li>
<li>巧妙利用树上结构，如可以用重链剖分的结构来快速定位：<a href="https://www.cnblogs.com/C202044zxy/p/15326159.html" target="_blank">Nauuo and Binary Tree</a>；若是多个数通过操作合并为一个数，可以思考操作的树形结构：<a href="https://www.cnblogs.com/C202044zxy/p/15886771.html" target="_blank">Eternal Average</a>；括号序列的树形结构是重要的：<a href="https://www.cnblogs.com/C202044zxy/p/16187444.html" target="_blank">[省选联考 2022] 序列变换</a>（差点因为这题没进队）</li>
</ul>

<h2 id="24-网络流">2.4 网络流</h2>

有一些入门的内容，不想整理了：<a href="https://www.cnblogs.com/C202044zxy/p/14284215.html" target="_blank">网络流简单题选做</a>；<a href="https://www.cnblogs.com/C202044zxy/p/14272032.html" target="_blank">网络流二十四题</a>

把下面这些整理完之后，我才发现好像网络流确实已经很难有新意了，很多 $\tt trick$ 都是反复出现的。

<h3 id="241-量的意义">2.4.1 量的意义</h3>

<ul>
<li>用流量的流入和流出代表加减，可以表示一些加减的不等式关系：<a href="https://www.cnblogs.com/C202044zxy/p/15811266.html" target="_blank">Red-Blue Graph</a></li>
<li>流量可以在基于要求的情况下，表示解决要求的途径，而费用可以看成途径的代价：<a href="https://www.cnblogs.com/C202044zxy/p/15519340.html" target="_blank">Chips Challenge</a></li>
<li>可以用路径来表达不合法的关系，再用最小割来获取最优解：<a href="https://www.cnblogs.com/C202044zxy/p/14939660.html" target="_blank">Starry Night Camping</a>；<a href="https://www.cnblogs.com/C202044zxy/p/16063441.html" target="_blank">老C的方块</a></li>
<li>对于覆盖类型的限制，把需要覆盖的点串联起来，用路径表示覆盖的关系：<a href="https://www.cnblogs.com/C202044zxy/p/16255643.html" target="_blank">奇怪的线段树</a></li>
</ul>

<h3 id="242-观察性质">2.4.2 观察性质</h3>

<ul>
<li>如果用网络流解决平面图问题，可以考虑黑白染色转二分图的性质：<a href="https://www.cnblogs.com/C202044zxy/p/15947433.html" target="_blank">过山车</a></li>
<li>网络流解决矩阵问题，可以用行列来建二分图：；但这有时候是个思维定式，如果行和列独立并且有其他限制，那么建单层图能更好地表达限制：<a href="https://www.cnblogs.com/C202044zxy/p/14381040.html" target="_blank">Asa's Chess Problem</a></li>
<li>完美匹配的等价条件是 $\tt Hall$ 定理，所以如果原问题和 $\tt Hall$ 有某种关联可以通过这层关系转化到网络流上：<a href="https://www.cnblogs.com/C202044zxy/p/15863869.html" target="_blank">Construction of a tree</a></li>
<li>拆分法，网络流一般只能解决单点对单点的限制，如果是多点对单点，那么大胆找结论，我们可以从答案的形式入手：<a href="https://www.cnblogs.com/C202044zxy/p/15510650.html" target="_blank">Rainbow Triples</a>；还有对代价的拆分，比如绝对值的微元贡献法：<a href="https://www.cnblogs.com/C202044zxy/p/15165965.html" target="_blank">One Billion Shades of Grey</a></li>
<li>如果某个问题可以建出其费用流模型，那么是有凸性的，就引申到了 $\tt wqs$ 之类的算法：<a href="https://www.cnblogs.com/C202044zxy/p/14604078.html" target="_blank">April Fools' Problem</a></li>
</ul>

<h3 id="243-小技巧">2.4.3 小技巧</h3>

<ul>
<li>单点的多重意义，可以考虑拆点，通常可以让限制表达得更明晰：<a href="https://www.cnblogs.com/C202044zxy/p/15947433.html" target="_blank">过山车</a>，<a href="https://www.cnblogs.com/C202044zxy/p/15930914.html" target="_blank">Making It Bipartite</a></li>
<li>动态网络流，如果有多个图但是差别不大，可以把多出来的边退流：<a href="https://www.cnblogs.com/C202044zxy/p/15895541.html" target="_blank">模拟赛7-C</a>；<a href="https://www.cnblogs.com/C202044zxy/p/15165965.html" target="_blank">One Billion Shades of Grey</a></li>
<li>手算最小割，常常需要枚举法和数据结构维护：<a href="https://www.cnblogs.com/C202044zxy/p/15510650.html" target="_blank">Rainbow Triples</a>；也可以对于点所属的颜色来 $dp$：<a href="https://www.cnblogs.com/C202044zxy/p/15124279.html" target="_blank">Breadboard Capacity</a>；手算最大流也常用，使用结论然后数据结构维护：<a href="https://www.cnblogs.com/C202044zxy/p/14872686.html" target="_blank">k-Maximum Subsequence Sum</a></li>
<li>保留有用的边，减少边数：<a href="https://www.cnblogs.com/C202044zxy/p/15350456.html" target="_blank">Bridge Club</a>；划分等价类，减少点数：<a href="https://www.cnblogs.com/C202044zxy/p/15226591.html" target="_blank">小埋与游乐场</a></li>
<li>优化建图，本质和图论的优化建图是同一类方法，$cdq$ 优化建图：<a href="https://www.cnblogs.com/C202044zxy/p/14381661.html" target="_blank">通信</a>；主席树优化建图：<a href="https://www.cnblogs.com/C202044zxy/p/14378425.html" target="_blank">a+b Problem</a></li>
</ul>

<h3 id="244-图匹配问题">2.4.4 图匹配问题</h3>

<ul>
<li>一般图匹配也许可以通过结论转化成二分图匹配（左部右部的匹配次数相同）：<a href="https://www.cnblogs.com/C202044zxy/p/15881238.html" target="_blank">模拟赛5-A</a></li>
<li>一些情况下贪心地匹配：<a href="https://www.cnblogs.com/C202044zxy/p/15897460.html" target="_blank">Add to Square</a>（达到构造的界）；<a href="https://www.cnblogs.com/C202044zxy/p/15837755.html" target="_blank">模拟赛2-C</a>（特殊的偏序关系）</li>
<li>二分图的独立性（只需要考虑某一部的具体情况）：<a href="https://www.cnblogs.com/C202044zxy/p/15477707.html" target="_blank">Bipartite Blanket</a></li>
<li>不能走回头路的博弈问题可以考察二分图匹配相关的结论，这是因为交错路具有特殊的数量关系：<a href="https://www.cnblogs.com/C202044zxy/p/15477325.html" target="_blank">游戏</a></li>
<li>经典问题：有向图求环划分，可以拆成入点和出点跑二分图匹配：<a href="https://www.cnblogs.com/C202044zxy/p/14611845.html" target="_blank">边</a></li>
</ul>

<h3 id="245-常见模型">2.4.5 常见模型</h3>

<ul>
<li>混合图的欧拉回路，思考清楚度数在原问题中对应着什么即可套用此模型：<a href="https://www.cnblogs.com/C202044zxy/p/16008733.html" target="_blank">wait</a></li>
<li>最大权闭合子图，可以解决带强制选取关系的选$/$不选问题，或者也可以解决类似的二选一问题：<a href="https://www.cnblogs.com/C202044zxy/p/15933535.html" target="_blank">魔法商店</a></li>
<li>最长反链，转最小链覆盖之后，用 $\tt dfs$ 的方法构造即可：<a href="https://www.cnblogs.com/C202044zxy/p/15924584.html" target="_blank">Birthday</a>；也有构造新的偏序集再跑最长反链的题目：<a href="https://www.cnblogs.com/C202044zxy/p/15930914.html" target="_blank">Making It Bipartite</a></li>
<li>上下界网络流，可以表示一些强制限制：<a href="https://www.cnblogs.com/C202044zxy/p/14770466.html" target="_blank">Showing Off</a>；<a href="https://www.cnblogs.com/C202044zxy/p/14349814.html" target="_blank">带上下界的网络流</a></li>
</ul>

<h1 id="3unknown">3.unknown</h1>

<h2 id="31-计数">3.1 计数</h2>

<h3 id="311-问题转化">3.1.1 问题转化</h3>

<ul>
<li>映射法，遇到多对一的问题时（多种方案生成同构的结果），可以通过添加限制把多化一，构成双射：<a href="https://www.cnblogs.com/C202044zxy/p/16102529.html" target="_blank">Two Histograms</a>；可以根据数感，和常见的结构建立映射关系：<a href="https://www.cnblogs.com/C202044zxy/p/16004751.html" target="_blank">神必的集合</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15058538.html" target="_blank">Slime and Sequences</a>；遇到信息题时（确定未知变量），考虑我们都知道什么信息，和信息建立映射关系：<a href="https://www.cnblogs.com/C202044zxy/p/15994415.html" target="_blank">Bears and Juice</a></li>
<li>组合意义法，如果方案数是加权的，那么思考权值的组合意义，最好化为存粹的方案数：<a href="https://www.cnblogs.com/C202044zxy/p/16107610.html" target="_blank">Min Product Sum</a>；<a href="https://www.cnblogs.com/C202044zxy/p/15238558.html" target="_blank">教数学的校长</a>；组合意义尽量合并多步计数（即把原来复杂的结果化为一个过程）：<a href="https://www.cnblogs.com/C202044zxy/p/16353264.html" target="_blank">盒</a>；对于外层的枚举可以建立虚点来等效替换：<a href="https://www.cnblogs.com/C202044zxy/p/16446842.html" target="_blank">数叶子</a>；逆向应用一些定理也是组合意义的一个方向：<a href="https://www.cnblogs.com/C202044zxy/p/16463153.html" target="_blank">无损加密</a></li>
<li>把限制转化成单侧的，这样计数顺序会更明显：<a href="https://www.cnblogs.com/C202044zxy/p/16284676.html" target="_blank">Centroid Probabilities</a></li>
<li>切换计数对象，简化计数的主体：<a href="https://www.cnblogs.com/C202044zxy/p/16377966.html" target="_blank">stairs</a>；去除不易处理的限制：<a href="https://www.cnblogs.com/C202044zxy/p/16379754.html" target="_blank">count</a>；添加限制转化为容易计数的对象：<a href="https://www.cnblogs.com/C202044zxy/p/16387741.html" target="_blank">Two Pieces</a></li>
</ul>

<h3 id="312-容斥反演">3.1.2 容斥/反演</h3>

<ul>
<li>最常见的容斥方法是枚举不合法的个数：<a href="https://www.cnblogs.com/C202044zxy/p/16102529.html" target="_blank">Two Histograms</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16371128.html" target="_blank">神经网络</a></li>
<li>如果存在选取个数 $\leq a_i$ 的限制，可以强制选取 $a_i+1$ 个，然后记上 $-1$ 的容斥系数：<a href="https://www.cnblogs.com/C202044zxy/p/15918930.html" target="_blank">钥匙</a></li>
<li>$\tt LGV$ 引理，原本求的是偶数逆序对的不交路径减去奇数逆序对的不交路径。在特殊的图中（比如网格图，数轴）可以直接求不交路径数量：<a href="https://www.cnblogs.com/C202044zxy/p/16463153.html" target="_blank">无损加密</a>，如果终点未知还可以考虑 $dp$ 计算行列式：<a href="https://www.cnblogs.com/C202044zxy/p/15205953.html" target="_blank">Random Robots</a></li>
<li>容斥的另一种思考方向，先给出一种会算重的计数方法，然后找出这种计数方法算重的原因，然后对这个算重的原因容斥：<a href="https://www.cnblogs.com/C202044zxy/p/16277483.html" target="_blank">树</a>；比如经典模型 $\tt DAG$ 计数，就是容斥入度为 $0$ 的点：<a href="https://www.cnblogs.com/C202044zxy/p/15116570.html" target="_blank">Finding satisfactory solutions</a>；类似的基环树计数，可以容斥叶子：<a href="https://www.cnblogs.com/C202044zxy/p/16487647.html" target="_blank">人类补完计划</a></li>
<li>集合划分容斥，用于解决相等关系的限制，直接对边数容斥即可：<a href="https://www.cnblogs.com/C202044zxy/p/15309579.html" target="_blank">Distinct Multiples</a></li>
</ul>

<h3 id="313-统计方法">3.1.3 统计方法</h3>

<ul>
<li>思考答案的形式，然后思考在哪里统计，如果答案是区间则可以在端点处统计：<a href="https://www.cnblogs.com/C202044zxy/p/16042628.html" target="_blank">Chords</a>；如果答案是树上的点集，那么可以在其 $\tt lca$ 处统计（写成代码就是在合并子树的时候统计）：<a href="https://www.cnblogs.com/C202044zxy/p/15042761.html" target="_blank">Vladislav and a Great Legend</a></li>
<li>思考统计的顺序，确定合理的顺序可以让式子变得更简单，通常有偏序关系就意味着可能需要思考顺序：<a href="https://www.cnblogs.com/C202044zxy/p/16015348.html" target="_blank">Inversions</a>；分步统计，逐步确定的方法也是重要的：<a href="https://www.cnblogs.com/C202044zxy/p/15918930.html" target="_blank">钥匙</a>；为保证顺序，还可以在一次计数中结合不同的计数方法：<a href="https://www.cnblogs.com/C202044zxy/p/15845375.html" target="_blank">模拟赛3-A</a>；可以先确定一个大致的顺序，然后在转移时修正它：<a href="https://www.cnblogs.com/C202044zxy/p/16411931.html" target="_blank">Square Constraints</a></li>
<li>关于去重方法，可以考虑给同构的方案增加限制：<a href="https://www.cnblogs.com/C202044zxy/p/15957474.html" target="_blank">Piling Up</a>；比较无脑的去重方法，考虑一种方案会被计算多少次，然后用除法去重：<a href="https://www.cnblogs.com/C202044zxy/p/15845858.html" target="_blank">Fox And Travelling</a>；如果要考虑无序的子结构，可以考虑隔板法去重：<a href="https://www.cnblogs.com/C202044zxy/p/15398552.html" target="_blank">Shake It!</a></li>
</ul>

<h3 id="314-常见模型">3.1.4 常见模型</h3>

<ul>
<li>卡塔兰数模型，可以解决在网格图中行走但是不越过某条直线的方案数：<a href="https://www.cnblogs.com/C202044zxy/p/16000636.html" target="_blank">Ball Eat Chameleons</a></li>
<li>斯特林数模型，可以在 $n$ 大 $k$ 小的情况下优化计算 $n^k$：<a href="https://www.cnblogs.com/C202044zxy/p/15042761.html" target="_blank">Vladislav and a Great Legend</a></li>
<li>本质不同字符串计数的问题，直接考虑建立 $\tt DFA$：<a href="https://www.cnblogs.com/C202044zxy/p/15571493.html" target="_blank">Strange Operation</a></li>
</ul>

<h3 id="315-概率期望">3.1.5 概率期望</h3>

<ul>
<li>注意利用等概率的性质，比如转等概率环，可以把总体的方案放到单点上去：<a href="https://www.cnblogs.com/C202044zxy/p/14825837.html" target="_blank">Wine Thief</a>、也可以方便地计算总方案数：<a href="https://www.cnblogs.com/C202044zxy/p/14819023.html" target="_blank">AmShZ Farm</a></li>
<li>如果题目保证了出现的概率，那么这题可能就是基于概率来寻找关键点：<a href="https://www.cnblogs.com/C202044zxy/p/15158524.html" target="_blank">James and the Chase</a></li>
<li>拆分。独立变量的概率常常可以拆分，这样可以分步解决问题：<a href="https://www.cnblogs.com/C202044zxy/p/16326908.html" target="_blank">Strongly Connected Tournament</a>；概率对期望的拆分十分重要，可以完成期望对概率的转化：<a href="https://www.cnblogs.com/C202044zxy/p/14993601.html" target="_blank">Shuffles Cards</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16072724.html" target="_blank">Gachapon</a></li>
<li>概率期望类型的题目中，一些前缀 $/$ 差分类型的转化往往能够简化问题：<a href="https://www.cnblogs.com/C202044zxy/p/16044998.html" target="_blank">地震后的幻想乡</a></li>
</ul>

<h2 id="32-基础方法">3.2 基础方法</h2>

<h3 id="321-势能法均摊法">3.2.1 势能法/均摊法</h3>

<ul>
<li>适合势能法的题目有一些特征，比如覆盖问题：<a href="https://www.cnblogs.com/C202044zxy/p/16049898.html" target="_blank">小Z与函数</a>；染色问题：<a href="https://www.cnblogs.com/C202044zxy/p/15008306.html" target="_blank">Intervals of Intervals</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16331231.html" target="_blank">亿块田</a>（位运算可以看成数位的颜色段均摊）；区间合并与分裂问题（通常是维护的东西具有单调性，可以把值相同的一段看成区间的题目）：<a href="https://www.cnblogs.com/C202044zxy/p/15915211.html" target="_blank">货币</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15473246.html" target="_blank">Holy Diver</a></li>
<li>使用势能法时，可以从一些感性的角度，通过定义势能函数来入手：<a href="https://www.cnblogs.com/C202044zxy/p/15252239.html" target="_blank">Souvenirs</a></li>
<li>尽管有时候操作比较复杂，但是若是具有 <code>某变量一定减少</code> 的性质，就可以通过加速一些简单的过程来优化：<a href="https://www.cnblogs.com/C202044zxy/p/16032635.html" target="_blank">Addition and Andition</a>；可以通过一些简单的操作变换，使得减少量和花费的时间之间对应起来：<a href="https://www.cnblogs.com/C202044zxy/p/15398834.html" target="_blank">五彩斑斓的世界</a></li>
<li>势能线段树，可以维护区间取 $\min$、区间除法等操作：<a href="https://www.cnblogs.com/C202044zxy/p/15182641.html" target="_blank">浅谈势能线段树在特殊区间问题上的应用</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15483652.html" target="_blank">Cartesian Tree</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15350456.html" target="_blank">Stations</a></li>
<li>均摊法最重要的是分析总体复杂度，所以一些暴力的操作可能看起来很慢但是总体复杂度优秀：<a href="https://www.cnblogs.com/C202044zxy/p/15458447.html" target="_blank">Berland Miners</a></li>
</ul>

<h3 id="322-拆分法">3.2.2 拆分法</h3>

使用拆分法时，最重要的是保证独立性。

<ul>
<li>如果题目中存在时间顺序，可以把这个顺序破除来保证独立性（即换一个顺序计算）：<a href="https://www.cnblogs.com/C202044zxy/p/16032635.html" target="_blank">Addition and Andition</a></li>
<li>可以对左右端点拆分来优化，比如应用到区间 $dp$ 问题：<a href="https://www.cnblogs.com/C202044zxy/p/15994415.html" target="_blank">Student's Camp</a>；可以拆分之后分别计算：<a href="https://www.cnblogs.com/C202044zxy/p/15483652.html" target="_blank">Cartesian Tree</a></li>
<li>高维问题可以考虑拆分成独立的低维问题：<a href="https://www.cnblogs.com/C202044zxy/p/15869034.html" target="_blank">Berserk Robot</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15367129.html" target="_blank">Jumping sequence</a></li>
<li>拆分法可以用来集中限制，比如把两个主体间的限制拆开，等效到一个主体上：<a href="https://www.cnblogs.com/C202044zxy/p/15848141.html" target="_blank">Drazil and His Happy Friends</a>；拆分法还可以分解限制，可以把区间限制拆开，分解到单点上：<a href="https://www.cnblogs.com/C202044zxy/p/15837755.html" target="_blank">模拟赛2-B</a></li>
<li>线段树可以理解成对二进制数位的拆分，有些题目可能利用线段树来拆分：<a href="https://www.cnblogs.com/C202044zxy/p/15799307.html" target="_blank">Xor-Set</a></li>
</ul>

<h3 id="323-调整法">3.2.3 调整法</h3>

调整法的使用过程是：选取初始状态（<a href="https://www.cnblogs.com/C202044zxy/p/16260889.html" target="_blank">Sergey's problem</a>）、确定调整方式。

<ul>
<li>有很多匹配问题具有神奇结论，证明可以考虑调整法：<a href="https://www.cnblogs.com/C202044zxy/p/16011707.html" target="_blank">Modulo Pairing</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15054014.html" target="_blank">Bear and Cavalry</a></li>
<li>如果某问题连判定合法性都困难，<s>还要求你最优化的话</s>，那么大胆使用调整法。从最优状态开始调整，使用最小代价来达成合法性（<s>这样可以两个愿望一次满足</s>）：<a href="https://www.cnblogs.com/C202044zxy/p/15957474.html" target="_blank">Two Faced Cards</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15519340.html" target="_blank">Chips Challenge</a></li>
<li>邓老师调整法，从一个合法状态开始，然后对它进行微调使得权值变大 $/$ 变小：<a href="https://www.cnblogs.com/C202044zxy/p/15933902.html" target="_blank">邓老师调整法</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15542920.html" target="_blank">Pastoral Oddities</a></li>
<li>可以用调整法来研究最优解满足的性质，这时候使用一些简单的不等关系是重要的：<a href="https://www.cnblogs.com/C202044zxy/p/15918930.html" target="_blank">遇到困难睡大觉</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15815703.html" target="_blank">转盘</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15074110.html" target="_blank">Max Correct Set</a></li>
<li>两种决策的问题可以考虑<strong>主一副二</strong>的方法，先固定一个，然后用另一个调整以获得最优解：<a href="https://www.cnblogs.com/C202044zxy/p/14968568.html" target="_blank">Squares</a></li>
<li>连续型问题向离散型问题的转化，可以通过调整法证明只会取到端点值：<a href="https://www.cnblogs.com/C202044zxy/p/16202461.html" target="_blank">Parametric MST</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16308106.html" target="_blank">Levko and Game</a></li>
</ul>

<h3 id="324-分治法倍增法">3.2.4 分治法/倍增法</h3>

<ul>
<li>矩形问题可以考虑交替分治，加上点双指针单调性什么的可以做到优秀复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/15516918.html" target="_blank">Empty Rectangles</a></li>
<li>类似后缀数组的倍增技巧，优化的重点是充分利用上一层的信息：<a href="https://www.cnblogs.com/C202044zxy/p/16036528.html" target="_blank">Minimal String Xoration</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15375981.html" target="_blank">月球列车</a></li>
<li>使用倍增法的重要步骤是结合单次询问分析，找到关键的固定的可加速过程，然后倍增：<a href="https://www.cnblogs.com/C202044zxy/p/15918930.html" target="_blank">麻烦的杂货店</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14842025.html" target="_blank">Hopping Around the Array</a></li>
<li>倍增是基于二进制的，所以某些异或问题用倍增有奇效：<a href="https://www.cnblogs.com/C202044zxy/p/15369329.html" target="_blank">温故而知新</a></li>
<li>确定唯一的后继就可以倍增，可以认为地让这个后继满足一些优良性质：<a href="https://www.cnblogs.com/C202044zxy/p/16484442.html" target="_blank">唱诗</a></li>
</ul>

<h3 id="325-神奇复杂度">3.2.5 神奇复杂度</h3>

<ul>
<li>只保留有用的点 $/$ 点对再暴力计算，可以从一些基本的不等关系入手：<a href="https://www.cnblogs.com/C202044zxy/p/15775729.html" target="_blank">Weighted Increasing Subsequences</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16004751.html" target="_blank">法阵</a></li>
<li>动态的扩展状态，只考虑用得到的状态，在总状态数特别少的情况下特别有用：<a href="https://www.cnblogs.com/C202044zxy/p/15156174.html" target="_blank">A Serious Referee</a></li>
<li><code>bitset</code>，一个开挂般的存在，没有减少任何计算量，但是却常常是有效的优化。字符串匹配问题常常可以套用 <code>bitset</code>：<a href="https://www.cnblogs.com/C202044zxy/p/16258252.html" target="_blank">Substrings in a String</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16453521.html" target="_blank">strings</a>；利用 <code>bitset</code> 来递推：<a href="https://www.cnblogs.com/C202044zxy/p/16270722.html" target="_blank">New Year and the Tricolore Recreation</a>；利用 <code>bitset</code> 加速对应位置异或：<a href="https://www.cnblogs.com/C202044zxy/p/16394834.html" target="_blank">区间距离</a></li>
<li>找到某个能让值域翻倍$/$折半的关键元素，可以优化暴力的复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/15369362.html" target="_blank">大套子</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16332625.html" target="_blank">Phoenix and Diamonds</a></li>
<li>四毛子思想。直接对原序列分块，对于每一块内处理出所有子集的信息，可以平衡复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/16453521.html" target="_blank">strings</a></li>
</ul>

<h3 id="326-根号相关">3.2.6 根号相关</h3>

<ul>
<li>总和一定时，注意种类 $\sqrt n$ 的结论：<a href="https://www.cnblogs.com/C202044zxy/p/16036528.html" target="_blank">Snowy Mountain</a>、<a href="https://www.cnblogs.com/C202044zxy/p/15175343.html" target="_blank">Tree Degree Subset Sum</a></li>
<li>值域分治。在位运算和四则运算混合时，预处理时先最大化前一半的数位，再最大化后一半的数位，预处理和询问平衡做到 $O(n\sqrt n)$ 之类的复杂度：<a href="https://www.cnblogs.com/C202044zxy/p/15787730.html" target="_blank">In a Trap</a>；暴力前一半，状压后一半，可以做到 $O(\sqrt n)$：<a href="https://www.cnblogs.com/C202044zxy/p/16292793.html" target="_blank">进制转换</a>；值域分块带有天然的偏序关系，在偏序关系限制比较严格的时候可以尝试使用：<a href="https://www.cnblogs.com/C202044zxy/p/16289009.html" target="_blank">Set Merging</a></li>
<li>根号分治。把种类按照出现次数分类已经是老生常谈了，但是注意分治后的情况下具有的性质：<a href="https://www.cnblogs.com/C202044zxy/p/16277483.html" target="_blank">众数</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16281427.html" target="_blank">Huffman Coding on Segment</a>；序列跳跃问题可以直接对后继的距离根号分治：<a href="https://www.cnblogs.com/C202044zxy/p/16299969.html" target="_blank">Summer Oenothera Exhibition</a></li>
<li>操作分块。结构变化且复杂的题目可以考虑每 $O(\sqrt n)$ 个操作分成一块，分别处理。注意这样转化之后结构会变简单，可以把一些复杂的操作用暴力来实现：<a href="https://www.cnblogs.com/C202044zxy/p/16332625.html" target="_blank">Jumping Through the Array</a></li>
</ul>

<h3 id="327-贡献法">3.2.7 贡献法</h3>

贡献法最基础的用法就是改变和式的计算方式，使用贡献法时，首先确定贡献对象，然后思考这个对象在什么情况下会产生多大的贡献：<a href="https://www.cnblogs.com/C202044zxy/p/16270722.html" target="_blank">Move by Prime</a>

<ul>
<li>多种情况下要统一贡献形式，可能需要钦定合适的贡献方式：<a href="https://www.cnblogs.com/C202044zxy/p/16348877.html" target="_blank">钥匙</a>（可以贪心匹配上的就贡献）</li>
<li>基于大小关系比较的题目中，可以考虑使用 <code>01-principle</code>，即先考虑 $01$ 序列的情况，再用贡献法推广：<a href="https://www.cnblogs.com/C202044zxy/p/16492266.html" target="_blank">线段树</a></li>
</ul>

<h3 id="328-随机化">3.2.8 随机化</h3>

<ul>
<li>扩大值域以减小出错概率。如果想通过行列式表示积和式的一些性质（比如是否为 $0$），那么可以扩大值域，直接计算行列式，出错的概率是极小的：<a href="https://www.cnblogs.com/C202044zxy/p/16331231.html" target="_blank">亿些整理</a></li>
<li>在出现频率差距较大时，随机抽样调查若干次可以获得想要的结果：<a href="https://www.cnblogs.com/C202044zxy/p/16335876.html" target="_blank">Olha and Igor</a></li>
</ul>

<h3 id="329-枚举法">3.2.9 枚举法</h3>

<ul>
<li>适当的枚举有助于考虑限制，比如点不交的问题中，可以枚举不交的那个点：<a href="https://www.cnblogs.com/C202044zxy/p/16416921.html" target="_blank">Path</a></li>
<li>用枚举法向简单问题化归：<a href="https://www.cnblogs.com/C202044zxy/p/16435769.html" target="_blank">Prefix Suffix Addition</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14653420.html" target="_blank">制作菜品</a></li>
<li>切换枚举对象，前提是分析清楚枚举过程：<a href="https://www.cnblogs.com/C202044zxy/p/16475708.html" target="_blank">鸽子固定器</a></li>
</ul>

<h3 id="3210-贪心法">3.2.10 贪心法</h3>

<ul>
<li>
我们使用贪心时，尽可能要单一化贪心的对象，独立化贪心的代价。这需要我们观察代价的特点，使用拆分法对代价进行一些处理：<a href="https://www.cnblogs.com/C202044zxy/p/16187444.html" target="_blank">[省选联考 2022] 序列变换</a>；贪心对象多化一的处理：<a href="https://www.cnblogs.com/C202044zxy/p/16387741.html" target="_blank">新年的腮雷</a>
</li>
<li>
统一代价的形式，这样更便于贪心的使用：<a href="https://www.cnblogs.com/C202044zxy/p/16202461.html" target="_blank">Parametric MST</a>；统一操作的形式，也便于贪心：<a href="https://www.cnblogs.com/C202044zxy/p/16270722.html" target="_blank">Game Relics</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16356234.html" target="_blank">Escape</a>
</li>
<li>
树上依赖贪心。如果存在先选父亲才能选儿子的限制时，弄出合并规则然后用堆维护：<a href="https://www.cnblogs.com/C202044zxy/p/16448895.html" target="_blank">三角形</a>、<a href="https://blog.csdn.net/C202044zxy/article/details/109259656" target="_blank" rel="noopener nofollow">牛半仙的魔塔</a>
</li>
<li>
如果代价具有凸性，那么可以会指向堆贪心之类的做法：<a href="https://www.cnblogs.com/C202044zxy/p/16521066.html" target="_blank">WYR-Leveling Ground</a>
</li>
</ul>

<h2 id="33-数据结构">3.3 数据结构</h2>

这里并不是对数据结构的系统总结，而是总结了一些有意思的思维点，可能对你做数据结构有帮助，但是前提还是有扎实的数据结构基础，能对任何结构信手拈来。

<h3 id="331-问题转化">3.3.1 问题转化</h3>

<ul>
<li>写出操作的线性变换形式，就可以直接套用矩阵：<a href="https://www.cnblogs.com/C202044zxy/p/16218568.html" target="_blank">密码箱</a></li>
<li>删除操作，可以看成回退到上一时刻，可以用保留所有历史版本的主席树来支持回退：<a href="https://www.cnblogs.com/C202044zxy/p/16255643.html" target="_blank">火车管理</a></li>
<li>对一段区间内的分段函数求和，可以对把自变量当成版本，以位置来建立主席树：<a href="https://www.cnblogs.com/C202044zxy/p/16455138.html" target="_blank">Tower Defense</a></li>
<li>对于强制在线的问题，可以先思考离线怎么处理，然后套上可持久化数据结构：<a href="https://www.cnblogs.com/C202044zxy/p/16375233.html" target="_blank">区间第k小</a></li>
</ul>

<h3 id="332-考虑限制">3.3.2 考虑限制</h3>

<ul>
<li>切换限制的主体。很多时候从题目的方向直接翻译是行不通的，这时候弄清限制涉及到了哪些对象，然后通过选取其他对象的形式来重新表述限制，就达到了切换限制主体的效果：<a href="https://www.cnblogs.com/C202044zxy/p/16236683.html" target="_blank">铃原露露</a></li>
<li>保留有效限制。限制之间也存在偏序关系，如果通过一些技巧可以达到排除无效限制的效果，那么就可能把限制的总量规约到一个较小的数量集。比如树上启发式合并来考虑和 $\tt lca$ 有关的限制：<a href="https://www.cnblogs.com/C202044zxy/p/16236683.html" target="_blank">铃原露露</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14160764.html" target="_blank">事情的相似度</a></li>
<li>放宽限制。并不一定要在满足限制时检查，可以找维护一个合适的必要条件，把总检查次数限制在一定范围内：<a href="https://www.cnblogs.com/C202044zxy/p/16398027.html" target="_blank">被创与地震</a></li>
<li>收紧限制。对于限制较弱的问题，可以通过讨论特殊情况，使得要考虑的范围缩小：<a href="https://www.cnblogs.com/C202044zxy/p/16416921.html" target="_blank">Path</a></li>
</ul>

<h3 id="333-确定维护对象">3.3.3 确定维护对象</h3>

<ul>
<li>尽量不要去维护有关特定值的信息，可以通过放宽条件的方式转化为维护最值：<a href="https://www.cnblogs.com/C202044zxy/p/16260889.html" target="_blank">Bear and Bad Powers of 42</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16315198.html" target="_blank">Into Blocks</a></li>
<li>可以通过切换主体的方式来确定维护对象。譬如有 $A\rightarrow B$ 的影响，从 $A$ 的角度看是一种效果，从 $B$ 的角度看又是另一种效果。根据修改与询问的特性变换角度，可以确定最为合适的维护对象：<a href="https://www.cnblogs.com/C202044zxy/p/16260889.html" target="_blank">The Tree</a>；先弄清维护的信息是什么，然后切换主体，保持维护信息的不变即可：<a href="https://www.cnblogs.com/C202044zxy/p/16376035.html" target="_blank">前进四</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16466375.html" target="_blank">诡异操作</a></li>
<li>切换承载信息的主体，前提是原来的主体和新选取的主体之间可以建立对应关系：<a href="https://www.cnblogs.com/C202044zxy/p/16344315.html" target="_blank">Nauuo and ODT</a>；寻找决定性的对象，比如这个对象决定了答案，那么我们就尽力去描述它：<a href="https://www.cnblogs.com/C202044zxy/p/16456930.html" target="_blank">区间和</a></li>
</ul>

<h3 id="334-思考如何维护">3.3.4 思考如何维护</h3>

<ul>
<li>
如果难以对维护对象选取主元，那么考虑对称的维护两个对象：<a href="https://www.cnblogs.com/C202044zxy/p/16515516.html" target="_blank">图论</a>
</li>
<li>
等效操作的思想是很重要的，当你切换维护对象之后，可以通过等效操作来适应现在所维护的东西：<a href="https://www.cnblogs.com/C202044zxy/p/16260889.html" target="_blank">The Tree</a>、<a href="https://www.cnblogs.com/C202044zxy/p/16382822.html" target="_blank">数轴变换</a>
</li>
<li>
整体标记法。思考清楚整体标记对于单点的影响即可：<a href="https://www.cnblogs.com/C202044zxy/p/16400820.html" target="_blank">Latin Square</a>
</li>
<li>
注意维护信息的顺序，比如有的问题只适合以一种顺序加入：<a href="https://www.cnblogs.com/C202044zxy/p/16416921.html" target="_blank">Julia the snail</a>
</li>
</ul>

<h3 id="335-常见模型">3.3.5 常见模型</h3>

<ul>
<li>递归半边模型。其本质是离线与在线的结合，利用维护好的信息每次可以将考虑的区间折半。维护括号匹配：<a href="https://www.cnblogs.com/C202044zxy/p/16416921.html" target="_blank">Nastya and CBS</a>；维护极长上升子序列类的信息：<a href="https://www.cnblogs.com/C202044zxy/p/15815703.html" target="_blank">转盘</a></li>
<li>染色模型。一些类区间赋值类操作可以向染色模型转化：<a href="https://www.cnblogs.com/C202044zxy/p/16218568.html" target="_blank">轻重边</a>；区间求并的问题，可以扫描线加染色模型，也就是维护最晚被染的颜色：<a href="https://www.cnblogs.com/C202044zxy/p/16422164.html" target="_blank">rrusq</a>、<a href="https://www.cnblogs.com/C202044zxy/p/14163522.html" target="_blank">区间本质不同子串个数</a></li>
<li>猫树分治。主要作用是提供一个分治中点，比如可以把分治中点当成历史最大值的起点：<a href="https://www.cnblogs.com/C202044zxy/p/16422164.html" target="_blank">rprmq1</a></li>
<li>二进制分组。可以支持末尾的在线加入，搬到线段树上可以支持末尾删除和区间查询：<a href="https://www.cnblogs.com/C202044zxy/p/16453521.html" target="_blank">Unknown</a></li>
</ul>

