# 在代码审查中看什么



注意：在考虑这些点之前，请始终确保考虑[代码审查标准](standard.md)。

## 设计

覆盖一次审查的最重要的事情就是CL的整体设计。
CL中不同代码片段之间的交互明智吗？这个变动属于你的代码库还是依赖库？
它和系统其他部分集成得好吗？
现在是添加这个功能的好时机吗？

## 功能

这个CL做了该开发人员想要做的事情了么？开发人员想要给该代码的用户带来什么收益？
“用户”通常既有最终用户（当他们受到变更影响）和开发人员（他们未来将必须“使用”这个代码）。

大多时候，我们期望开发人员在提交代码审查之前对CL进行足够好的测试来确保这些CL能够正确运行。
然而，做为审查者，你应该一直思考边界情况，查找并发问题，尽量像用户一样思考，并确保在阅读代码之前没有可见的bug。

当CL对用户有影响，比如**界面改变**，那么检查CL的行为对审查者来说就至关重要了，你 *能够* 要求对CL进行验证的时间。
当你在读代码时，很难理解有些变更对用户会产生怎样的影响。对于这样的变更，如果自己尝试测试这个CL太不方便，你可以让开发人员给你一个功能Demo。

代码审查中考虑功能的另一个重要情况是如果CL中执行了某种在理论上可能产生思索或竞态条件**并行编程**。
这类问题很难通过运行代码被探测到，通常要求人们（开发者和审查者）一起对其进行谨慎的思考，确定问题不会被引入。
（要注意的是，这也是为什么不要使用并发模型的一大原因，竞态条件和思索都可能让其非常复杂，难以进行代码审查或代码难以理解）。

## 复杂性

CL比原本应该的更复杂么？对此，需要在CL的各个层面进行检查：个别代码行过于复杂？函数过于复杂？类过于复杂？
“过于复杂”通常的意思是**“不能快速被代码的读者所理解”**。还意味着**“开发者在试图调用或修改这一代码时，更可能引入bug”**。

有一种特殊类型的复杂度叫做**过度工程**，开发者把代码写得非常通用但超出需要，或者添加了系统当前不需要的功能。审查者应当特别警惕过度工程。
鼓励开发者解决他们所知道的*现在*需要解决的问题，而不是解决开发者猜测未来可能需要解决的问题。
未来的问题应当在真正到来并且你能看到其实际样子和现实中需求时才解决。

## 测试

要求有适合该变更的单元测试、集成测试或端到端测试。
通常，测试应当添加到产品代码一同添加到CL中，除非这个CL是在处理[紧急情况](../emergencies.md)。

确保CL中的测试是正确的、明智的、有用的。测试不会测试资深，我们很少为我们的测试写测试——每个人都必须确保测试有效。

当代码出问题的时候，测试真的会失败吗？
如果代码变更掩盖了测试，测试会产生假阳性吗？
每个测试都会做出简单且有用的断言吗？
这些测试按不同的测试方法进行正确划分了吗？

记住，测试也是代码，必须得到维护。不要仅仅因为它们不是主程序的一部分就接受测试复杂性。

## 命名

开发者为每件事都挑选了一个好名字吗？一个好的名字要足够长，传达这个东西是什么或做什么，但有不会过长而难以阅读。

## 注释

开发者用可理解的英文编写了清晰的注释吗？
所有的注释都真的是必须的吗？
通常，当注释**解释为什么**某些代码会存在、并且不是在解释某些代码在*做什么*时，这些注释就是有用的。
如果代码因为不够清晰而解释自身，那就应该把代码写得更简单些。
尽管有一些例外，比如正则表达式和复杂算法通常用注释来解释做了什么会很有意义，
但是通常注释时用来描述代码本身不可能包含的信息的，比如一个决定背后的原因。

看看这个CL之前存在的注释也会很有帮助。
也许有某个TODO现在需要删掉，也许注释的建议与这个变更截然相反，等。

注意，注释不同于类、模块或者函数的*文档*，文档应该用来表达一段代码的目的、应当如何使用以及使用时代码应有怎样的行为。

## 风格

在谷歌，我们对所有主要语言都有[风格指南](http://google.github.io/styleguide/)，甚至许多小众语言也有。
确保CL遵循正确的风格指南。

如果你想要改进某些风格指南中没有的风格点，需要在你的评论前添加前缀“Nit：”，让开发者知道这是一个你认为要改进的代码，但不是强制的。
不要仅仅根据个人的风格喜好而让CL提交阻塞。

CL的作者不应当将重大风格变更与其他变更合在一起提交。
这会导致难以看到CL中哪些进行了改变，让合并和回退更复杂，并导致其他问题。
比如：如果作者想要重新格式化整个文件，就应当提交给你一份只有重新格式化的CL，在这之后再发一个所做的功能变更的CL。

## 一致性

What if the existing code is inconsistent with the style guide? Per our
[code review principles](standard.md#principles), the style guide is the
absolute authority: if something is required by the style guide, the CL should
follow the guidelines.

Otherwise, if the style guideline is more a recommendation and not a
requirement, then it's a judgment call whether the new code should be consistent
with the style guide or the surrounding code. Bias towards following the style
guide unless the local inconsistency would be too confusing.

If no other rule applies, the author should maintain consistency with the
existing code.

Either way, encourage the author to file a bug and add a TODO for cleaning up
existing code.

## 文档

If a CL changes how users build, test, interact with, or release code, check to
see that it also updates associated documentation, including
READMEs, g3doc pages, and any generated
reference docs. If the CL deletes or deprecates code, consider whether the
documentation should also be deleted.
If documentation is
missing, ask for it.

## 每一行 {#every_line}

Look at *every* line of code that you have been assigned to review. Some things
like data files, generated code, or large data structures you can scan over
sometimes, but don't scan over a human-written class, function, or block of code
and assume that what's inside of it is okay. Obviously some code deserves more
careful scrutiny than other code&mdash;that's a judgment call that you have to
make&mdash;but you should at least be sure that you *understand* what all the
code is doing.

If it's too hard for you to read the code and this is slowing down the review,
then you should let the developer know that
and wait for them to clarify it before you try to review it. At Google, we hire
great software engineers, and you are one of them. If you can't understand the
code, it's very likely that other developers won't either. So you're also
helping future developers understand this code, when you ask the developer to
clarify it.

If you understand the code but you don't feel qualified to do some part of the
review, make sure there is a reviewer on the CL who is qualified, particularly
for complex issues such as security, concurrency, accessibility,
internationalization, etc.

## 上下文

It is often helpful to look at the CL in a broad context. Usually the code
review tool will only show you a few lines of code around the parts that are
being changed. Sometimes you have to look at the whole file to be sure that the
change actually makes sense. For example, you might see only four new lines
being added, but when you look at the whole file, you see those four lines are
in a 50-line method that now really needs to be broken up into smaller methods.

It's also useful to think about the CL in the context of the system as a whole.
Is this CL improving the code health of the system or is it making the whole
system more complex, less tested, etc.? **Don't accept CLs that degrade the code
health of the system.** Most systems become complex through many small changes
that add up, so it's important to prevent even small complexities in new
changes.

## 好的事情 {#good_things}

If you see something nice in the CL, tell the developer, especially when they
addressed one of your comments in a great way. Code reviews often just focus on
mistakes, but they should offer encouragement and appreciation for good
practices, as well. It’s sometimes even more valuable, in terms of mentoring, to
tell a developer what they did right than to tell them what they did wrong.

## 概要

在开展代码审查时，你应该确保：

-   代码设计良好。
-   功能对代码的用户有益。
-   任何UI的改变都是明智的且看起来不错。
-   并行编程是安全的。
-   代码复杂度不高于必需。
-   开发人员没有实现他们在未来 *可能* 需要但不知道他们现在需要实现的事情。
-   代码具备合适的单元测试。
-   测试设计良好。
-   开发人员对所有事情使用了清晰的名字。
-   注释清晰、有用，多为解释 *为什么* 而非 *什么*。
-   代码编写了合适的文档（通常用g3doc）。
-   代码负荷我们的风格指南。

确保审查要求审查代码的 **每一行**，看看**上下文**，确保你在**改进代码健康**并且赞赏开发人员所做的 **好的事情**。

接下来：[导航评审中的CL](navigate.md)
