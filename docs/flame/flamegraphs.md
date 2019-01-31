# 火焰图

我们再来看[上一步](./first_analysis)中生成的火焰图

![](https://clinicjs.org/static/60ec54d4c38a25cb8c567ccf71a6c187/65be2/03.png)

先忽略周围的控制面板，专注于理解可视化部分。

在生成火焰图时，我们主要关注三个关键问题：

- 在采样期间，哪些函数相互调用 
- 每个函数在 CPU 上的执行时间
- 每个函数在栈顶的停留时间

这三个问题都可以在可视化部分找到答案。

## 函数间的调用关系（调用栈）

下面的图中，一块代表一个函数调用，每一块都聚合了该函数导致的调用栈。当一个函数块（译者注：为了方便理解，将 block 翻译为函数块）位于另一个函数块的顶部时，说明它被它下面的函数块调用，下面的函数块又被其下面的函数块调用，依此类推。

![](https://clinicjs.org/static/c784a05011433eb4418ae85791697da8/c4232/04-A.png)

在 Clinic 的火焰图中，使用文本和边框的颜色帮助你区分不同来源的代码。白色表示你正在分析的应用中的代码（即由你控制的代码）。蓝色表示 `node_modules` 中的代码，而灰色表示 Node.js 本身的核心代码。

## 函数在 CPU 上的执行时间（函数块宽度）

函数块的宽度代表分析期间，函数在 CPU 上的执行时间。但这段时间内，函数不一定在执行自己的代码，如果在它上面还有函数快，说明它调用了上面的函数并等待它执行完成。

Clinic 火焰图按照函数块的宽度排序，最宽的（执行时间最长的）显示在左边。

![](https://clinicjs.org/static/cb5f24545b483df675c04a361af12edd/c4232/04-B.png)

## 函数在栈顶的停留时间（热度）

函数在栈顶的停留时间等效于：函数阻塞 Node.js 事件循环的时间。如果一个函数经常停留在栈顶，意味着它花费更多的时间执行自己的代码，而不是调用其他函数或者触发函数回调。

在 Node.js 中，同一时间只能执行一个函数（排除 Worker thread 等情况），如果花费大量时间执行某个函数，就不能执行其他代码了，包括触发 I/O 回调。这就是”阻塞事件循环“这个词的本质。

在函数块顶部的亮条的亮度表示函数停留在栈顶时间的百分比。换句话说，越亮（越热）表明执行自己代码的时间越多，从而阻止其他带代码执行。

![](https://clinicjs.org/static/38f13a6ea48ca78ae269acf140dd128d/c4232/04-C.png)

当一个函数阻塞事件循环的概率高于其他函数时，我们将其称为“热”函数。寻找这些“热”函数就是寻找优化代码的好地方。

Clinic 火焰图默认选中”最热的“函数，并提供了切换到下一个”热“函数的控制面板。