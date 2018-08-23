# Version control

Here we will not discuss why version control in applications is needed, that should be
self-explanitory, but we will try to define good and bad practices.

## VCS

### No cvs, svn or other ancient version control systems

I know that some ancient companies still use these, but centralized version control
systems are in general deprecated, you should not be using them, period.

### In general, just please use GIT

If you od not have other special use cases, just please use GIT, everyone uses GIT,
GIT is like a standar for version control. 

### Do not deploy your GIT server, if not needed

There are free git providers like bitbucket and gitlab, that in general have better
security as your personal git server, so if you do not need do not deploy your GIT
server. If you need to deploy your git server deploy gitlab or gogs, which are simple
to deploy and not very hard to maintain.

## Commits

## Make verbose, contextual commits

When i see commits like "update" or "make some changes" i go like:

![](http://s2.quickmeme.com/img/78/788d7237ecca60d8d235cb352eab832f472264065a048fcd24adc834f0ca82f0.jpg)

Please take a time to write this commit message and give it also some context, so the one viewing your
commits will quickly see if commit is worth his time. It will also make pull requests much easies, as
pull request will include commit message.

### Example of good commit message

```schema/record: add support for versioning```

In this commit you clearly see it makes changes in record schema, and that it adds versioning support to it.

### Split commits in logical units

I know you are lazy, or in a hurry or don't know how to split commits in logical units,
but please learn, and don't be lazy, it will make everyone reading your code(including
yourself) like you more. You will in general be a better person. If you do not split,
at least document all the changes you've made in a commit.
