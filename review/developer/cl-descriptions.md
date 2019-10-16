# Writing good CL descriptions 

# 如何写好Change List的描述



A CL description is a public record of **what** change is being made and **why**
it was made. It will become a permanent part of our version control history, and
will possibly be read by hundreds of people other than your reviewers over the
years.

Change List描述了代码变更的**内容**和**原因**。它将成为代码版本控制历史的一部分，在几年内会被除了代码评审人员之外的几百人阅读。

Future developers will search for your CL based on its description. Someone in
the future might be looking for your change because of a faint memory of its
relevance but without the specifics handy. If all the important information is
in the code and not the description, it's going to be a lot harder for them to
locate your CL.

此后，开发者会通过描述去搜索Change List。日后或许有人查找Change List的时候只是记得大概但是记不清楚细节。如果所有的重要的信息都是在代码里面，但是没有在Change List的描述中，这样就很难找到想要的Change List。

## First Line {#firstline}

## 首行 

*   Short summary of what is being done.
*   简短的总结一下做了什么。
*   Complete sentence, written as though it was an order.
*   像命令一样，写一个完整句子。
*   Follow by empty line.
*   紧跟着空一行。

The **first line** of a CL description should be a short summary of
*specifically* **what** *is being done by the CL*, followed by a blank line.
This is what most future code searchers will see when they are browsing the
version control history of a piece of code, so this first line should be
informative enough that they don't have to read your CL or its whole description
just to get a general idea of what your CL actually *did*.

Change List的**首行**应该简短的总结*Change List具体***做了什么**，然后空一行。这是以后绝大多数搜代码的人在代码版本控制的历史中能够看到的，所以说第一行必须提供足够的信息可以让其他人不需要看代码或者完整的描述就可以知晓Change List到底*做了什么*。

By tradition, the first line of a CL description is a complete sentence, written
as though it were an order (an imperative sentence). For example, say
\"**Delete** the FizzBuzz RPC and **replace** it with the new system." instead
of \"**Deleting** the FizzBuzz RPC and **replacing** it with the new system."
You don't have to write the rest of the description as an imperative sentence,
though.

传统意义上，Change List描述的首行是一个完整的句子，就像是一句命令(祈使句)。例如，"**删除**the FizzBuzz RPC同时用新系统**替换**它。"，而不是"**正在删除**the FizzBuzz RPC并正在用新系统**替换**它。"。
第一行描述的剩余部分可以不用写成祈使句。

## Body is Informative {#informative}

## 完整的正文

The rest of the description should be informative. It might include a brief
description of the problem that's being solved, and why this is the best
approach. If there are any shortcomings to the approach, they should be
mentioned. If relevant, include background information such as bug numbers,
benchmark results, and links to design documents.

剩下的描述应当是完整的。它或许了包含对所解决问题的简短描述，或者说明了改进后的做法是最好的。它还应当提及改进后的做法是否可能有缺点。如果足够相关，还应当包含背景信息，例如bug的编号，性能测试（benchmark）的结果，设计文档的链接等。

Even small CLs deserve a little attention to detail. Put the CL in context.

即使很小的Change List也值得关注其细节。将Change List放到上下文中。

## Bad CL Descriptions {#bad}

## 错误的Change List描述

"Fix bug" is an inadequate CL description. What bug? What did you do to fix it?
Other similarly bad descriptions include:

"修正bug"这样的描述是不足的。bug是什么？你对bug的修复内容是什么？下面列举了其它这类错误的描述：

-   "Fix build."
-   "Add patch."
-   "Moving code from A to B."
-   "Phase 1."
-   "Add convenience functions."
-   "kill weird URLs."

-   “修复编译”。
-   “添加补丁（patch）”。
-   “将代码从A移动到B。”
-   “阶段1。”
-   “添加便利功能。”
-   “杀死怪异的网址。”

Some of those are real CL descriptions. Their authors may believe they are
providing useful information, but they are not serving the purpose of a CL
description.

其中一些是真正的Change List描述。它的作者可能认提供了有用的信息，但它们是不符合Change List目的的描述。

## Good CL Descriptions {#good}

## 优秀的Change List描述

Here are some examples of good descriptions.

### Functionality change

### 功能的变更

Example:
> rpc: remove size limit on RPC server message freelist.
>
> Servers like FizzBuzz have very large messages and would benefit from reuse.
> Make the freelist larger, and add a goroutine that frees the freelist entries
> slowly over time, so that idle servers eventually release all freelist
> entries.

> rpc：删除RPC服务器消息空闲列表的大小限制。
>
> 像FizzBuzz这样的服务器具有非常大的消息，可以从重用中受益。
> 增大freelist，并添加一个释放freelist条目的goroutine
> 随着时间的流逝，空闲的服务器最终释放所有freelist条目

The first few words describe what the CL actually does. The rest of the
description talks about the problem being solved, why this is a good solution,
and a bit more information about the specific implementation.

前几个词描述了CL的到底干了什么。剩下的描述谈论了要解决的问题，这为什么一个好的解决方案，以及有关具体实现的更多信息。

### Refactoring 重构

Example:
> Construct a Task with a TimeKeeper to use its TimeStr and Now methods.
>
> Add a Now method to Task, so the borglet() getter method can be removed (which
> was only used by OOMCandidate to call borglet's Now method). This replaces the
> methods on Borglet that delegate to a TimeKeeper.
>
> Allowing Tasks to supply Now is a step toward eliminating the dependency on
> Borglet. Eventually, collaborators that depend on getting Now from the Task
> should be changed to use a TimeKeeper directly, but this has been an
> accommodation to refactoring in small steps.
>
> Continuing the long-range goal of refactoring the Borglet Hierarchy.

> 使用TimeKeeper构造了一个任务去使用其TimeStr方法和Now方法。
>
> 向Task中添加Now方法，以便可以删除borglet（）的getter方法（仅由OOMCandidate去调用borglet的Now方法）。这取代了Borglet上委托给TimeKeeper的方法。
>
> 允许任务提供Now方法迈出了消除对Borglet（）依赖的一步。最终，要从从任务中获取Now方法的的协作者应该改为直接使TimeKeeper，但这已经是可以一步一步地进行重构。
>
> 接着重构的长期目标是重构Borglet层次结构。

The first line describes what the CL does and how this is a change from the
past. The rest of the description talks about the specific implementation, the
context of the CL, that the solution isn't ideal, and possible future direction.
It also explains *why* this change is being made.

第一行描述了CL做了什么以及相对之前的变化。剩余的CL描述了具体的实现，CL的上下文，改动是不是完美的，以及未来重构的方向。
它也解释了**为什么**要重构。

### Small CL that needs some context

### 小的代码变更需要上下文

Example:
> Create a Python3 build rule for status.py.
>
> This allows consumers who are already using this as in Python3 to depend on a
> rule that is next to the original status build rule instead of somewhere in
> their own tree. It encourages new consumers to use Python3 if they can,
> instead of Python2, and significantly simplifies some automated build file
> refactoring tools being worked on currently.

> 对status.py新建了Python3 的编译规则
>
> 这将让那些在Python3中运行status.py的人就近找到编译规则，就不用在自己的tree中编译。
> 这将鼓励使用Python3,并且极大的简化了目前用作重构的自动化构建文件。

The first sentence describes what's actually being done. The rest of the
description explains *why* the change is being made and gives the reviewer a lot
of context.

第一行描述了CL做了什么以及相对之前的变化。剩余的CL描述了具体的实现，CL的上下文，改动是不是完美的，以及未来重构的方向。
它也解释了**为什么**要重构。

## Review the description before submitting the CL

## 提交Change List前检查其描述

CLs can undergo significant change during review. It can be worthwhile to review
a CL description before submitting the CL, to ensure that the description still
reflects what the CL does.

检查试Change list可能会改动很大。提交前检查Change List的描述是非常值得，这样可以保证描述如实的反映了这次变更。

Next: [Small CLs](small-cls.md)
