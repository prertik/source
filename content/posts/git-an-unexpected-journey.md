+++
date = "2018-05-01T10:00:00+5:45"
title = "Git: An Unexpected Journey"
tags = ["Git", "C", "Shell", "Open-source"]
categories = ["git"]
description = ""
images = [
    "https://prertik.github.io/static/sanic.png"
]
next = ""
+++

This summer of '18, I'll be working in Git. Git is an insanely
awesome software. I still remember the day I learned about Git via
Github and instantly got hooked. Git isn't just an everyday software
you encounter, just look at the documentation and observe closely
how it works and you will understand the rock solid design.
Enough with Git introduction already. There are _too_ _much_ posts
describing Git already.

## Sending Patches

Rather than going to the theory of Git, lets learn how to send patch
to Git. Sending patches to Git is different than sending patches to
other JS Frameworks, Game Engines, etc. You don't use Github to send
patches to Git. [Documentation/SubmittingPatches](https://github.com/git/git/blob/master/Documentation/SubmittingPatches)
already contains a great deal of information for anybody to learn
how to send patch.
But, let me summarize it.

Ok, you've modified some code. You check it and believe it doesn't break
anything else. You run tests on it. It looks good. You follow the
standard C writing guideline. So, you decide to send it.
First you must do is fire up terminal and run `git diff --check`.
If you don't see anything there, it's good you have not introduced
whitespaces in your code. But, if you do see codes there fix
those whitespaces. Git community is picky about whitespaces.

Then run `git log <file you modified>` and see all the changes you made.
Now, think of good commit message. This is very important as it
describes the maintainers what you are fixing. Write the first line
of the commit message with a short description of about 50 characters.
Also, prefix is needed for the first line like:
`test: avoid pipes in git related commands for test`
Now, in after the short description, you should write a lengthier
commit message describing the changes you made. The commit message
should be in imperative mood. e.g. "avoid using pipes ...".
This should be very descriptive and very clear of what you've done.
Also, remember to sign-off your patch. Run `git commit <files> -s`.

Now, time to generate your patch. Git has a tool for generating patches.
Cool, right? Run `git format-patch HEAD~` to generate a single patch
for your changes. If your change is of only a single type i.e. change
in typo in different files then, only generate a single file. But, if
your changes are in different files of different nature then, run
`git format-patch HEAD~<number of commits to convert to patches>`.
Remember you'll need multiple commits if you've changed multiple files
differently e.g. you changed a test file for typo and `git-rebase.sh`
to add some extra functionalities, then you have to write two commit
messages describing both changes.
Now, a patch file or multiple patch files will be generated depending
on your changes. You can apply this patchfile by running `git am <patchfile>`.

Now, time to send to patch to the mailing list. Git has a tool for this
too. Run `git send-email <your-patch-file>`. Now, to use send-email, you need
to set up your mailing id and format message with the mail. This should be
in plain text. Follow [git send-email documentation](https://git-scm.com/docs/git-send-email)
closely. This explains everything in much more detail.
Your patch now, goes through lot of review process. Follow closely,
discuss with the community, understand and acknowledge their comments.
Write quality codes rather than large quantity.


## GSoC '18

So, in this GSoC timeline. I'll be working to make `git rebase` a builtin.
i.e. `git rebase` is previously written in shell scripts. My job is to convert
it into C.
Why is this necessary one might ask.
Well, Many components of Git are still in the form of shell and Perl scripts.
This has certain advantages of being extensible but causes problems in production
code on multiple platforms like Windows. I'm going to rewrite a couple of shell
and perl scripts into portable and performant C code, making them built-ins.
The major advantage of doing this is improvement in efficiency and performance.

So, what does rebase do?
There's a cool begineer friendly introduction to rebase [here](https://dev.to/maxwell_dev/the-git-rebase-introduction-i-wish-id-had)
Or, you can follow, [git-rebase documentation](https://git-scm.com/docs/git-rebase).
I'm not going to explain again here as it is already done in a much
better way in both the git documentation and the post.
Now, this is just a beginning post. I will be updating and keeping
a series of what I learn from Git. I will be writing every week.
This will be a great journey.

Ah, nearly forgot to mention. If all of our patches in GSoC '18 gets
merged into Git. Git will be fast. How fast you might ask? Time will tell.

{{< figure src="/static/sanic.png" >}}
