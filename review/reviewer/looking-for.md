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

CL比原本应该的更复杂么？对此，需要在CL的各个层面进行检查：个别代码行过于复杂？函数过于复杂？类过于复杂？“过于复杂”通常的意思是**“不能快速被代码的读者所理解”**。还意味着**“开发者在试图调用或修改这一代码时，更可能引入bug”**。

有一种特殊类型的复杂度叫做**过度工程**，开发者把代码写得非常通用但超出需要，或者添加了系统当前不需要的功能。审查者应当特别警惕过度工程。
鼓励开发者解决他们所知道的*现在*需要解决的问题，而不是解决开发者猜测未来可能需要解决的问题。
未来的问题应当在真正到来并且你能看到其实际样子和现实中需求时才解决。

## 测试

Ask for unit, integration, or end-to-end
tests as appropriate for the change. In general, tests should be added in the
same CL as the production code unless the CL is handling an
[emergency](../emergencies.md).

Make sure that the tests in the CL are correct, sensible, and useful. Tests do
not test themselves, and we rarely write tests for our tests—a human must ensure
that tests are valid.

Will the tests actually fail when the code is broken? If the code changes
beneath them, will they start producing false positives? Does each test make
simple and useful assertions? Are the tests separated appropriately between
different test methods?

Remember that tests are also code that has to be maintained. Don't accept
complexity in tests just because they aren't part of the main binary.

## 命名

Did the developer pick good names for everything? A good name is long enough to
fully communicate what the item is or does, without being so long that it
becomes hard to read.

## 注释

Did the developer write clear comments in understandable English? Are all of the
comments actually necessary? Usually comments are useful when they **explain
why** some code exists, and should not be explaining *what* some code is doing.
If the code isn't clear enough to explain itself, then the code should be made
simpler. There are some exceptions (regular expressions and complex algorithms
often benefit greatly from comments that explain what they're doing, for
example) but mostly comments are for information that the code itself can't
possibly contain, like the reasoning behind a decision.

It can also be helpful to look at comments that were there before this CL. Maybe
there is a TODO that can be removed now, a comment advising against this change
being made, etc.

Note that comments are different from *documentation* of classes, modules, or
functions, which should instead express the purpose of a piece of code, how it
should be used, and how it behaves when used.

## 风格

We have [style guides](http://google.github.io/styleguide/) at Google for all
of our major languages, and even for most of the minor languages. Make sure the
CL follows the appropriate style guides.

If you want to improve some style point that isn't in the style guide, prefix
your comment with "Nit:" to let the developer know that it's a nitpick that you
think would improve the code but isn't mandatory. Don't block CLs from being
submitted based only on personal style preferences.

The author of the CL should not include major style changes combined with other
changes. It makes it hard to see what is being changed in the CL, makes merges
and rollbacks more complex, and causes other problems. For example, if the
author wants to reformat the whole file, have them send you just the
reformatting as one CL, and then send another CL with their functional changes
after that.

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
