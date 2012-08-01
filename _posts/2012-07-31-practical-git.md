---
layout: post
title: Practical Git
---

I am a scientist, not a programmer. But I do spend a significant amount of time programming, every day, and learning how to use
git really helped me. That being said, I don't need to use all the features of git, and I suspect that most non programmers don't
need to either.

Below are the main commands I have found useful. I'll assume that you have already used git, and can use its basic functionnalities:

## commit vs commit -a

It took me some time to understand the difference. In short, __commit__ will commit only the files that you have added using the command
__add__ (in green), whereas __commit -a__ will commit all the files that you have modified (green and red), whether you added them or not.

Example:

Suppose you have 3 files in your directory: foo, bar, bli. 

You create your repo:

``` console
git init
git add foo bar
git commit -m "Initial commit"
```

Nothing fancy: git is now tracking two files.

Now let's modify both files:

``` console
echo "a" >> foo
echo "a" >> bar
```

Now see the difference:

``` console
git add foo
git commit -m "this will only commit the changes on foo"
```

And:

``` ruby
git commit -am "this will commit both foo and bar"
```

Note that doing:

``` bash
git add foo
git add bar
git commit -m "this will commit both foo and bar"
```

is naturally equivalent to using __commit -a__

 
