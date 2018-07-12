+++
date = "2018-07-10"
title = "Week 10 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hello from Kathmandu where it has been raining a lot!

This week also there were some development on `rebase` land!

Continuing form the past week blog post, I submitted newer iterations of
[patch](https://public-inbox.org/git/20180708180104.17921-1-predatoramigo@gmail.com/). Well, after some excellent review, the patch seemed to need some important changes, it was merged into `pu` but a re-roll is expected to fix some issues in [it](https://public-inbox.org/git/CAPig+cQopjftfSoPHPZQAzECTAUUwZ-pXYMeWEV=VJBFm63t9g@mail.gmail.com/). New iteration of this patch series will take some time to arrive.

There were lots of works done this week to add options to builtin rebase. The tricky script to convert was [this](https://github.com/prertik/git/blob/ae91dbb2556d7ea9f4e205fbf7847fa8579c64b4/TODO-rebase.sh#L661-L697) but it was [done](https://github.com/git/git/pull/505/commits/2837d71846519366c9e7629d827190075e443165). Then `git rebase <upstream> <switch-to>` was [implemented](https://github.com/git/git/pull/505/commits/6c0bc8c08064dc37fab6baf57856e9b66eff04a7). Another cool option `--force-rebase` was implemented along with `--signoff`.
The options conversion will land into the mailing list in the upcoming weeks along with the new iteration of aforementioned patch.

So, the plan for the upcoming week is to convert all the options (except the ones requiring other rebase types) and then work on other types of rebase.

That's all for this week!
