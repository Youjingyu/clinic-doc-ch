# 查看分析结果

Clinic Doctor 分析结果主要包含三部分：
> The Clinic Doctor profile has three main sections:

- 警告栏：如果有问题的话，指出应用的主要问题。同时也包含视图控件
> Alert Bar: Points us towards the main problem, if there is one. Also contains View Controls
- 图表：绘制 Doctor 得到的数据
> Graphs: Plot the data from which Doctor is drawing its conclusions
- 建议面板：有关问题的详细说明，并提供接下来的步骤
> Recommendations Panel: Detailed explanation of the problem with next steps

## 警告栏

![](https://clinicjs.org/static/36bf05d98947002e03836e15c083df3a/ace55/04-A.png)

点击此按钮打开它，我们会看到应用的主要问题的一行摘要（如果有的话）。将鼠标悬停在此摘要上，它将突出显示 Doctor 认为与分析该问题最相关的特定图表的标题。
> Click on this to open it out, and we see a one-line summary of the main problem, if there is one. Hover over this summary and it will underline the title of the specific graph that Doctor thinks is most relevant to understanding the problem.

Doctor 通常不会同时识别多个问题，因此这里通常只显示一个问题，或者说没有发现任何问题。这是因为一个性能问题可能会破坏诊断另一个问题所需的数据。例如，如果 Doctor 确定存在事件循环问题，则可能无法获取足够的数据来判断是否存在 I/O 问题。
> Doctor does not generally identify more than one issue, so there will generally be either one problem here, or a note that no problems were found. This is because one performance problem can disrupt the data needed to diagnose another problem. For example, if Doctor is sure there is an event loop problem, it might not be able to take enough readings to judge if there is an I/O problem.

第一次使用的用户主要使用警告栏查看是否存在问题，然后直接查看建议面板中的说明以更好地理解它。熟练的用户会看出常见问题，然后研究适当的图表，以寻找特定场景下造成问题的原因。
> A first-time user will mainly use the Alert Bar to see if there is a detected problem or not, before going straight to the description in the Recommendations Panel to understand it better. A more experienced user will recognise common detected problems and then study the appropriate graphs for clues about how this particular example of the problem is manefesting itself.

在上面的例子中，Doctor 告诉我们它检测到的问题是 “潜在的事件循环问题”，可以在 Event Loop Delay 图中看到：
> In the above example, Doctor is telling us that it has detected a problem, and the problem is "a potential Event Loop issue", which can be seen in the Event Loop Delay graph:

![](https://clinicjs.org/static/c0a8148579033ba7d12a334fc122d845/ace55/04-B.png)

## 视图控件

警报栏的右侧有两个按钮可以更改视图：
> To the right of the Alert Bar there are two buttons to change the view:

![](https://clinicjs.org/assets/images/doctor-docs-04-C.png) 列表视图/网格视图。默认情况下，Doctor 在网格视图中显示所有图形，以便可以立即在屏幕上看到它们。如果通过这个按钮切换到列表视图，每个图形都会占据屏幕的整个宽度。这对于想要研究图表细节的高级用户非常有用。在列表视图中，单击警告栏中描述的问题会将页面向下滚动到最相关的图形（如果它不在视窗中的话）。
> Grid View / List View. By default, Doctor shows all graphs in a grid so they can all be seen on the screen at once. This button switches to a List View where each graph takes the full width of the screen. This can be useful for advanced users who might want to study the detail of the graphs. When in List View, clicking on the problem described in the Alert Bar scrolls the page down to the most relevant graph, if it is not in view.

![](https://clinicjs.org/static/a488fa25a507f7684c86653986efb60e/d582b/04-E.png)

![](https://clinicjs.org/assets/images/doctor-docs-04-D.png) 白间主题/深色主题。默认情况下，Doctor 使用深色背景和浅色文本的主题。这有利于减少眩光，但有些情况（或某些个人喜好），具有浅色背景和深色文字的主题更好。例如，我们可能在希望拍摄打印在纸上的屏幕截图时，或者在光线充足且难以阅读黑暗主题的房间中投屏 Doctor 的分析结果时切换到白间主题。
> Light Theme / Dark Theme. By default, Doctor uses a theme with a dark background and light text. This is good for reducing glare, but some in situations (or for some individual preferences), a theme with a light background and dark text is better. For example, we will probably want to switch to the Light Theme when taking screenshots that will be printed on paper, or when projecting a Doctor profile in a well-lit room where the dark theme is hard to read.

![](https://clinicjs.org/static/4303a4129bb57240bd7f68fda00a41a1/ace55/04-F.png)

## 图表

这些图绘制了 Doctor 分析中使用的各种变量随时间的变化情况，即从分析结果中的开始时间（X 轴的左端）到结束时间（X 轴的右端）段内的变化。
> These plot various variables used in Doctor's analysis over time, from the start time of the profile (left end of the X-axis) to the finish time (right end of the X-axis).

所有图表的 X 轴都使用相同的刻度。将鼠标悬停在一个图表上，我们会看到所有其他图表都会显示同一时间点上的值。
> All graphs use the same X-axis. We will see that hovering over one shows the values at the same point in time on all the other graphs.

![](https://clinicjs.org/static/b750e14ce27b15143813ea66c7b85384/ace55/04-G.png)

### CPU 占用（%）

CPU 占用图显示了被分析的 Node.js 进程在任何一个时间点所占用的机器可用 CPU 容量的百分比。
> This graph shows what percentage of the machine's available CPU capacity was being used by the Node.js process being profiled at any one point in time.

![](https://clinicjs.org/static/389c9373f960566e37d4a083ebaee209/88f33/04-H.png)

如果计算机具有多个核心，CPU 使用率可能会超过 100％。CPU 占用图中的 100％ 表示单个核心容量的 100％。
> CPU Usage can exceed 100% if the machine has multiple cores. 100% on this graph means 100% of the capacity of a single core.

CPU 占用图中的峰值说明 CPU 活动频繁。如果峰值过多并且与事件循环阻塞相关（见下文），则可能会出现问题，但快速（回落）的峰值可能表示服务器能够快速处理高负载。CPU 活动过少可能意味着 Node.js 进程在等待 I/O 操作完成，比如缓慢的数据库查询或文件写入。
> Spikes in this graph indicate high CPU activity. This can be a problem if it is excessive and correlates with event loop blockage (see below), but rapid spikes can be a sign that the server is healthily processing high load quickly. Too little CPU activity can be a sign that the Node.js process is stuck waiting for an I/O operation to complete, like a slow database query or file write.

在上面的分析结果中，处理器差不多一直出于繁忙状态，看起来很健康。
> In this profile, the processor is usually fairly busy, which looks healthy.

在本练习的第 6 部分 [修复 I/O 问题](./fixing_an_IO_problem.html) 中，我们将看到一个不健康的CPU占用图示例。
> In part 6 of this walkthrough, Fixing an I/O problem, we will see an example of an unhealthy CPU Usage graph.

### 内存占用（MB）

该图表有三条数据线，表示每个时间点的内存占用量（MB），三种数据都使用相同的比例。
> This graph has three lines, showing megabytes of memory at each point in time, all on the same scale.

![](https://clinicjs.org/static/3ae52eee7e17187afb4f6a8f7c6b8493/a1f39/04-I.png)

三条线分别是：
> The three lines are:

- RSS（Resident Set Size，驻留集大小）：三个数据中最大的，表示为执行该进程而分配的所有内存。这条线与 Total Heap Allocated 那条线之间的差值表示非堆内存，例如存储 JS 代码本身，包含变量指针的“堆栈”以及布尔、整数状态等原始值，以及 Buffer 内容的内存池。
> RSS（Resident Set Size）: This will always be the highest value, representing all memory allocated as part of the execution of this process. The gap between this line and the Total Heap Allocated line represents non-heap memory, such as storing of the JS code itself, the "stack" which contains variable pointers and primitives like boolean and integer states, and a memory pool for Buffer contents.
- THA（Total Heap Allocated，分配的总堆内存）：这是为存储具有引用的项而预留的空间，如字符串、对象和函数闭包。与存储指向这些项的指针的堆栈不同，在需要堆内存之前，会为堆内存分配预先设置的内存量。
> THA (Total Heap Allocated): This is the amount of space that has been set aside for storing items that have a reference, such as strings, objects and function closures. Unlike the stack, where the reference pointers to these items are stored, a pre-set amount of memory is assigned for the heap, before it is needed.
- HU（Heap Used，使用的堆内存）：这是某个时间点实际使用的堆内存量。它表示在给定时间点上已分配但还未被垃圾回收的所有字符串、对象、闭包等的总大小。这通常是最有趣的一行，而 RSS 和 Total Heap Allocated 提供上下文。
> HU (Heap Used): This is how much heap memory is actually being used at this point. It represents the total size of all strings, objects, closures etc that have been allocated but not garbage collected at a given point in time. This is usually the most interesting line, with RSS and Total Heap Allocated providing context.

不断增加的 Heap Used 表明可能存在内存泄漏，如果对某些内容的引用仍在作用域内，意味着它不能被垃圾回收，因此导致可用内存最终干涸（耗尽）。还有一种截然相反的情况，也是一个常见问题：内存急剧下降，与 Event Loop Delay 图中的高峰数据相关，表明破坏性的垃圾回收事件正在破坏进程并阻止 Node.js 执行代码。
> A constantly increasing Heap Used line suggests a possible memory leak, where references to something are remaining in-scope, meaning it can't ever be garbage collected and therefore causing available memory to eventually run dry. The opposite is perhaps a more common problem: many sharp drops, correlating with high readings in the Event Loop Delay graph, suggests that disruptive garbage collection events are disrupting the process and blocking Node.js from executing code.

在上面的分析结果中，堆内存逐渐上升和下降，总是有大量空闲的堆内存，Resident Set Size 中也有大量非堆内存。看起来比较健康。
> In this profile, the heap goes up and down fairly gradually, there is always plenty of spare heap allocation, and there is plenty of non-heap memory in the Resident Set as well. This looks healthy.

如果我们遇到不健康的内存图，导致问题的数据线将标记为红色：
> If we encounter an unhealthy Memory graph, the specific line indicating a problem will be marked in red:

![](https://clinicjs.org/static/54f57b35734b5bc7c4caba599f008c6d/091f6/04-Q.png)

### 事件循环延迟（ms）

该图表示数据节点所在的时间点，Node.js 被同步 JavaScript 代码阻塞。
> This represents points in time in which Node.js was blocked by executing synchronous JavaScript code.

![](https://clinicjs.org/static/96a3560909bdff9ba406fa0e400777b1/4d6f1/04-J.png)

了解图表的工作原理对我们来说至关重要，因为它还提供了有关其他图表的精确信息：
> It is important that we understand how this graph works, because it also gives us information about the acuity of the other graphs:

- Y 轴表示提示框箭头指向的时间点（上图）结束的事件循环延迟的持续时间
> The Y-axis represents the duration of the event loop delay that ended at the moment in time indicated by the tooltip arrow
- 该时间点后（上图）总是跟着一条与 X 轴时间一样长的水平线。说明，在这条线的长度范围内，Node.js 都被阻塞了，因此，我们在其它图形上都没有数据。如果我们沿着包含大量事件循环延迟的图形移动光标，提示框会跳动 - 因为 Node.js 执行了一些较慢的同步代码，所以其它图表在跳动之间（的时间内）都收集不到任何数据，。
> This is then always followed by a horizontal line representing the same amount of time on the X-axis. For the length of this line, Node.js was blocked, therefore, we don't have any data on any graphs. If we run the cursor along a graph containing substantial event loop delay, the tooltip jumps - this is because no data could be collected between the jumps, for any graphs, because Node.js was stuck executing some slow synchronous code.

> 译者注：原文在这里可能缺一张图，导致下面的描述对应不上任何图

例如，在下面的屏幕截图中，我们可以看到：
> For example, in the below screenshot, we can see:

- 这是（连接处）最长的事件循环延迟，因为它是 Y 轴上的最高点。
> That this is the (joint) longest event loop delay seen in this profile, because it is the highest point on the Y-axis.
- 通过查看此提示框与前一个提示框之间的水平线，说明此延迟占用了分析结果中的一大部分时间。
> That this delay took up a noticable chunk of the duration of the profile, by looking at the horizontal line between this tooltip and the previous one.

移动光标，可以看到四个事件循环延迟占了大部分运行时间。我们还可以看到这导致其他数据看起来非常正常 - 大约在 1/4 秒之后，每次读取内存、CPU 等数据之间会有明显的跳跃，因为 Node.js 忙于执行一些缓慢的同步代码，以至于无法再次读取。
> Moving the cursor along, we can see that four event loop delays account for most of the run time. We can also see that this is causing the other data to be very course - after the first quarter of a second or so, there are noticable jumps between each reading for memory, CPU, etc, because Node.js was too busy executing some slow synchronous code to even take another reading.

这显然不健康 - Doctor 已将其标记为红色，并在 “警告栏” 中指向该图表。
> This is clearly not healthy - and Doctor has flagged it as such, colouring this graph red and pointing to it in the Alert box.

### 活跃的句柄

此图显示了当前活动的、等待输出的 I/O 句柄的数量。
> This graph shows the quantity of I/O handles presently active, awaiting an output.

![](https://clinicjs.org/static/219ffa9cf01783b28257f3e3e05545d9/1eddf/04-K.png)

当 Node.js 做异步分发时，例如使用 [libuv](https://libuv.org/) 分发像文件写入或数据库查询操作系统之类的任务，它会存储一个“句柄”。“活动句柄”指已分发但未完成的任务。
> When Node.js delegates asychronously, such as using libuv to delegate a task like a file write or a database query the operating system, it stores a "handle". An "active handle" is a delegated task that has not reported as being complete.

因此，Active Handles 图表让我们了解 Node.js 进程在任一时间点等待的异步 I/O 任务量。理想情况下，这应该遵循一个有序的模式，处理请求时上升，完成请求时下降。它还可以与其他图形组合以提供更多线索 - 例如，服务器的 CPU 峰值和新增的活动句柄是相关的，那么它也应当随着传入的请求发生变化。
> The Active Handles graph therefore gives us an idea of how much asychronous I/O activity the Node.js process is waiting on at any point in time. This should ideally follow an orderly pattern, rising and falling as requests are handled and completed. It can also offer clues when combined with the other graphs - for example, CPU spikes on a server that correlate with increased active handles should usually also correlate with incoming requests.

活动句柄图一般来说是为其他图表提供上下文依据的。很难说有一个统一的活动句柄图（Active Handles）评判标准：在不知道应用程序逻辑的情况下仅仅通过它就给出“应用不太健康”的结论显然是有失公允的。
> This graph generally provides context for the other graphs. It's hard to say generically what an Active Handles graph "should" look like: we generally can't point to an Active Handles graph and say "That looks unhealthy" without knowing the application logic.

从上图中我们可以看到有这样一个阶段：句柄活动很少，也没有其他活动。这有可能是进程刚刚准备好。然后紧接着是一段稳定的活动句柄数，这可以认为是正在处理传入的请求。基于以上信息，我们可以忽略那个活动句柄很少的早期阶段，因为它并不代表实际的服务器活动。
> Here, we have a period with very few active handles, and very little other activity, which presumably represents the process getting ready. There is then a steady period of [103] active handles, which presumably represents incoming requests being dealt with. It tells us we can probably ignore that early period with very few active handles as not representing typical server activity.

## 建议面板

单击底部的蓝色栏，将打开一个面板，上面显示了更多关于 Doctor 针对此应用程序发生了什么的结论。
> Click on the blue bar at the bottom, and it will open a panel telling us more about Doctor's conclusions about what is happening with this application.

![](https://clinicjs.org/static/b005ce77c877d64ecf2d58d17f891e3a/ace55/04-M.png)

它由两部分组成：建议摘要和详细建议文档，点击“Read more”按钮可以打开详细建议文档。
> This is in two parts: a Recommendations Summary, and a Recommendation in Detail article below a 'Read more' button.

#### 建议摘要

建议摘要提供了识别问题的简单要点概述，通常带有建议的下一步。
> This gives a simple bullet point overview of the identified problem, typically with a suggested next step.

![](https://clinicjs.org/static/474986b77bde7a332535c5a4a68cb27d/091f6/04-N.png)

介绍一下这里的几个 UI 控件：
> There are a few UI controls:

- 'x' 关闭面板
> The 'x' closes the panel
- "Browse undetected issue" 允许我们阅读 Doctor 可以识别但没有从这次分析结果中识别出的问题的描述。单击此按钮会展开一些选项卡，以显示 Doctor 没有从这次分析结果识别出的问题的描述。这在某些情况下很有用，例如：
> "Browse undetected issues" allows us to read descriptions of issues that Doctor can identify but hasn't identified for this profile. Clicking on this expands out some tabs to show descriptions of problems that Doctor has not identified for this profile. For example, We might find this useful, for example:
1. 在处理 Node.js 性能问题时，避免在修复现有问题时有产生新问题。
> While learning about Node.js performance, to avoid creating new problems while fixing an existing problem.
2. 如[警告栏](#警告栏)部分所述，Doctor 通常不会一次诊断多个问题，所以可能存在未被 Doctor 发现的已知问题。
> To understand why Doctor might not have identified a known problem. As discussed in the Alert Bar section, Doctor generally does not diagnose more than one problem at a time.

![](https://clinicjs.org/static/4bf69d3b979a9915246c8048ab673700/d582b/04-O.png)

在当前的例子中，它告诉我们“可能有一个或多个长时间运行的同步操作阻塞线程”，并建议使用 `clinic flame` 缩小范围。
> In this case, it tells us that "There may be one or more long running synchronous operations blocking the thread", and recommends using clinic flame to narrow down.

阅读摘要后，我们建议你点击 "Read more" 以更深入地了解问题。
> After reading the summary, we recommend clicking "Read more" to understand the problem in more depth.

#### 详细建议

单击 "Read more" 会展开建议面板，以显示有关已诊断出的性能问题的详细文档。这些通常有三个部分（单击左侧的内容列表会跳到特定部分）：
> Clicking 'Read more' expands the Recommendations Panel to show a detailed article on the performance problem that has been diagnosed. These usually have three sections (clicking on the contents list on the left allows us to skip to a particular section):

- Understanding the analysis：深入描述这个问题。
> Understanding the analysis describes the problem in depth.
- Next Steps：详细介绍了一些建议步骤，以缩小问题的确切原因，以便我们进行修复。通常这涉及到使用 Clinic 工具集中的另一个工具，该工具可以识别有问题的代码所在的行。
> Next Steps details some recommended steps to narrow down on the exact cause of the problem, so we can fix it. Usually this involves using another tool in the Clinic suite which can identify individual lines of problematic code.
Reference：参考文献，提供建议进一步了解问题所需阅读资料的链接。
> Reference gives links for suggested further reading and credits any sources that were quoted or used in writing this recommendation.

![](https://clinicjs.org/static/899f2e13b32fa86e73f309d8f410200a/ace55/04-P.png)

现在我们准备好开始解决问题了。
> Then we're ready to look at fixing the problem.