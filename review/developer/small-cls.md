# 小CL



## 为什么写小CL？ {#why}

小而简单的CL可以：

-   **审查速度更快。** 对审查者来说，找到5分钟完成一个小CL的审查并审查多个CL要比抽出30分钟审查一个大的CL要简单的多。
-   **审查更彻底。** 采用大的变更，需要反复回忆大量的变更细节，才能想起当时的思路，而有些重要的思考就会遗忘或丢失，导致审查者和变更作者受挫。
-   **更不容易引入bug。** 由于你做的变更更少，对你和你的审查者能更有效地讨论该CL的影响，并发现是否引入了bug。
-   **如果CL被拒绝，浪费的工作更少。** 如果你写了个巨大的CL，然后你的审查者说整个方向都是错的，那你就浪费了大量工作。
-   **更早合并。** 做大的CL要花很长时间，所以你合并时会有很多冲突，并且你将不得不频繁合并。
-   **更容易把设计做好。** 小变更的设计打磨和代码健康保养比改进大变更的所有细节要容易得多。
-   **审查中的阻塞更少。** 分多个独立的小块发布整个变更能够让你在等待CL审查的同时持续编码。
-   **回退更容易。** 大CL更可能碰触到在CL最初发布和CL回退之间被别人更新的文件，这会让回退变得复杂（期间的CL可能也需要回退）。

注意，**审查者有权仅仅由于过大直接拒绝你的CL**。通常他们会感谢你的贡献，但要求你将它拆分成一系列更小的变更。
在你已经写完一个变更之后再拆分，需要做许多工作，或者需要大量时间争论为什么审查者应该接收你的大变更。如果已开始就写小的CL，就简单得多。

## 什么是小？ {#what_is_small}

通常，一个CL的正确大小是指**一个独立变更**。其意思是：

-   CL做最小的改变，每个**只解决一件事**。这通常只是一个功能的一部分，而不是一次提交整个功能。一般来说，将CL写得过度小比写得过大要好。
    你需要和你的审查者一起找到可接受的大小。
-   代码审查者理解CL所需要的所有事情（除了未来的开发）都要包含在CL中，比如CL的描述、已有的代码、或者某个已经审查的CL。
-   当CL被签入之后，系统能够继续对其用户和开发人员正常运行。
-   CL不能小到难以理解其改变的意图。如果你添加了一个新的API，你应该将API的使用放到同一个CL中，以便审查者能够更好地理解API会被如何使用。
    这也有助于避免签入未使用的API。

没有硬性且快速的规则规定多大算“太大”。100行代码通常是一个CL的合理大小，1000行通常就太大了，但这取决于你的审查者的判断。
一次变更中所改到的文件数量也影响其“大小”。对一个文件的200行变更可能是OK的，但跨50个文件修改200行就太大了。

要时刻提醒自己：你从一开始写代码的时候就已经深度参与进来了，但审查者通常对上下文一无所知。
对你来说看起来是大小可以接受的CL对你的审查者来说可能就崩溃了。
如有疑问，请将CL写得比你认为必须要写的大小更小。
审查者很少会抱怨CL太小。

## 什么时候大CL也是OK的？ {#large_okay}

有些情形下，大的变更不会被认为是不好的：

-   你通常会将删除一整个文件算作一行变更，因为它不会花审查人员很多时间做审查。
-   有时，大CL是由你完全信任的自动化重构工具生成的，审查者的工作只是象征性地检查，并说它们的确想要改变。
    这些CL会更大一些，但上面的一些警告（比如合并和测试）依然适用。
    and say that they really do want the change. These CLs can be larger,

### 按文件拆分 {#splitting-files}

另一种分解CL的方式是对需要不同审查者审查的文件进行分组，但需要形成各自独立的变更。

例如：你发布了一个对协议缓冲区进行修改的CL和另一个对使用该协议的代码进行修改的CL。你必须先发布协议CL，再发布代码CL，但它们能够同时进行审查。
如果你这样做，你可能要同时通知两组审查者你写了另一个CL，以便他们了解你的变更上下文。


再举个例子：你发布了一个代码变更的CL和另一个配置或实验使用该代码的CL；要想在必要的时候回退也很简单，因为配置/实验文件有时能比代码变更更快地推到生产环境。

## 分离重构 {#refactoring}

通常，最好再独立于功能变更或bug修复的CL中进行重构。例如，移动重命名某个类应当和修复这个类的bug在不同的CL。
当CL是拆开的，审查者能更容易地理解每个CL所带来的改变。

虽然小的清理（比如修改本地变量名）会包含在某个功能变更或bug修复CL中，但开发者和审查者要判断将重构包含在当前CL中是否会导致规模大到审查更加困难。

## 保持相关的测试代码同在CL中 {#test_code}

避免将测试代码拆分到一个单独的CL中。用于验证你的代码修改的测试应该连同代码放在同一个CL中，即便这会增加代码变更行数。

然而，<i>独立的</i>测试修改可以先放到单独的CL中，类似于[refactorings guidelines](#refactoring)。这包括：

*   用新的测试验证之前已经存在的、提交过的代码。
*   重构测试代码（例如，引入帮助器函数）。
*   引入更大规模测试框架代码（例如，集成测试）。

## 不要破坏构建 {#break}

If you have several CLs that depend on each other, you need to find a way to
make sure the whole system keeps working after each CL is submitted. Otherwise
you might break the build for all your fellow developers for a few minutes
between your CL submissions (or even longer if something goes wrong unexpectedly
with your later CL submissions).

## 无法使其足够小 {#cant}

Sometimes you will encounter situations where it seems like your CL *has* to be
large. This is very rarely true. Authors who practice writing small CLs can
almost always find a way to decompose functionality into a series of small
changes.

Before writing a large CL, consider whether preceding it with a refactoring-only
CL could pave the way for a cleaner implementation. Talk to your teammates and
see if anybody has thoughts on how to implement the functionality in small CLs
instead.

If all of these options fail (which should be extremely rare) then get consent
from your reviewers in advance to review a large CL, so they are warned about
what is coming. In this situation, expect to be going through the review process
for a long time, be vigilant about not introducing bugs, and be extra diligent
about writing tests.

接下来：[如何处理审查者评论](handling-comments.md)
