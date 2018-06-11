+++
date = "2018-06-11"
title = "Week 06 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hi everyone.

Hope all of you readers had a great weekend. This week I worked on emulating `run_specific_rebase ()` which is in [git-rebase.sh](https://github.com/git/git/blob/master/git-rebase.sh#L203-L222). Firstly, I have tried to emulate specifically, this shell script:

```shell
. git-rebase--$type
git_rebase__$type${preserve_merges:+__preserve_merges}
```

The work in progress patch of this conversion can be found [here](https://github.com/git/git/pull/505). The patch isn't ready as `apply_autostash()` function need some extra working. To make it working slight modifications to [sequencer.c](https://github.com/git/git/blob/master/sequencer.c) is required which will be done in the coming days.

In other news, a [patch](https://public-inbox.org/git/20180607171344.23331-1-newren@gmail.com/) which landed on the mailing list is interesting. This patch will surely affect my current plan but this patch can help me to convert all modes into builtin code more easily than originally planned.

That's a wrap-up for this week. Stay tuned for next week.

Some Git jokes:

Why did the commit cross the rebase?
To git to the other repo.

A branch, a tag, and a reflog walk into a bar. The bartender says, "What is this, some sort of rebase?"

More jokes can be found [here](https://github.com/EugeneKay/git-jokes).
