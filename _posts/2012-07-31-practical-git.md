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

coming soon

