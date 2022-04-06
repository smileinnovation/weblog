---
layout: post
url: https://medium.com/@/e09ec171f4d0
title: AI for code
subtitle: AI allows us to understand and measure codebase in a new way, and those last years have been exciting with startups trying to get the best…
slug: ai-for-code
description: 
tags: 
- development
- ai
- nlp
- code
- tech-manager
author: fagas
---

AI allows us to understand and measure codebase in a new way, and those last years have been exciting with startups trying to get the best of what AI on code could do.

![Generative art by Eric Davidson](/assets/images/posts/0*kPWj7mUpWc1GfVUn.jpg)

# Why should I be interested?

**Legacy management is not an en easy task**, test or documentation are most of the time missing so adding feature easily introduce new bugs, and upgrading libraries for the sake of security require a huge amount of time, and much more when you have to fix it later on. That’s one point for automation.

Without tests and documentation, **you are slow to deliver new features**, and when you do, you are more likely to fail and take forever to recover. That’s another point for automation.

But why tests and documentation are missing? Because** tests are boring** compare to the huge dopamine rush brought by the possibility of changing the world with few lines of code. A developer would happily spend their time solving issues rather than doing dummy tasks.

**That’s 3 points in favor of automation in code.**

# State of the art AI for code automation.

[Source{d} lead a Fosdem track in 2019 on Machine learning on code](https://archive.fosdem.org/2019/schedule/track/ml_on_code/), and it was mindblowing. Here are few talks that inspired me and a few applications in the tech market, from [GitHub Copilot ](https://copilot.github.com/)to [Ponicode](https://www.ponicode.com/).

This what the next-gen development part could be like.

### AI for code analysis

[Understanding Source Code with Deep Learning](https://archive.fosdem.org/2019/schedule/event/ml_on_code_understanding/)

> Code is written by humans for humans and machines. By learning from the human-oriented components of code, recent research has invented models that start to “understand” some aspects of source code. Using Graph Neural Networks to learn from the rich semantic relationships within code and, by training them on a self-supervised task, they have allowed us to find bugs in open-source projects.

*by [Miltos Allamanis](https://archive.fosdem.org/2019/schedule/speaker/miltos_allamanis/) @ Fosdem 2019*

### Pair programming with AI

[Suggesting Fixes during Code Review with ML](https://archive.fosdem.org/2019/schedule/event/ml_on_code_code_review_suggestions/)

> Reading foreign code is hard, and suggesting improvements is even harder. Yet a dramatic portion of code review time goes to figuring out the boring details: formatting, naming, micro-optimizations and best practices. We believe that all of those can be automated with ML on Code

[Github Copilot ](https://copilot.github.com/)
You get suggestions for whole lines or entire functions right inside your editor.

![One of the biggest news on the subject, let’s see how it goes.](/assets/images/posts/0*7aVhfwVVEfnPUKi8.png)

### Unit test generation & code documentation

Units testing is a cornerstone for software delivery and a bottleneck that automation tools have to tackle to ease everyone’s life.

A french company lead made [Ponicode](https://www.ponicode.com/) VSCode extension by Patrick Joubert ex recast founder, who decided to turn his NLP knowledge to code, especially Unit test generation, as a starter and much more are coming.

![Ponicode raises $3.4 million to develop AI that automates code testing](/assets/images/posts/0*9trx1nKl3RdvNm1b.png)

[Diffblue](https://www.diffblue.com/) IntelliJ extension for Java to catch regressions earlier in your pipeline with Java unit tests that are automatically created by Diffblue Cover

<iframe src="/assets/images/posts/f1f69bdae61041ff4512d2952fdda9a1.html"></iframe>

[Codist.ai](https://codist-ai.com/) Your projets deserve to be 100% documented and you don’t need to think twice about it 🧠 CodistAI helps document source code faster.

![](/assets/images/posts/0*4ElimCQZCPPgZo7k.jpg)

### Code metrics

During the last year, Covid emphasized the need to have clear metrics on code and solutions like Athenian try to solve the issue of understanding the tech team's outputs and bottlenecks based on code analytics.

It’s not their first attempt, as members used to work for Source{d}.

![](/assets/images/posts/0*kh00ft4NX5oU6MgQ.png)

https://www.athenian.co/

### Code migration, from bare metal to Cloud Native

Moving from one language to another is a long-awaited dream with at the moment falling scaling result: from to COBOL to C#, from python 2 to 3…

[How to build an automatic refactoring and migration toolkit](https://archive.fosdem.org/2019/schedule/event/ml_on_code_automatic_refactoring/)

> Every code base needs to be modernised at some point, either to reduce the technical debt or to migrate to another language. In such complex and challenging projects, automation is a key point to reduce the workload of low added-value tasks as monotonous refactoring or redundant paradigm translation.

by [Juliette Tisseyre @ Fosdem 2019](https://archive.fosdem.org/2019/schedule/event/ml_on_code_automatic_refactoring/)

# The future of software development is automation

Tech managers & developers should dig into AI applied to code that enhanced developer productivity.

It’s time to treat your code like your treat data, with meaningful KPI/OKR and recurrent data quality tasks.

* what could be automated: unit tests and other low-level but critical tasks

* what could be accelerated: code migration, switching legacy applications to cloud-native

* we could be assisted: PR auto-tagging, pair programming, or snippet autosuggestion

It’s definitely not a walk in the park as key challenges to solve will be on the road: intellectual property, license, quality of code suggestion, security issues, bias in training data, garbage code in, garbage code out…

And if you care to have a deeper look at the opportunities brought by AI on code, there is always an awesome repository[ listing solutions & capacities](https://github.com/src-d/awesome-machine-learning-on-source-code).

Have fun with your new superpowers.

# Thanks for reading

Did you enjoy it? If so, don’t hesitate to 👏 this article or s[ubscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)


