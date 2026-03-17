the wonderful git stash 
to merge the local and remote version, you can git stash, pull, then reapply. the commands would be like this: 
```shell
git stash push -u -m "..."
git pull
git stash pop
```

to read more: git amend, git rebase -i
