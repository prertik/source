+++
date = "2018-06-19"
title = "Week 07 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hello everyone.

This week there was lots of progress in builtin rebase. Lots of thanks to [Johannes Schindelin](https://github.com/dscho),
without him the patch wouldn't be in current state. Rather
than work on emulating smaller functions from `git-rebase.sh`
like I explained previously in my blog post. Me and [Dscho](https://github.com/dscho) went a lot farther and implemented a super simple rebase. You can find the code [here](https://github.com/git/git/pull/505).
If you're feeling adventurous get the code and run some rebase tasks.
Maybe try this:
```shell
./git --exec-path="$PWD" -c rebase.usebuiltin=true rebase HEAD^
```
Or if you want to use your terminal more maybe try this?

```shell
mkdir 123
git init 123
cd 123
touch a1
git add a1
git commit -m a1 a1
for i in 2 3 4; do touch a$i; git add a$i; git commit -m a$i a$i; done
cd ..
./git --exec-path=$PWD -C 123 -c rebase.usebuiltin=true rebase HEAD~
```

Did you get a message saying `warning: TODO`? Well it's because the `apply_autostash()` hasn't been implemented yet...
Modifying this and making it run will be handled in later patch.

Well if you try `echo $?` it returns 0 which is a proof of saying that our code works till that point i.e. until the `apply_autostash()`. Still lots of environment variables need to be set. There's still _a\_lot\_ to do as we are moving steadily towards a working builtin rebase. The full rebase as promised is very hard to reach as it requires lots and lots of configurations and coding. Time will tell how much I can complete in this GSoC time period and with every week the code is somewhat progressing towards something awesome. :-)

There are lots of awesome things going on here which I'll explain in the blog post where I'll breakdown every functions and total internal workings of my code of builtin rebase.

Oh and one important thing if you want to have a collaborative coding session but are waiting for some positive review to try [Visual Studio Live Share](https://code.visualstudio.com/blogs/2017/11/15/live-share).
You have mine. It works nicely. Though it's still in preview and lots of improvements are needed, it's very usable and get you more productive.

That's it for this week. Stay tuned.