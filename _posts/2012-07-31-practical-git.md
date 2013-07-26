---
layout: post
title: Practical Git
---

The purpose of this post is to give a guided tour of git from the perspective of a non-professional programmer. My research involves
a fair amount of programming, and while it took me some time to master git, I can clearly see the positive impact it had on my workflow. 
Below are some of the commands I find myself using a lot:

{% highlight bash %}
# Creates three files
touch foo bar bli

# Creates the repo
git init

# Note that you can initialize a repo
# even if you already have files in your
# directory.

# Adds two of the files to the "commit zone"
git add foo bar

# Commits the changes
git commit -m "Initial commit"

# At this point, git tracks two files. You can 
# check it using the following command:

git ls-tree -r master

# We will come back to this command and explain the options
# later.

# Next, let's modify one of the files

echo "a" >> foo

# You can check that git noticed the modification by typing:

git status

# The files modified appear under the section "files not staged for commit",
# in red.
# Let's stage your file for commit:

git add foo

git status

# Now the file appears in green. Note that the add command is the same we 
# use before. The first time, it did two things at once: it told git to 
# start tracking the file AND added it to the commit zone. This time, git 
# was already tracking the file, so it just added it to the commit zone.

echo "b" >> bar  

git status

# We modify the second file, and see that it appears in red, while the first
# one is in green. That's before the second has not been added to the commit
# zone. What happens now if we commit the changes?

git commit -m "second commit"

git status

# You see that your first file has been committed, but the second file hasn't, it's
# still in red outside the commit zone. Ok, so that means that only the files in the 
# commit zone get committed, that makes sense.

git add bar
git commit -m "third commit"

# Cool, now bar is committed too. Now watch out:

echo "a" >> foo
echo "b" >> bar

git add foo

git commit -am "fourth commit"

# You would expect foo to be committed, but not bar. But notice the -a option. This stands
# for "all". That is, it tells git to commit every file that's been changed since the last
# commit.

# Now let's examine all our commits so far:

git log 

git log --pretty=oneline

# They show the same thing, with different levels of details. 

{% endhighlight %}



## commit vs commit -a

It took me some time to understand the difference. In short, __commit__ will commit only the files that you have added using the command
__add__ (in green), whereas __commit -a__ will commit all the files that you have modified (green and red), whether you added them or not.

Example:

Suppose you have 3 files in your directory: foo, bar, bli. 

You create your repo:

{% highlight bash %}
git init
git add foo bar
git commit -m "Initial commit"
{% endhighlight %}

Nothing fancy: git is now tracking two files.

Now let's modify both files:

{% highlight bash %}
echo "a" >> foo
echo "a" >> bar
{% endhighlight %}

Now see the difference:

{% highlight bash %}
git add foo
git commit -m "this will only commit the changes on foo"
{% endhighlight %}


And:

{% highlight bash %}
git commit -am "this will commit both foo and bar"
{% endhighlight %}


Note that doing:

{% highlight bash %}
git add foo
git add bar
git commit -m "this will commit both foo and bar"
{% endhighlight %}

is naturally equivalent to using __commit -a__

## branching, branching, branching

Branching is as easy as typing

{% highlight bash %}
git branch myNewBranch
git checkout myNewBranch
{% endhighlight %}

The first line above creates a new branch _myNewBranch_, 
and the second switches to this branch. Note that there
is always at least one branch: the branch called _master_.

What are branches used for? Imagine you have a project, and
you want to add a new feature to your project, call it _feature 1_.
You know that this will involve deep changes of the code, and
you don't want to mess up your working code, especially if you
don't know how long it's gonna take to implement you new feature.

That's a typical use case for branches. the worklow is the
following: 

{% highlight bash %}
git branch feature1
git checkout feature1
< .. do some stuff ..>
< .. do some commits ..>
git checkout master
git merge feature1
git branch -d feature1
{% endhighlight %}

The first two lines we already know: we create a new branch
and switch to it. We then make some changes, commit them, make
some more changes and commit them again. Now we're satisfied
with our new feature and we want to integrate it to the main branch.
The _merge_ command does exactly that. Once the branch is merged to 
the master, we no longer need it, and so we delete it with _branch -d_.

## What's change since the last commit? -- git diff

There are two things that you might want to know before committing 
some changes:

1) What are the things that got modified since the last commit, and that
are about to be committed (ie, are staged for commit)?

You can get this information with:

{% highlight bash %}
git diff --staged # or git diff --cached
{% endhighlight %}

2) What are the things that got modified since the last commit, and that 
are not included in the next commit (unstaged)

That's an even shorter command:

{% highlight bash %}
git diff 
{% endhighlight %}


## Removing files from git

Here again, you might want one of two things:

1) completely remove the file from your system, and untrack it

{% highlight bash %}
git rm <file to remove>
{% endhighlight %}

2) keep the file on you system, but ask git to stop tracking it

{% highlight bash %}
git rm --cached <file to remove>
{% endhighlight %}


## Undoing things

There are a number of ways to undo things, or modify things. 

Say you committed something, and realize you forgot to include
a file in the commit. You can fix that:

{% highlight bash %}
git add forgottenFile.txt
git commit --amend -m "new commit message replacing old one"
{% endhighlight %}

Note that you don't need to add a commit message when you amend
a commit.

Now another scenario. Say you made modifications to a file, staged
it for commit, but realize that your modifications suck, and that
you want to keep the previously committed version.

{% highlight bash %}
git reset HEAD badmodif.txt # first you need to unstage file
git checkout badmodif.txt # then you retrieve the file as it was in last commit
{% endhighlight %}

coming soon..

