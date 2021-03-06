---
title: "Git Tips"
category: GIT
Date: 2013-08-09T00:00:00+08:00
tags: [GIT, Linux]
---

# Git Tips

## Revert a git patch

Chengdu Gerrit has “Revert Change” button, so you can revert patch very easy.
But, Shanghai Gerrit version is too old, it doesn’t support “Revert Change” on Gerrit, for this case, you can try the below git commands.

Check git repository status:

    $ git status

Make git repository clean:

    $ git checkout .
    $ git clean -fd

Find &lt;commit&gt; which you want to revert:

    $ git log

Revert patch:

For plain text files:

    $ git show &lt;commit&gt; | git apply -R

For binary files:

    $ git show --binary &lt;commit&gt; | git apply -R

## How to tell which commit a tag points to

Summary: Since there are about 4 almost equally acceptable yet different answers I will summarise all the different ways to skin a tag.

* git rev-list $TAG | head -n 1 (thanks mipadi). git rev-list outputs the commits that lead up to the $TAG similar to git log but only showing the SHA1 of the commit. For non-unix heads like myself head -n 1 will show the first line of the output, which is the tip of the tag.
* git show-ref --tags (thanks Charles Bailey) will show all tags (local and fetched from remote) and their SHA1s.
* git show-ref $TAG(thanks Jakub Narębski) will show the tag and its path along with the SHA1.
* git rev-parse $TAG(thanks Jakub Narębski) will show the SHA1 of an unannotated tag.
* git rev-parse --verify $TAG^{commit} (thanks Jakub Narębski) will show a SHA1 of both annotated and unannotated tags. On Windows use git rev-parse --verify %TAG%^^^^{commit} (four hats).
* `cat .git/refs/tags/*` or `cat .git/packed-refs` (thanks The MYYN) depending on whether or not the tag is local or fetched from remote.

reference: <http://stackoverflow.com/questions/1862423/git-how-to-tell-which-commit-a-tag-points-to>

## Switch current branch in git bare repository

I actually want to remove a branch in the bare repository i am working with, but this task hits a dead end because I cannot switch away from the master repository without a 'work tree' which a bare repository does not have.

When I run "git branch -d master" the output is:

    error: Cannot delete the branch 'master' which you are currently on.

So I try to switch to another branch called 'develop' by running "git checkout develop" and the output is:

    fatal: This operation must be run in a work tree

Solution:

    git symbolic-ref HEAD refs/heads/develop
    git branch -d master
    git branch -a # check the current default branch

reference: <http://stackoverflow.com/questions/4468322/switch-current-branch-in-git-bare-repository>
