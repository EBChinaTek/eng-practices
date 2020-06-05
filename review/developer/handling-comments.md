# 如何处理审查者的评论



当你将某个CL发去审查，你的审查者可能会在你的CL上回复若干评论。你要了解有关处理审查者评论的一些有用的事情。

## 不要认为评论是针对你个人的 {#personal}

审查的目标是维护我们的代码库和我们的产品的质量。
当某个审查者对你的代码给出了一条批评，应当认为他们在试图帮助你、帮助代码库、帮助公司（Google），而不是对你或你的能力进行攻击。

有时，审查者会感到心烦挫败，并且他们会在其评论中表达这种情绪。这对于审查者来说不是好的实践，
但是作为一名开发人员，你应当对此做好心理准备。问问你自己：“这个审查者试图想我传达哪些建设性的事情？”，然后就当作这是他们真正说的。

**Never respond in anger to code review comments.** That is a serious breach of
professional etiquette that will live forever in the code review tool. If you
are too angry or annoyed to respond kindly, then walk away from your computer
for a while, or work on something else until you feel calm enough to reply
politely.

In general, if a reviewer isn't providing feedback in a way that's constructive
and polite, explain this to them in person. If you can't talk to them in person
or on a video call, then send them a private email. Explain to them in a kind
way what you don't like and what you'd like them to do differently. If they also
respond in a non-constructive way to this private discussion, or it doesn't have
the intended effect, then
escalate to your manager as
appropriate.

## Fix the Code {#code}

If a reviewer says that they don't understand something in your code, your first
response should be to clarify the code itself. If the code can't be clarified,
add a code comment that explains why the code is there. If a comment seems
pointless, only then should your response be an explanation in the code review
tool.

If a reviewer didn't understand some piece of your code, it's likely other
future readers of the code won't understand either. Writing a response in the
code review tool doesn't help future code readers, but clarifying your code or
adding code comments does help them.

## Think for Yourself {#think}

Writing a CL can take a lot of work. It's often really satisfying to finally
send one out for review, feel like it's done, and be pretty sure that no further
work is needed. So when a reviewer comes back with comments on things that could
be improved, it's easy to reflexively think the comments are wrong, the reviewer
is blocking you unnecessarily, or they should just let you submit the CL.
However, **no matter how certain you are** at this point, take a moment to step
back and consider if the reviewer is providing valuable feedback that will help
the codebase and Google. Your first question to yourself should always be, "Is
the reviewer correct?"

If you can't answer that question, it's likely the reviewer needs to clarify
their comments.

If you *have* considered it and you still think you're right, feel free to
respond with an explanation of why your method of doing things is better for the
codebase, users, and/or Google. Often, reviewers are actually providing
*suggestions* and they want you to think for yourself about what's best. You
might actually know something about the users, codebase, or CL that the reviewer
doesn't know. So fill them in; give them more context. Usually you can come to
some consensus between yourself and the reviewer based on technical facts.

## Resolving Conflicts {#conflicts}

Your first step in resolving conflicts should always be to try to come to
consensus with your reviewer. If you can't achieve consensus, see
[The Standard of Code Review](../reviewer/standard.md), which gives principles
to follow in such a situation.
