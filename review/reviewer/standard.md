# 代码审查的标准



代码审查的主要目的是确保谷歌代码库的整体代码健康的到持续提升。所有代码审查的工具与过程都以此为目的而设计。

为了实现这个愿景，必须要平衡一系列的权衡点。

首先，开发人员必须能够在其任务中 _有所进展_ 。如果你从来没有响代码库中提交过一次改进，那代码库就永远不会改进。同时，如果审查者让 _任何_ 变更都非常难以进入代码库，就会抑制开发人员在未来做出改进。

另一方面，确保每个CL的质量不会随着时间降低代码库的整体质量是审查者的职责所在。
这很难察觉，因为通常，随着时间推移，代码健康会经历一系列微小的难以察觉的降低进而导致代码库降级。
特别是当团队在重要时间约束下，团队感觉到他们必须抄近路才能完成目标，这种情况就会发生。

同时，审查者对他们所审查的代码有所有权和责任。他们要确保代码库保持可持续、可维护，以及["代码审查看什么"](looking-for.md)中提到的所有事情。

因此，我们采用如下规则做为我们期望代码评审的标准：

**通常，当某个CL能明显提升目标系统整体代码健康的时候，审查者应该倾向于通过它，即便该CL并不完美。**

这是所有代码审查指南的 _唯一_ 高级原则。

当然，对此是有限制的。例如，如果一个CL添加了一个审查者不想要加入到系统的功能，那审查者当然能拒绝审核，即便代码设计很好。

A key point here is that there is no such thing as "perfect" code&mdash;there is
only _better_ code. Reviewers should not require the author to polish every tiny
piece of a CL before granting approval. Rather, the reviewer should balance out
the need to make forward progress compared to the importance of the changes they
are suggesting. Instead of seeking perfection, what a reviewer should seek is
_continuous improvement_. A CL that, as a whole, improves the maintainability,
readability, and understandability of the system shouldn't be delayed for days
or weeks because it isn't "perfect."

Reviewers should _always_ feel free to leave comments expressing that something
could be better, but if it's not very important, prefix it with something like
"Nit: " to let the author know that it's just a point of polish that they could
choose to ignore.

Note: Nothing in this document justifies checking in CLs that definitely
_worsen_ the overall code health of the system. The only time you would do that
would be in an [emergency](../emergencies.md).

## 辅导

Code review can have an important function of teaching developers something new
about a language, a framework, or general software design principles. It's
always fine to leave comments that help a developer learn something new. Sharing
knowledge is part of improving the code health of a system over time. Just keep
in mind that if your comment is purely educational, but not critical to meeting
the standards described in this document, prefix it with "Nit: " or otherwise
indicate that it's not mandatory for the author to resolve it in this CL.

## 原则 {#principles}

*   Technical facts and data overrule opinions and personal preferences.

*   On matters of style, the [style guide](http://google.github.io/styleguide/)
    is the absolute authority. Any purely style point (whitespace, etc.) that is
    not in the style guide is a matter of personal preference. The style should
    be consistent with what is there. If there is no previous style, accept the
    author's.

*   **Aspects of software design are almost never a pure style issue or just a
    personal preference.** They are based on underlying principles and should be
    weighed on those principles, not simply by personal opinion. Sometimes there
    are a few valid options. If the author can demonstrate (either through data
    or based on solid engineering principles) that several approaches are
    equally valid, then the reviewer should accept the preference of the author.
    Otherwise the choice is dictated by standard principles of software design.

*   If no other rule applies, then the reviewer may ask the author to be
    consistent with what is in the current codebase, as long as that doesn't
    worsen the overall code health of the system.

## 解决冲突 {#conflicts}

In any conflict on a code review, the first step should always be for the
developer and reviewer to try to come to consensus, based on the contents of
this document and the other documents in [The CL Author's Guide](../developer/)
and this [Reviewer Guide](index.md).

When coming to consensus becomes especially difficult, it can help to have a
face-to-face meeting or a video conference between the reviewer and the author, instead of
just trying to resolve the conflict through code review comments. (If you do
this, though, make sure to record the results of the discussion as a comment on
the CL, for future readers.)

If that doesn't resolve the situation, the most common way to resolve it would
be to escalate. Often the
escalation path is to a broader team discussion, having a Technical Lead weigh in, asking
for a decision from a maintainer of the code, or asking an Eng Manager to help
out. **Don't let a CL sit around because the author and the reviewer can't come
to an agreement.**

Next: [What to look for in a code review](looking-for.md)
