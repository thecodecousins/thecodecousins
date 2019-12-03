---
title: "Git Revert to a Good Commit"
date: 2019-12-01T14:28:16+07:00
authors: ["kn"]
tags: ["git", "trick", "blog"]
keywords: ["git"]
description: "How to revert to a good commit"
showFullContent: false
cover: "https://imgs.xkcd.com/comics/git_commit.png"
---


Today my co-worker asked me how to revert to an old and good commit on master ( after you accidentally merged `dev branch` to `master` ).
There are many ways to do that, for example we can use `reset` to discard all wrong commit to revert back, like `undo` when writing text. 
You can find others way on[`this topic on Stack Overflow`](https://stackoverflow.com/questions/4114095/how-do-i-revert-a-git-repository-to-a-previous-commit)
But the problem is too many options, we dont know which one is good, or even dont need to understand - it just works.

So i will introduce you my traditional way and extremely accurate.

First of all, after you did a `commit` and `merge` you should not delete commit with `git reset` (Undo) thus we want to repair it instead of delete it.

Next, in general, between any two `commit` in you git repository is a different line of codes, which you can always see wit the git `diff` command. 
Revert an old commit is to delete these different.

Suppose we are in `master` branch and we want to revert back to commit `8398e5b`. 
So you first need to `clone/checkout` *master* branch to a new clean folder ( the directory where you type `git status -u` and you see nothing there.)

```bash
$ git status -u           # make sure it is clean
$ git checkout master     # be sure you are in master
$ git diff HEAD..8398e5b  > patch.diff
```

In the last command, the order is very important.
The `commit` we want to revert to must be in the end. 
Then you apply this diff

```bash
$ patch -p1 < patch.diff
$ git status -u
```

Check if any new files have not been commited in the result of `git status -u` above, these are the files included in the old commit **8398e5b**:

```bash
$ git add some/new/files.txt
$ git commit -m "Revert to 8398e5b, thanks to @kn"
```

To be sure, check the results:

```bash
$ git diff 8398e5b..
```

This time it should appear ... nothing, that is, you have completely restored your old commit. 🎉🎉🎉

This way is always successful, simple, you can even take advantage of a few things to adjust. 
And above all, instead of having to understand a bunch of commands like `git revert`, `git reset`,... you just focus on understanding the problem of **diff, diff, and diff**
