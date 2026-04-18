---
title: Untitled 
tags: []
draft: true
date: 2026-04-07
---


i realized that i don't know enough about version control 

i still don't know the difference between a normal branch and a `remotes/branch`


`git push origin` --> explicitly names the remote `origin` but relies on Git to tell which branch to push to --> usually matching the remote branch name. 

`git push origin branch-name` --> tell Git exactly where to push (`origin` the remote), what to push `branch-name`

The `-u` flag sets up a tracking relationship --> from then, `git push` knows where to send it --> without `-u` on the first push bare `git push` won't know what to do 


This could get tricky and the Git tree could get messy very quickly. 


|                           | remote   | branch   | requires tracking setup |
| ------------------------- | -------- | -------- | ----------------------- |
| git push                  | inferred | inferred | yes                     |
| git push origin           | explicit | inferred | partially               |
| git push origin my-branch | explicit | explicit | no                      |

check if your local branch has commits that the remote doesn't
`git log origin/main..HEAD` --> this checks if local branch has commits that the remote doesn't. 
--> if this shows nothing, branch has no new commits compared to `main` nothing to PR


`git status` --> check what's staged/unstaged

`git log --oneline --graph origin/main branch-name`



![[Pasted image 20260407210406.png]]



`git branch -d <branch_name>`

delete remote `git push <remote_name> --delete <branch_name>`


delete branch remotely != delete branch locally 

