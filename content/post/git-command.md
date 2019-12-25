---
title: "Git Command"
author: "Kunyi Lu"
date: 2019-10-20T18:43:55-07:00
tags: ["Git", "Command Line Tool"]
draft: false
---

_ _ _ 
This blog is a detailed summary of the Pro Git book. After I read the book, I made a summary of what commands are leveraged in practice frequently. 

# Getting Started
## What is Git
Git is a version control tool. 

**The Three States**

Git has three main states that your file can reside in: modified, staged, and committed

* Modified: files are changed but are not committed to database yet
* Staged(after `git add`): a modified file in its current version is marked to go into next snapshot
* Committed(after `git commit`): data is safely stored in local database

## First-Time Git Setup
Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates.

The first thing you should do when you install Git is to set your user name and email address. 
> $ git config --global user.name "Kunyi Lu"
> 
> $ git config --global user.email kunyilu@example.com

You need to do this only once if you pass the `--global` option, because then Git will always use that information for anything you do on that system. If you want to override the configuration, you can run this command without `--global` in a specific project. 

You can check your settings using 
> $ git config --list

# Git Basics
## Checking the Status of Your Files 
You can use `git status` command to check status of each file. While the `git status` output is pretty comprehensive, it's also quite wordy. If you run `git status -s` or `git status --short`, you get a far more simplified output from the command.

When you run `git status -s`, new files that aren't tracked hava a `??` next to them, new files that have been added to the staging area have an `A`, modified files have an `M` and so on. 

## Recording Changes to the Repository
View your staged and unstaged changes
    
> $ git diff

you can see what you've changed but not yet staged. 

> $ git diff --staged (--staged and --cached are synonyms)

you can see what you've staged that will go into your next commit.

## Removing Files 
> $ git rm

The file will be removed from your staging area and then commited. It also removes the file from your working directory. If you simply remove the file from your working directory, it will still show up under the "Changes not staged for commit" area. 

> $ git rm --cached

This command will keep the file in the working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of `.a` compiled files. 

You can pass files, directories, and file-glob patterns to the `git rm` command. For example:

> $ git rm log/\\*.log

Note that the backslash(\\) in front of the `*`. This is necessary because Git does its own filename expansion in addition to your shell's filename expansion. This command removes all files that have the `.log` extension in the `log/` directory.

## Viewing the Commit History
> $ git log 

Although the command to show the commit history is simple, a huge number and variety of options to the `git log` command are available to show you exactly what you're looking for. 

1. `-p` or `--patch`: shows the difference introduced in each commit. You can also limit the number of log entries displayed, such as using `-2` to show only the last two entries.
2. `--stat`: sees some abbreviated stats for each commit. Prints below each commit entry a list of modified files, how many files were changed, and how many lines in those files were added and removed. It also puts a summary of the information at the end. 
3. `--pretty`: changes the log output to formats other than the default. For example, `git log --pretty=oneline`, `git log --pretty=short`... Specifically, `format` allows you to specify your own log output format. 

> $ git log --pretty=format:"%h - %an, %ar : %s"

The `--graph` option adds a nice little ASCII graph showing your branch and merge history. 

## Undoing Things
When you commit too early and possibly forget to add some files, or you mess up your commit message, you want to redo that commit, make the additional changes you forgot, stage them, and commit again using the `--amend` option. 

> $ git commit -m "initial commit" <br/>
> $ git add forgotten_file <br/>
> $ git commit --amend

To unstage files, you can use `git reset`

> $ git reset HEAD file_name.md 

To discard the changes you've made, you can use `git checkout --`

> $ git checkout -- file_name.md

To delete recent commits, you can use `git reset --hard HEAD~{}`

> $ git reset --hard HEAD~1

## Showing your Remotes
To see which remote servers you have configured, you can run `git remote` command. If you've cloned your repository, you should at least see `origin` - that is the default name Git gives to the server you cloned from. 

# Git Branching

## Basic Merge Conflicts
If you changed the same part of the same file differently in the two branches you're merging, Git won't be able to merge them cleanly. After you've resolved each of these sections in each conflicted file, run `git add` on each file to mark it as resolved. 

## Rebasing
With the `rebase` command, you can take all the changes that were committed on one branch and replay them on a different branch. 

> git checkout experiment <br/>
> git rebase master <br/>
> First, rewinding head to replay your work on top of it...
> 
> git checkout master <br/>
> git merge experiment

# Git Tools
## Stashing and Cleaning
Often, when you've working on part of your projec, things are in a messy state and you want to switch branches for a bit to work on something else. The problem is, you don't want to do a commit of half-done work just so you can get back to this point later. So you can use `git stash`.

> git stash <br/>
> git stash apply 

## Rewriting History
Many times, when working with Git, you may want to revise your local commit history. 

> git rebase -i commit_id <br/>
> evry commit in the range commit_id ... HEAD with a changed message and all of its descendants will be rewritten.

# Reference
* Git official doc: [https://git-scm.com/doc]