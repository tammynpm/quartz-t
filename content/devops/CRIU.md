checkpoint and restore 
# checkpoint
* /proc file system (general place where criu takes all the information it needs) include
	* files descriptors info
	* pipes parameteres
	* memory maps
## checkpoint stage: 
the process dumper (aka dumper) 
- collect process tree and freeze it
- collect tasks' resources and dump them 
- cleanup 


# restore
restore procedure (aka restorer): morph itself into the task it restores with 4 steps:
1. resolve shared resources
2. fork the process tree
3. restore basic tasks resources
4. switch to restorer context, restore the rest and continue