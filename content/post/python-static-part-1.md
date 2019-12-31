+++
date = "2019-12-31"
title = "Static Typing on a data-intensive project - Part I"
description = ""
draft = true
categories = ["nsfw"]
tags = ["python", "static", "typing", "programming"]
+++

There's been some buzz around python community for some years about type systems.
And it sounds really fun and some great ideas have come from [dropbox](https://blogs.dropbox.com/tech/2019/09/our-journey-to-type-checking-4-million-lines-of-python/) and [GvR](https://www.techrepublic.com/article/the-creator-of-python-on-how-the-programming-language-is-learning-from-typescript/). Yes, static typing really has several benefits as, pairing it with the editors and IDEs grant awesome constraint checking during development rather than the runtime. And, the way python started with adding optional [type hints](https://www.python.org/dev/peps/pep-0484/) is a really good move. Lately [other](http://mypy-lang.org/) [tools](https://github.com/microsoft/pyright) which ride open the optional type hints of python have matured and are readily been used on lots of open-source projects and big python companies like: Dropbox, Microsoft.
