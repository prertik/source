+++
date = "2018-06-25"
title = "Week 08 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hello Everyone.

This is week was awesome. The [patch](https://github.com/git/git/pull/505) I'm working on hit a milestone. Now, it can perform a real rebase. Of course, the environment variables are kept default and this might introduce bugs but for development purposes; to see real development, this is one amazing way. There was also some work on refactoring out codes from `sequencer.c` to `checkout.c` so that the codes can be used from our builtin rebase. So, after refactoring, a bug got introduced due to improper locking. This issue was fixed in [this commit] (https://github.com/git/git/pull/505/commits/b58689f0cce89525e5a985122213c077b8926862). This patch series might debut in the mailing list in the coming days.

You can test the patch using the same technique mentioned in previous blog post.

Stay tuned for more.