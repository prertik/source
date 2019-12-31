+++
date = "2019-12-31"
title = "Static Typing on a data-intensive project - Part I"
description = ""
draft = false
categories = ["nsfw"]
tags = ["python", "static", "typing", "programming"]
+++

**Disclaimer: These posts on **NSFW** do not reflect the thoughts of my employer
and in no way an outcome of my employers.**

## Overture:

> Every decade we get heavy buzz on type systems from proponents of different
> type systems. In the early days of Python and Ruby, the buzz was more in the
> favour of dynamic type systems but, the times they are a-changin' or are they?

There's been some buzz around python community for some years about type systems.
And it sounds really fun and some great ideas have come from
[dropbox](https://blogs.dropbox.com/tech/2019/09/our-journey-to-type-checking-4-million-lines-of-python/)
and
[GvR](https://www.techrepublic.com/article/the-creator-of-python-on-how-the-programming-language-is-learning-from-typescript/).
Yes, static typing really has several benefits as, pairing it with the editors and
IDEs grant awesome constraint checking during development rather than the runtime.
And, the way python started with adding optional [type hints](https://www.python.org/dev/peps/pep-0484/)
is a really good move. Lately [other](http://mypy-lang.org/) [tools](https://github.com/microsoft/pyright)
which ride open the optional type hints of python have matured and are readily
been used on lots of open-source projects and big python companies like: Dropbox,
Microsoft.

I have experience using mypy for static type checking a python project and
it is delightful to use and it is *hellish* fast. Some parts of it has been
written on C for performance as, we are *aware* of 
python's [performance issues](https://benchmarksgame-team.pages.debian.net/benchmarksgame/measurements/python3.html). 
Dropbox's post agrees:

> We did some profiling to find the most common of these “slow operations”. Armed with this data, we tried to either tweak mypyc to generate faster C code for these operations, or to rewrite the relevant Python code using faster operations (and sometimes there was nothing we could easily do). The latter was often much easier than implementing the same transformation automatically in the compiler. Longer term we’d like to automate many of these transformations, but at this point we were focused on making mypy faster with minimal effort, and at times we cut a few corners.

The recent moves made by Python is inspired by the huge success of TypeScript.
Ofcourse, there are people too in the JavaScript ecosystem who have been [putting](https://levelup.gitconnected.com/kyle-simpson-ive-forgotten-more-javascript-than-most-people-ever-learn-3bddc6c13e93)
[off](https://twitter.com/dan_abramov/status/1082460320009015296?lang=en) TypeScript.
Though, it is mentioned that having static typing on big projects is very important.

## Core:

Now, let's fast forward to the reason for this blog.
Python is a data modeling language. What I mean by this is, data can be modeled
and manipulated in python than other languages and the Python data model describes
APIs which are excellent and well suited for the data modeling scenarios. Though,
this data model is often called Object Model but, let's stay on the data model
faction for now.

During my work on a medium code base (> 50k lines) of python, we had twice thought
of trying static typing and worked on type annotations and integrating `mypy` on
the code base for extra static typing goodness. Of course, the recent buzz motivated
us towards it rather than the real requirement of static typing as, though our
project had been running for some time and solving issues on a domain for some
time, we still hadn't reached the **big corporate needs** as we didn't feel
static typing was our savior.

So, we worked to add type annotations to our code-base, now we are providing
solutions for education domain and we would receive *a lot* of information and
have to solve problems my modeling our python code-base to the information we receive.
The problem with information is information is sparse, we cannot force the
information to be in a certain way but must have our programs model the information
we receive. Secondly, information we receive is open ended i.e. it has no limit or
boundary and these information keeps on flowing. The problem with having to program
for information we cannot contain and not knowing the state of information is that
we cannot model our program to handle the information the first time, as we keep
on iterating stuffs and even, have to isolate information sources for our program.

Let me give you an example, since, our project is of education domain, we keep
on having issues with `id` of students as, some information source find it
better to have id as string, some as integers, some as string with characters and
so on. Now, suppose we add type hints to our python code base which models this
information.

```python
def get_student_name_from_id(id: int) -> str:
  """code to retrieve student name from the given id"""
  
  # handle all the logic

  return student_name
```

Here, we are annotating with the information received that, the id is of `int`
type. Not caring for other type. Yes, changing this `int` to `str` for the
information received is straight forward too, as this is a single function.
But, will only a single function use this? In a good team of python programmers.
Won't there be many methods or functions which use `id` for *whole lot of stuffs*.
There can be code present like this, (and I can somewhat nod in it's existence)

```python
def find_high_performing_students(id: int, area: str) -> str:
  # codes like these
  
  return student_name
```

Now, we have to remodel lots of stuffs when the information changes. Then, for
each new source of information, we'll either have to isolate the program 
for that particular source or version our program for a source or the better
way, to handle `id` which can be `int` or `str` and then, type cast it or
parse it, depending upon the scenario. So, we'll have to think time and again we
change information source on the possible solution for this, our [pointy haired
boss](https://en.wikipedia.org/wiki/Pointy-haired_Boss) won't be happy with this,
as the boss is concerned with the graph and will sound the concern as why we
are facing this issue time and again. Still I haven't discussed the third issue,
i.e. string `id` with characters that will need some serious modeling.

Then, again, there [are](https://monkeytype.readthedocs.io/en/latest/index.html)
[tools](https://github.com/dropbox/pyannotate) for finding `type` which collects
runtime types of function arguments and return values. Yes, these tools are
great and are medicine for the right itch, but, the problem you see, here are,
we're kind of depending on the dynamic characteristics of python to obtain that
for us and our code is fine without us going the static `type` way, yes, having
type systems will be much better but, even without it, we can survive and have
gone so far.

## Underture:

In this [talk](https://www.youtube.com/watch?v=2V1FtfBDsLU&t=37m07s) by Rich
Hickey, he questions the values of static types which is a perfectly valid
question as, it is difficult using static or proper type systems when, we're
modeling for the real world information which is unstructured and disoriented.

[Steve Yegge](https://en.wikipedia.org/wiki/Steve_Yegge) has some amazing thoughts
on his 2007 masterpiece [The Pinocchio Problem](https://steve-yegge.blogspot.com/2007/01/pinocchio-problem.html):

> A static type system has absolutely no runtime effect on a correct program: exactly the same machine code is generated regardless of how many types you chose to model. Case in point: C++ code is no more efficient than C code, and C lacks any but the most primitive type system. C isn't dynamically typed, but it's not particularly statically typed either. It's possible to write fast, robust programs in straight C. (Emacs and the Linux kernel are two fine examples.)

> Because most programmers nowadays prefer to build marionettes rather than scary real children, most programming is oriented towards building layer upon layer of hardware. C++ and Java programmers (and I'd be willing to bet, C# programmers) are by default baking every line of code they write into hardware by modeling everything in the type system up front.

> It's possible to write loosely-typed C++ and Java code using just arrays, linked lists, hashes, trees, and functions, but it's strongly deprecated (or at least frowned upon) in both camps, where strong typing and heavy-duty modeling have been growing increasingly fashionable for over a decade, to the detriment of productivity (which translates to software schedule) and flexibility (which also translates to schedule, via the ability to incorporate new features.

Apologies for quoting Steve Yegge *a lot*. But, he is *really* really amazing programmer and writer.
This blog post gets it right, even today.

But, we have come a long way from there and have *a lot* of new languages with
new ideas how to go around the issues imposed by type systems(both camp, I'm sorry)
which I'll discuss on next post and model it in python to solve our particular
problems.

---
Thanks to [Robus Gauli](https://github.com/RobusGauli) and [Rabin Gaire](https://github.com/rabingaire) for good discussions on type systems.
