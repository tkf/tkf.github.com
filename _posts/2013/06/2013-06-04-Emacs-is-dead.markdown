---
layout: post
title: Emacs is Dead (translated from Japanese)
---

# {{ page.title }} #

This is an English translation of "[Emacsは死んだ](http://cx4a.org/pub/emacs-is-dead.ja.html)" by Tomohiro Matsuyama.

## Emacs is Dead

-- Tomohiro Matsuyama (2010/2/22)

First of all, I have to apologize for titling this article in such a sentimental way.  If you read this article, even part of it, you would understand how I love Emacs and think it never dies.  However, because of this, I chose the title, at least self-analysis told so.  Nevertheless, I hope you would overlook about the title.

This article is an abstract of the part of my presentation for Mitou Youth \[1].  It could stay in my brain and my HDD forever, but I made this article thinking that it could have some social value.  If you don't enjoy this then it truly is not valuable and if you enjoy this then it may have some value.  Either way, it wouldn't affect my future so much.  I'll stop introduction here and start the main issue.

### Philosophy of Emacs

This "philosophy of Emacs" is something I made up.  As far as I know, nobody, even Stallman, talks about "philosophy of Emacs".  Especially, I think Stallman is too busy for activity toward his greater goal of the "philosophy of freedom" to care about tiny problem such as "philosophy of Emacs".  Anyway, what I want to say is that philosophical value is very low as there is no back up.  You should be aware of that.

Needless to say, the biggest characteristic of Emacs is Emacs Lisp.  Emacs Lisp is one of lisp dialect, but to be honest (although it is not as bad as Vim script), it's no match for other dynamically typed language such as Ruby or Python in respect to runtime or library.  Nevertheless, because of minimization of Emacs C core and extensibility of Lisp, Emacs user can drastically change Emacs.  Emacs libraries such as anything.el which surprised Emacs user by innovative interface and undo-tree.el \[2] which fascinates me recently cannot be realized without basis of Emacs Lisp.  However, as I said, Emacs Lisp is not good as a runtime.  Here is a list of current problem

1. No multi-threading support
2. No shared-library support
3. No low layer API such as file IO

These problem prevents us from implementing advanced functionality in Emacs Lisp.  For example, as there is no multi-threading support, it is not possible to do heavy task in background without blocking editing by user.  Lack of shared library support makes it impossible to use advanced shared library from Emacs Lisp.  Due to lack of low layer API, advanced and high-speed processing cannot be implemented purely in Emacs Lisp.

A general and conventional way to workaround these problems is to use external program.  For example, Tramp, which is used for editing files in remote machine, uses complex programs such as ssh and ftp via Emacs Lisp to provide editing interface on Emacs.  Using complex programs such as svn and git, VC does version controlling as if everything is done in Emacs.  There are many other functions from standard Emacs libraries depending on external programs.  This method is excellent because Emacs Lisp can concentrate on user interface which is what it is good at while let external program does other miscellaneous complex processing.  As Emacs Lisp is not a good runtime, breaking tasks in this way makes sense.  The side effect of breaking tasks is that **excellent functions of Emacs can be defined as an external program**.  Furthermore, as messaging between Emacs Lisp and external program is "visualized" as inter-process communication, there is huge reusability.

Let's do one thought experiment.  Assume that we are implementing code completion for Ruby in Eclipse and Emacs.  Eclipse has plugin architecture based on Java and efficient processing by encapsulation of data structure is its virtue (this can be said in many other products) \[3].  Therefore, excellent ruby completion provided only in Eclipse world.  Try searching "Eclipse Ruby".  Several plugins can be found but everything is for Eclipse.  As they are tightly coupled with Eclipse, using them in platforms other than Eclipse is very hard.  Even if it is possible, it would exhaust developers by repeating merges from the main line.

So, how about Emacs?  Due to the practical reason, the problem of Emacs Lisp runtime, it is hard to implement code completion directly on Emacs.  At this point, we consider using the sacred sword of utilizing external program.  There is no limit for the way to implement the external program.  You can implement it as you like to make command line interface or network communication interface (I recommend command line interface).  Then communicating with the external program from Emacs Lisp via inter-process communication or network communication would realize the goal.

You may notice at this point.  The biggest difference between the Eclipse version and Emacs version is whether **it uses external program or not**.  The Eclispe version restricts how it is used.  On the other hand, the Emacs version can be used outside of Emacs.  With small amount of coding, it can be used from Vim or TextMate.  I feel that how it increases social value is reminiscent of GPL.  That is to say, maximizing freedom as a whole by restricting individual freedom a little bit.

In sum, Emacs Lisp has defect as a runtime and as a result it increases incentive to maximizing its potential social value.  It may be just historical understanding.  At this point in time, I would say it is more accurate to think that Emacs Lisp runtime intentionally has defect in order to maximize social values.  If I remember correctly, Stallman himself stubbornly rejected the change for shared library support \[4].  There could be the matter of maximization of social values behind the scenes.  Altogether, I think restriction on Emacs Lisp is intentionally (well, maybe not) done to maximizing values of software based on the  "Philosophy of Emacs" and I think this should be protected for eternity.

### A problem brought by new era
Performance of computer progresses significantly.  The progress of performance brings new needs and researches, and there are new waves in the Emacs world.   Among them I want to talk especially about js2-mode and Semantics.  Let's begin with js2-mode.

[js2-mode](https://code.google.com/p/js2-mode/) is developed by an eccentric developer called [Steve Yegge](http://steve-yegge.blogspot.fr/) and the remarkable point is that it has JavaScript interpreter implemented in Emacs Lisp.  He thought advanced and interactive error check, code completion and refactoring can be done by having JavaScript interpreter in Emacs.  I don't know if these functions are properly implemented, but his inference makes sense.  Existing error check for syntax error etc. is done by an extension called Flymake which is bundled in standard Emacs.  How Flymake works is simple; it starts appropriate error check program when buffer is saved or auto-saved, analyze the output of it and then highlight the line of the problem.  If an editor depends on external programs like this (although it is general in Emacs), it is often hard to implement functions mentioned above because of the inconsistency of data.  I think he thought that having JavaScript interpreter implemented in Emacs Lisp covers this point, however, it tragically **follows the way of Eclipse**.  Even if js2-mode becomes an excellent software and it receives appreciation from Emacs users, if it is not used from Vim or TextMate or other software programs, I don't think it has much social value.  If my "Philosophy of Emacs" is right, such exclusive strategy is meaningless.  There is many blockers such as inconsistency of data, as long as we use Emacs, we should concern more about maximizing social values.

Next I will talk about the problem of [Semantic](http://cedet.sourceforge.net/semantic.shtml).  I think it has severer matters than js2-mode.  This is because Semantic is going to provide a set of parsers **implemented totally in Emacs Lisp**.  Furthermore, it is merged to Emacs and **it is going to be bundled in Emacs by the next release**.  First of all, I'll explain about Semantic.  Semantic is a set of parser generators and parsers which can be written in Emacs Lisp and its ambitious goal includes giant functions such as code completion of C++ and small functions such as tag jump.  It has developed since long time ago and if you search about Emacs code completion, most of the articles are related to Semantic.  Thanks to long development, it looks like that it performs more-or-less correctly and you may be using Semantic (without noticing it).  If I were to support Semantic, I could be able to find only one point; noticing text processing capability of Emacs Lisp.  However, as this humble defence is broken through due the runtime defects mentioned above, I cannot defend it at all.

Let's get to the problem of Semantic.  As mentioned above, Semantic (including its parent package CEDET) has the following two fatal problems.

1. It is written in Emacs Lisp
2. It is goingto be bundled as of Emacs 23.2

The problem (1) is the same as that of js2-mode.  In essence, the value realized by Semantic is closed in Emacs and it does not increase social value.  It is contrariety to the "Philosophy of Emacs" and it is dangerous first step toward Eclispe.  Furthermore, the problem (2) enlarges this dangerousness because Emacs development team completely ignores the "Philosophy of Emacs" \[5].  This is exclusion against other software programs and is an act without social morality.  If it proceeds without being viewed with suspicion, Emacs platform itself starts being attracting and incentive to make software closed in Emacs world will be increased.  **The reason why Emacs platform is good is that it cooperates with OS, not because it is good by itself.**

The virtue of Unix convention of making a big thing by combining small things is inherited in Emacs.  This virtue itself is the reason why I am using Emacs.  However, this may be becoming history.  What is the meaning of the world without the virtue or philosophy, but with only benefit?  I'd be disappointed if Emacs become so.

### What should we do?
This is a very difficult matter.  Probably this is just too much of pessimism in the first place.  At any rate, what we can do is to follow the "Philosophy of Emacs" as much as possible.  We should not resort to force such as not using software.  The best way to do it would be to suggest better solutions with as much forgiveness as we could have.

### License

This article is licensed under [Creative Commons Attribution-Noncommercial-No Derivative Works 3.0](http://creativecommons.org/licenses/by-nc-nd/3.0/deed).

### Footnotes

Footnotes by translator, if not specified.

\[1] Project held by Information-technology Promotion
 Agency, Japan to help young software developers.

\[2] (Original footnote 1) an innovative undo functionality which allows you to go back to any editing version by treating undo history as a tree.

\[3] I am not sure about this translation.

\[4] Yes, I (translator) know that there is a go sign now and Stallman's intention was not to encourage external programs.  Rather, it is to prevent bypassing GPL from non-free software programs.  Also, Stallman prevents using gcc as external program which does not match to the Philosophy of Emacs Matsuyama speaks of.   But this article is rather old so I'd not blame him.

\[5] (Original footnote 2) probably there is no such thing in the first place
