+++
date = "2018-05-28"
title = "Week 03 and Week 04 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "c", "shell"]
+++

Hi everyone,

Sorry for the late blog. There wasn't more action on week 03 and there was some action on week 04 \*finally\*. So, I had to merge both.

Oh and a note to the readers, the previously assured Part II of Git Rebase will take some time as I plan to explain you the workings from the rebase builtin I will create. :-)

So, during these two weeks, a skeletal for rebase builtin was created. What's a skeletal? Well, it's a placeholder for the future rebase or the builtin rebase which will be the default when it gets there. There were some discussions on it, as can be seen from the initial [pull-request](https://github.com/git/git/pull/497). This is the beauty of open-source. Every awesome developers around the globe are on board to create awesome software to make life more easier. This is what most proprietary software lack. That's why they are buggy and crash while Git runs seamlessly.

So, after few discussions with the mentors, Christian Couder, Stefan Beller and Johannes Schindelin, we finally agreed to an approach to make rebase into a buitlin proposed by Johannes which is similar to the technique he applied to create difftool to a builtin. You can find the new commit following this approach [here](https://github.com/git/git/pull/497/commits/41531d0ba88cfe20e870d57ee437d0fcd7d05064).

This new technique currently falls back to the `legacy-rebase` which is a fancy term given to the rebase script file currently active. But, offers a configuration to run this builtin `rebase.useBuiltin` for future testing. There will be some extra functionalities to accommodate with this feature in the coming days, hopefully.

That's a wrap for this week. More to follow next week and once again I apologize for the lateness for this blog post.
