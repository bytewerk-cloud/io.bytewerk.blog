+++ 
draft = false
date = 2023-11-01T13:20:31+01:00
title = "Enforce uniform code formatting across teams"
description = "My thoughts on how to achieve a holistic, language agnostic code format across teams and projects."
slug = "uniform-code-formatting"
authors = ["Christian Lohr"]
tags = ["software engineering", "code quality", "standards"]
categories = ["software engineering", "software architecture"]
externalLink = ""
series = []
+++

With the rise of language centric formatting tools like `gofmt`, it's become easier than ever to achieve a holistic coding style across team members and projects, avoiding endless warfare over primeval subjective matters like [white space after punctuation](https://xkcd.com/1285/), tabs vs. Spaces, etc... It also combats the cargo cult around style guides promoted by large tech companies (such as the [AirBnB JS Guide](https://github.com/airbnb/javascript)) - although these are still around, they've become much more focused on overall code architecture and idiomatic usage of their governed language.

But how do you enforce the usage of a code formatting tool using a given rule set across a heterogeneous mix of operating systems, code editors, IDEs, shells and personal usage preferences? The most obvious choice that might spring to mind is to encompass this issue as part of your CI pipelines, running your code linting tools before builds and tests - and therefore actively encouraging your team to submit properly formatted code only.

Most modern IDEs will take care of these matters automatically, though *VSCode* uses a different rule set than *GoLand* for example. [EditorConfig](https://editorconfig.org/) exists as a way to unify settings across different engineering environments, though configuration can be cumbersome, and implementation support is pushed on to tool providers.

Over the past two years, I've therefore started using a different approach. There is no rule on which IDE or code editing tools to use, as long as you have the standard formatting and linting tools for your language installed on your local machine and are using `git` as your versioning software.

For example, the following snippet is taken directly from my `.git/hooks/pre-commit` file within our `go` mono repo, although the same setup would work for any other language providing default formatting tools.

```bash
#!/bin/bash

for file in $(git diff --cached --name-only --diff-filter=ACMRTUXB | grep "\.go"); do
  echo "(gofmt) $file"
  gofumpt -w $file
  gci write $file --skip-generated -s standard -s default
  git add "$file"
done
```

So, what does this actually? It iterates over all my changed `go` files that are about to be included in my commit. It then runs two formatter tools on each file and stages the changes again.

The `diff-filter` arguments ensure we only look at files which have **not** been deleted, you can find more information on their meaning [here](https://git-scm.com/docs/git-diff#Documentation/git-diff.txt---diff-filterACDMRTUXB82308203).

This allows me to completely ignore the style and format of my code, freeing up my brain to focus on implementation and architecture instead. Since passing the file around to coworkers, everyone has come up with their own little variations and adaptions, but we haven't come across a red CI pipeline due to malformed code once. 

It's nice to enjoy your CI waiting coffee in harmonic bliss. I worry about the failing tests enough as it is ☕
