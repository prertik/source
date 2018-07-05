+++
date = "2018-07-03"
title = "Week 09 Progress"
description = ""
draft = false
categories = ["git"]
tags = ["git", "shell", "c"]
+++

Hi,

This week I submitted patch [v1](https://public-inbox.org/git/20180628074655.5756-1-predatoramigo@gmail.com/) ,[v2] (https://public-inbox.org/git/20180702091509.15950-1-predatoramigo@gmail.com/) and am working on v3. On the other hand, there are other works going on towards converting the options of `git-rebase.sh`. Currently, the options supported are `--onto`, `--quiet`, `--verify`, `--continue` and support for an upstream that is actually a symmetric range i.e. `<rev1>...<rev2>` is also done.

All of these progress can be seen from [pull requests](https://github.com/git/git/pull/505).
Cleanup and addition of proper message will be done in the coming days.