+++
date = "2018-08-04"
title = "Week 13 - the last week of GSoC Work period!"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hello everyone, Sorry for the late post as you know I have been very busy lately giving finishing touches to the `builtin/rebase.c`. Yes, you read it right. We have a complete `rebase.c`, it has some nits still, as some tests particularly `t5520` and `t7406` are not passing but the good news is, the tests `t34*` are passing. Yipee!

Regarding the status on the mailing list, after some regression tests suggested by [@dscho](https://github.com/dscho), we found out that certain key changes need to be done to [v5](https://public-inbox.org/git/CAOZc8M_FJmMCEB1MkJrBRpLiFjy8OTEg_MxoNQTh5-brHxR-=A@mail.gmail.com/), so I requested the Git maintainer to hold it for some time i.e. not merge from `pu` to `next`. The good news is I'm getting `v6` ready as of now.

So, I'll be submitting all the commits of my branch to mailing list as soon as it is ready. As, I have stated in my previous blogs, there are lots of commits with specific changes as @dscho instructed me to do, as it will be more make easier for review process and won't get stuck in review cycle for long.

I'll conclude my GSoC blog series with this status of my work.

And a quick reminder, a detailed part II of [Git Inside® Part I: Don't Fear the Rebase](https://prertik.github.io/post/git-inside/) will be posted (after the complete GSoC period) for more detail read of my conversion from `git-rebase.sh` to `builtin/rebase.c`. I'll closely describe the underlyings of Git or Git Internals in that blog post. So, do keep on checking out this blog.