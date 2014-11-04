Better Code Review
==================

Makes it enjoyable for lazy engineers to review sophisticated code.

## Why Better Code Review?

When I worked at Twitter I often received the complaint that my code was too sophisticated and my colleagues couldn’t review it. That was definitely bullshit. They *could* review my code — they were just being lazy.

But software engineers are supposed to be lazy, so we just have to accept and enjoy laziness.

Although I did my best to present my code well, no one ever reviewed it, so, eventually, I quit.

Today, I believe I’ve solved this problem with *Better Code Review*, a better code-review tool that makes it very enjoyable for lazy engineers to review sophisticated code.

So, next time your reviewer complains that your code is too sophisticated, don’t quit your job. Just write your documentation in better-code-review format and continue making big money.

The big idea behind Better Code Review is to present code and its documentation in [Sidenote format](http://bettercodereview.org/#0#3#main:Top#sidenote:Sidenote%20format).

Check out this example: [pyalgebra.py](http://bettercodereview.org/#0#3#main:Top#pyalgebra:pyalgebra.py)

Maintainer: [Yana Zendo](http://yanazendo.org)

## Demo and homepage

[http://bettercodereview.org](http://bettercodereview.org)

## Engineering methodology

To develop Better Code Review I am using a novel engineering methodology. If you are interested in collaborating, please review this methodology [here](http://bettercodereview.org/#0#3#main:Top#development-method:engineering%20methodology).

## Feature requests

1. Implement Better Code Review. 

## Regarding feature request #1

You have a tremendous amount of discretion in choosing how to implement Better Code Review and choosing what features to implement. In general, you should optimize to create the best user experience. As a secondary concern, your code itself should be beautiful, elegant, and correct. You are also expected to use your implementation of Better Code Review to document your code.

I think an MVP implementation can be done straightforwardly by developing several components:

1. The better-code-review compiler will read in a source code project, and produce a Sidenote project as output.
  - Because I love Python and Scala, Python and Scala compilers are most appreciated.
2. better-code-review.js will be a Sidenote plugin. It will probably depend on sidenote.js. It will add all the features to Sidenote that are necessary to create the better-code-review web experience.

If you want to be paid for your implementation: file an issue, suggest a price, and we will negotiate from there.
