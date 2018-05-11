+++
date = "2018-05-10"
title = "Git Inside® Part I: Don't Fear the Rebase"
description = "This is a two part blog. This is Part I of II."
categories = ["git"]
tags = ["git", "c", "shell", "rebase", "merge"]
images = [
"https://prertik.github.io/static/tenor.gif",
"https://prertik.github.io/static/applause.gif"
]
author = "Pratik Karki"
+++
Ok, you've read my previous Git blog. There were nice feedback in the [mailing list](https://public-inbox.org/git/CAH5451mJO3Bgg9DST57fqcGGU-SPrcSECjXi0qOqeKsW0uaRzg@mail.gmail.com/) and in emails and IRC. This week I did a close study on `git-rebase.` 
There are \_a\_lot\_of\_posts\_ about [`git-rebase`](https://git-scm.com/docs/git-rebase) which talk of benefits of rebase and some are negative towards rebase.
By convention, it is understood that `rebase` is harder to understand for beginners that's why most will prefer merging. Even Github has merging of pull requests by default and to do rebase and merge you'll have to select it from the drop-down menu in the pull-requests section.

Don't be afraid of rebase and after reading this, think of all the things you could have done if you had known rebase. 

After understanding the power of rebase and use it to make your branches more cleaner and linear. You will be the cool one.

{{< figure src="/static/tenor.gif" >}}

Ok, let's de-structure `git-rebase`. The general idea of rebase is that it is used to reapply commits on top of another base tip. 
Suppose, you're working in a tech company which has many Engineers and your choice of DVCS is Git. Now, to introduce new features, the developers will work in their own feature branch which has to be merged someday or rebased into the master.

Suppose, the structure of the branch is:
```ascii
M0---M1---M2 master
  \
   \
    \
     B1---B2 feature
```
Now, you're base branch is M0 and the developers based their feature branch B1 on it. But, since there is always development going on in a tech company. The master branch gradually develops to M1 and M2. While the feature branch also gradually develops to B2. The developers will find certain aspects have changed in the code base and they need to keep up with the master branch.
Hence they will do,
```git
git rebase master
git rebase master feature
```
The structure of the branch now changes to:
```ascii
M0---M1---M2 master
           \
            \
             \
              B1'---B2' feature
```
Now, life goes on, hence, more code is written and branches keep on growing. Assume the structure after some time:
```ascii
M0---M1---M2---M3---B3'---M4---M5---M6---M7 master
           \
            \
             \
              B1'---B2'---B3---B4---B5 feature
```
Now, again the developers will rebase. But, this time let's assume feature B3 was crucial for master and was already sent as a patch via mail and it was merged into master. While rebasing it will be automatically skipped.
The new structure becomes:
```ascii
M0---M1---M2---M3---B3'---M4---M5---M6---M7 master
                                          \
                                           \
                                            \
                                             B1'---B2'---B4'---B5' feature
```
As you can see rebase creates a much more linear structure. This creates a neat and clean patch series which is easy for reviewer to review. The patches sent are all on the current state of master and is optimized for reviewing.

If you had to do the same process but merging at each step instead of rebasing i.e. doing repeated merges. The structures would roughly become:
```ascii
M0---M1---M2------M3---M4---M5---M6---M7
 \         \       \                   \
  \         \       \                   \
   \         \       \                   \
    B1---B2---C1--B3--C2---B4---B5--------C3
```
C1, C2, C3 denote the merge with additional merge commits that are done for every merge. This is quite difficult for reviewer to check as multiple changes are tested in the master brought by multiple changes. All the changes must be understood by reviewer and hence, must analyze the older version of master to analyze the changes brought by the new branches.

The information on merge remain as-is. But, the history of rebase gets omitted in the process. I do not imply that the commits are lost but the useful record which might show how the rebased patches are related to original patches are not present.

Another important thing that can be done via rebase is we can transplant a branch based on one branch to another branch. This is done via `rebase --onto`.

Let's assume, you're working on a mobile app project. You have a basic login phase for the app. A team is working on an experimental branch to introduce new cool features. You fork the experimental branch to make login cooler by adding facial recognition login rather than just plain login.
The structure might look like this:
```ascii
o---o---o---o---o---o  master
     \
      o---o---o---o---o  experimental
                       \
                        o---o---o  facial_recognition
```

Now, your team working on facial recognition make a breakthrough and the functionality is ready for production but, unluckily many works in experimental branch is still a work in process. So, what do you do in this situation?
Well, `git rebase` has magical voodoo to handle this situation.
You do

```git
git rebase --onto master experimental facial_recognition
```
If there are no conflicts. This will handle the task perfectly and the structure now looks something like this:
```ascii
o---o---o---o---o---o---o  master
    |                     \
    |                      o'---o'---o' facial_recognition
     \
      o---o---o---o---o  experimental
```
This is what you wanted. Cool, right?

What if there were conflicts. Well, rebase will stop at first conflicting commit and leave commit markers in the tree. Use `git diff` and locate `<<<<<<` markers to find the problem. 
After editing the conflicting files do
```git
git add <filename>
```
If the conflict is resolved then, do
```git
git rebase --continue
```
To undo the git rebase
```git
git rebase --abort
```
Much more configurations and options are available for rebase. Do checkout the git-rebase [docs](https://git-scm.com/docs/git-rebase).

## Interactive rebase

Now, onward to another awesome rebase option `rebase  -i` which is generally said as "Interactive rebase mode". With interactive mode, you can edit the commits which are rebased, re-order the commits and remove them if unwanted or bad patches.

Interactive mode is preferable to use for the following workflow:
```ascii
1. you have a wonderful idea
2. you want to hack the code and you do
3. prepare a series for submission
4. submit
```
Interactive rebase can be more powerful than automatic rebase in certain scenarios as it can be used to alter commits as they are moved to new branch and offers complete control over the branch's commit history. It will help to clean up a messy history before merging a feature into master.

An example of interactive rebase:
```git
git checkout experimental
git rebase -i master
```
This will open the editor of your choice. Mine is Emacs. The editor will show listings of all the commits:
```git
pick 44d5c7a Message for commit #1
pick ******* Message for commit #2
```
Now can you change the pick command or reorder the entries to modify the branch's history.
Alternatively, you can merge two commits as a single commit if they portray the same message for similar change. This can be done via 
```git 
git commit --fixup <commit>
```
After saving and closing the file, Git will perform the rebase according to your instructions and you're the incharge in interactive mode while rebase handles automatically for you. This is a unique feature.

You can also split commits using interactive mode. You can add commits or split a commit into two.
To do this do the following:

1. Do 
```git
git rebase -i <commit>^
``` 
where the `<commit>` is the commit you want to split.

2. Mark the commit with action "edit".

3. Execute 
```git 
git reset HEAD^
```

4. Now add changes to the index that you want to have as first commit and do 
```git
git add -i or git gui
```

5. Commit the now-current index.

6. Repeat 4 and 5 until the working tree is clean.

7. Continue rebase with 
```git
git rebase --continue
```

There is also an important case where you need to recover from upstream rebase. The rebase [doc](https://git-scm.com/docs/git-rebase#_recovering_from_upstream_rebase) details nicely and enough for understanding it. Don't miss it.

There are people's different opinions regarding rebase and merge. People claim one is better than the other. But, rather than claiming superiority of one over another and creating another flame war, we must embrace both as both are very important for varying situations. The amazing thing about Git is it doesn't force one more over another. It does what the maintainer prefers. So, using one over another is a matter of taste and may be due to certain cases but, claiming never to use one and always use another one because it is better is doing un-justice to the tooling and missing out on a great tool. 

With that said, let's look onto the scenarios where you can use Rebase or the cases for using Rebasing.

1. Never use rebase on public branches.
   Never rebase the master branch onto your feature branch. Since, this will only trigger change in your repository and everyone will be working on original master. You will need to synchronize your master branch with the original one i.e. merge them back together because your master branch history has diverged from everybody else's. You will need two sets of commits that does the same change to fix and can be very confusing. So, think carefully before running `git rebase`.
   
2. Be careful or omit `--force` flag in pushing a rebased master branch into remote repository.
   Only use `--force` flag if you know what you're doing. The conflict messages are there to help you find the errors. One of the only times you might need to force-push is when you've performed a local cleanup after you've pushed a private feature branch to a remote repository.

## Best cases for Rebase use:

1. In a local cleanup. By using interactive rebase and coding you can make sure each commit in your feature is focused and meaningful. Introducing `git-rebase` into your workflow affects local branches and other developers only can see your finished product which is clean and has easier to understand branch history. But, this is only applicable for private branches.

2. Integrating an approved feature. If a new feature is approved by your team, you can rebase the feature onto the tip of the master branch before merging the feature onto the main code. If you rebase before the merge, the merge will be fast-forwarded and will result in a perfectly linear history. This also helps to squash any follow-up commits added during a pull request.

3. After making a pull request, you mustn't do `git rebase` as it becomes a public branch for others to review the code. Hence, before submitting the pull request/s, it is better to use interactive rebase to clean up the code.

There is another important flag `-p` or `--preserve-merges` which you can use with rebase.`--preserve-merges` will recreate merge commits instead of flattening the history. It will replay commits a merge commit introduces. The merge conflict resolutions are not preserved. `--preserve-merges` uses `--interactive` internally and combining it with `--interactive` option while rebasing can trigger a bug present which can produce dubious results when re-ordering the commits. The bug detail can be found [here](https://git-scm.com/docs/git-rebase#_bugs).

[Johannes Schindelin](https://github.com/dscho) has also worked in `--recreate-merges` option which is still in review process and in `pu` branch in git repository as of now. This option is an alternative to `--preserve-merges` and is created to deprecate it. @dscho has written clear detail of the working of `--recreate-merges` in the [mailing list](https://public-inbox.org/git/71c42d6d3bb240d90071d5afdde81d1293fdf0ab.1516225925.git.johannes.schindelin@gmx.de/).

All in all both merging and rebasing are equally important. Rebase is an awesome tool we're lucky to have in our toolbox. Some might complain that they had to bisect through several hundred commits to track down the bug which was introduced due to a faulty rebase. To prevent these problems, one of my mentor during this GSoC, [Christian Couder](https://twitter.com/christiancouder) [pointed](http://colabti.org/irclogger/irclogger_log/git-devel?date=2018-05-06) out that using `git rebase --exec make master` it's possible to check that each commit from master to the tip of the current branch builds. So, rebase can help people check that rebase did not introduce bugs. He also pointed out that using rebase with `--exec`, we can run tests for each commit while using rebase.

It's all about tastes of developers. But, having and using the rebase tool present in our Git toolbox can simplify many tasks and is perfectly suited for certain cases.

This post was about the philosophy and general idea behind "rebase" and the follow-up post will be of breaking down of the current script file of `git-rebase.sh` and show how that can be made a built-in.

With this my research phase is done and will now start implementing changes. You can find me in `# git-devel` as "prertik".

Shout out to all the awesome developers behind Git.

{{< figure src="/static/applause.gif" >}}

Thanks to [Christian Couder](https://twitter.com/christiancouder) and [Johannes Schindelin](https://twitter.com/jschindelin) for helping me research on git-rebase. 

[Alban Gruin](https://blog.pa1ch.fr/posts/2018/05/05/en/gsoc2018-week-1.html) is working on converting interactive rebase to C and [Paul-Sebastian Ungureanu](https://ungps.github.io) is working on converting `git stash` to built-in during this GSoC. Do check out their blogs.

## References

Thanks to Christian for sharing the awesome blogs by [Michael Haggerty](http://softwareswirl.blogspot.com/2009/08/rebase-with-history-implementation.html) who is one of the top committer on Git and has written an awesome tool [git-imerge](http://softwareswirl.blogspot.fr/2013/05/git-imerge-practical-introduction.html) which introduces some awesome ideas.

\_a\_lot\_of\_posts\_regarding\_rebase\_:

* (https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
* (https://medium.com/@fredrikmorken/why-you-should-stop-using-git-rebase-5552bee4fed1)
* (https://nathanleclaire.com/blog/2014/09/14/dont-be-scared-of-git-rebase/)
* (https://www.derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/)
