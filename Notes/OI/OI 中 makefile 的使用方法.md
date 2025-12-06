## 前言

笔者在 CSP-S 2025 之前突发奇想，想到相比于传统的脚本，makefile 似乎在 Linux 环境下编译、运行、测试大样例、debug 等 OI 比赛中重要的环节有独特的易于编写、便于使用的特点。于是在 CSP-S 2025 与 NOIP 2025 中付诸实践，安利给同校的 OIer，取得了不错的反响。希望谨以此文给 OI 选手的比赛发挥添砖加瓦。

前置知识：基础的 Linux 命令行使用。

## GNU make

援引 [Wikipedia](https://zh.wikipedia.org/wiki/Make) 上的内容：

> Make 是一个在软件开发中所使用的构建工具，用于自动化建构软件。 它通过一个名为 Makefile 的文本文件来描述源代码文件之间的依赖关系和构建规则。 Make 会根据这些规则和依赖关系，判断哪些文件需要重新编译，并执行相应的编译命令，以确保最终生成可执行文件或其他目标文件。
> 
> GNU make 仿照 make 的标准功能（透过 clean-room 工程）重新改写，并加入作者觉得值得加入的新功能，常和 GNU 编译系统一起被使用，是大多数 GNU Linux 安装的一部分。

看上去和 OI 八杆子打不着关系对吧？但 GNU make 给我们提供了三个便捷的特性：

- 在大多数 Linux 发行版（包括 NOI Linux）上都有，无需额外编写内容；
- 可以用 makefile 描述依赖关系，可以适配先编译再测试或者 Subtask 依赖的流程；
- 遇到错误（例如编译错误或者 diff 比较大样例时出错）即停止。

于是我们可以构造相应的 makefile 加快我们调试的流程。

## 构造 makefile

makefile 的基本形式如下：

```makefile
target: dependency1, dependency2, ...
    command1 # 井号可以后面加注释
    command2 # 缩进必须是 \t，不是四个空格。用 VScode 编辑按 tab 键即可
    ...
```

其中 target 是目标名，dependency 是该目标名依赖的先前目标，command 是生成该目标所需要执行的命令。一个最简单的 OI 中使用的 makefile 可能如下所示：

```makefile
compile:
	g++ tree.cpp -std=c++14 -O2 -static # 这里建议复制 pdf 中的编译命令

test1:
	cp tree1.in tree.in
	./a.out
	diff -Zb tree1.ans tree.out

test2:
	cp tree2.in tree.in
	./a.out
	diff -Zb tree2.ans tree.out

test: compile test1 test2
	echo ok all testcases passed
```

以 NOIP 2025 中的 tree 一题为例。把这份内容保存到 `XX-XXXX/tree/makefile` 中，并在 `XX-XXXX/tree/` 目录下运行 `make test`，就会编译并测试所有大样例（本例中只有两个，实际可以自行以此类推添加）。如果中间出现错误（例如编译错误或者 diff 比较大样例时出错），make 会立即停止并输出错误信息。

这份 makefile 的具体作用是：编译程序，将样例 `treeX.in` 复制到 `tree.in`，并且将输出和 `treeX.ans` 进行对比。这样就不需要注释 `freopen` 或者更改 `freopen` 中的文件名造成爆零~~的悲剧~~了。`-Zb` 选项可以忽略行尾空格和空行差异。

如果我们要单独测试第一个大样例，可以运行 `make compile test1`。

如果想要更进一步，可以考虑以下几点：

### debug 编译

有时候我们需要加入 `-fsanitize=address,undefined` 等 debug 选项，可以通过添加一个 `debug` 目标来实现：

```makefile
debugc: 
    g++ tree.cpp -std=c++14 -O2 -D_GLIBCXX_DEBUG -fsanitize=address,undefined -g

debug: debugc test1 test2
    echo ok all testcases passed in debug mode
```

### 限制内存

可以通过 `ulimit -Sv <KB>` 来限制程序的内存使用，例如：

```makefile
test1:
    cp tree1.in tree.in
    ulimit -Sv 524288; ./a.out # 注意 ulimit 必须写在这一行，否则没用
    diff -Zb tree1.ans tree.out
```

### 测试运行时间

可以通过 `time` 命令来测量程序运行时间，例如：

```makefile
test1:
    cp tree1.in tree.in
    time ./a.out
    diff -Zb tree1.ans tree.out
```

### 对拍

可以通过添加一个 `gen` 目标来生成随机数据，然后对拍，例如：

```makefile
gen:
    g++ gen.cpp -o gen -std=c++14 -O2

brute:
    g++ brute.cpp -o brute -std=c++14 -O2

compare:
    ./gen > tree.in
    ulimit -Sv 524288; time ./a.out
    ./brute < tree.in > tree.ans # 可以不加 freopen 使用管道符
    diff -Zb tree.ans tree.out
    make compare # 递归调用 compare 继续对拍，直到出错
```

使用 `make compile gen brute compare` 即可开始对拍。

## 结语

以上便是 makefile 在 OI 比赛中的一些使用方法，说实话可能写的有点混乱，但一定是干货。希望大家能在比赛中用得上，提升调试效率，发挥出更好的水平！

<!-- 写这个文档，英语作业check，剪头发，凯尔希 -->
