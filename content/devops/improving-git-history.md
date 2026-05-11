---
title: Untitled 4
tags: []
draft: true
date: 2026-05-01
---
This post is a note to the guide `How to be a Git Wiz` by Geoff Pleiss
https://geoffpleiss.com/static/media/git_wizard.aeee2897.pdf

`git rebase -i HEAD^^^^^` -> merge WIP commits into a single commit + reorder commits + cannot rewrite the history after you've pushed

```
git merge --ff-only <branch> #foces no merge commit
git merge --no-ff <branch> #forces merge commit
git rebase <branch> #commits are merged on top of <branch>
```

commands to improve git history
```
.gitignore / git clean -nd
git commit --amend
git revert
git add/checkout/reset -p
git rebase -i 
git clean -df #remove untracked files

```

mastering git reset 
```
git reset --soft HEAD^ #undo last commit, keep changes in current working 
git reset --hard HEAD^ #undo last commit, completely remove the changes
git revert HEAD^ #add new commit that undoes the last previous commit 
```

make atomic commit
```
git add -p <file> #choose which lines to stage
git reset -p <file> #choose which lines to unstage
git checkout -p <file> #choose lines to undo
```


to force pull adn overwrite local changes, there is no `git pull --force`. we have to use a combination of `git fetch origin` and `git reset --hard origin/main`
