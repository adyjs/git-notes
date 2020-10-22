# git notes


## git structure

![git structure](https://miro.medium.com/max/686/1*diRLm1S5hkVoh5qeArND0Q.png)
(ref:https://medium.com/@lucasmaurer/git-gud-the-working-tree-staging-area-and-local-repo-a1f0f4822018)


1) when files added into a project folder, git will notice that,
and also call those files *untracked file*

by using "$ git add" put files into "staging area"

2) staging area is a place for you to decide which you want to 
make a snapshot of code and how you want to arranged to become a 
commit node

by using "$ git commit" put files into repository

3) snapshot of files all in ".git" folder, 
so no matter what you want to do after, 
git will compare those new files or new changes to the file in .git



# Clean / Restore / Rm

## clean

* meaning  


remove untracked files from the working tree


1) clear files that ignored by .gitignore
```
$ git clean -fX
```

## restore

* meaning  

restore the changes has made in working directory and staging area,
pretty like using $ git checkout for restore files.


1) restore tracked file you delete
```
$ git restore FILE_DELETED

```

## git rm

* meaning  


actually, remove file by "$ git rm" or by "$ rm" in BASH, 
that would not be quite different,
those file actually be deleted, 

the only different is, if you use
```
$ git rm 
```
then, you don't need to add "git change" into stage,
git add file automatically for you, all you need to do is commit the changes.


* clear cache

1) clear git cache for untrack something

if you don't want to really delete something from git,
just don't want git keep track something, 
then, clear the git cache of the file

```
$ git rm FILE_DONT_WANT_TRACKED --cache

```

now the trached cache of this file will be cleared,
therefor, this file in git will be the untracked file. 



# Log / Reflog / Blame

## log

* meaning  

check log of commits , including
commit related,
author,
date,
commit messages

1) limited / all log
```
$ git log [--all] [--oneline] [--graph] [-p] [--author="NAME"]

-p : display difference of each commits

```

2) log with specific file
```
$ git log file
```

3) log


## reflog

* meaning  

records about each operation related to commits, checkout, branches  

```
3436d2a (HEAD -> master) HEAD@{0}: commit: remove test.file
2b0109d HEAD@{1}: commit: add test.file
3a18267 HEAD@{2}: commit: add gitignore file
```
we could noticed that there is commit hash and HEAD@{index} in each line,

it is better by using HEAD@{index} for doing reset related opertion.


## blame

* meaning  

this will helps to find out the each line of codes which who had  wrote

```
$ git blame FILE


commit   author name  date                      line
nodes                                           number

182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800  1)
2a6319dd (AUTHOR_NAME 2020-10-09 05:22:25 +0800  2)
2a6319dd (AUTHOR_NAME 2020-10-09 05:22:25 +0800  3) 
2a6319dd (AUTHOR_NAME 2020-10-09 05:22:25 +0800  4)
2a6319dd (AUTHOR_NAME 2020-10-09 05:22:25 +0800  5)
2a6319dd (AUTHOR_NAME 2020-10-09 05:22:25 +0800  6) 
182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800  7) 
182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800  8)
182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800  9)
182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800 10) 
182d1ab0 (AUTHOR_NAME 2020-10-09 05:17:18 +0800 11) 
25f35224 (AUTHOR_NAME 2020-10-09 05:35:45 +0800 12)
25f35224 (AUTHOR_NAME 2020-10-09 05:35:45 +0800 13)
25f35224 (AUTHOR_NAME 2020-10-09 05:35:45 +0800 14) 

```



# Branch / Checkout

## branch

* meaning   

branch means there is a move-able tag flows along to the commits, 

actually, branch and commits node are independent respectively,

so, we could merge commits, or move tag onto other commits nodes.

1) generate a branch
```
$ git branch BRANCH_NAME [NODE_COMMIT_HASH]

```
if add a node commit hash after BRANCH_NAME,
then means you set a branch name tag on a commit node


2) rename a branch
```
$ git branch -m OLD_BRANCH_NAME NEW_BRANCH_NAME
```

3) force a branch tag moves to other commits
```
$ branch -f BRANCH_NAME COMMIT_NODE
```
like said before, branch name just like a sticker,
we use it for give the meaning for commit nodes,
they are independently between branch and commit node,
so we could move the branch sticker to anywhere we want,
it doesn't effect to the commit node.


4) delete a branch
```
$ git branch -d [-D] BRANCH_NAME
```
upcase -D means delete branch by force

same as above, the branch we deleted before merged,
we just delete the branch tag sticker, doesn't delete commit nodes


## checkout

* meaning  

checkout means change the state of working directory
actually, it is moving the HEAD pointer to points nodes of working tree,

each commit node is a state of codebase, also called snapshot,
so no matter change HEAD pointer to a branch tag or just a commit node, git will change the working directory state by commit node with the files in .git repository.


* checkout command usually used to :

1) move HEAD pointer between branches
```
$ git checkout BRANCH_NAME
```

2) detach HEAD to other commit nodes
```
$ git checkout COMMIT_HASH
```

3) checkout files from staging area (if doesn't add changes file from working directory, then staging area is as same as repository (.git), this usually used in situation of restoring something)
```
$ git checkout -- FILE

```
double dash means we don't want to switch branch,  
we just want to checkout FILE for restore, 



# Merge / Rebase / Squash / Cherry-Pick

## merge

* meaning  

merge means 2 branches were got some different, so they are in different flow of branch, pretty easy to understand,

if we want to combine these 2 branches, then need to merge

if A branch wants to merge B branch
then, needs to checkout to the branch A first

```
$ git merge B

```
then A are as same as B

* A merge B / B merge A , difference?

1) B base on A, A merge B

it will be Fast-Forward mode , A as same as B

```
$ git merge B [--no-ff]

```
--no-ff : means do not want to use fast-forward,  

so you can see the 2 branch merge in visual graph.



2) A base on C, B also base on C

it got some little complicated,
because A and B are different, so during merge it may got some conflict and will generate a new commit node.

the newest commit node is the combines of A and B,
no matter who merge who, the newest commit node always be the same.
the difference of who merge who is that 
which tag still flow with branch

if A merge B, then A will keep move on along with flow,
but B just stay on his old position.





## rebase

* meaning    


why use
```
branches merge will be very clean and simple
```

why not to use
```
rebase will not generate extra commit node, 
also modify records, modify branch visulization, 
that may be bringing some issues for self and other people.
```

```
A1 -> A2 -> A3
B1 -> B2

on branch A,
$ git rebase B

means branch A change base, 
to make B as new base of A.

B1 -> B2 -> A1' -> A2' -> A3'
```

### how to cancel rebase

* if rebase on progress
```
$ git rebase --abort

```


* if rebase done

1. reset method 1

```

if we do something dangerous operations, git will generate a variable called ORIG_HEAD, so we could do reset with this.

$ git reset ORIG_HEAD --hard

```

2. reset method 2
```
$ git reflog

check reflog, get the last HEAD position before on rebase start

```

* interactive rebase

pick commit nodes you want.

you could custom branch flows by interactive rebase,

you could ignore, pick or edit message on old commit nodes.

```
$ rebase -i
```


## squash

* meaning

combine commit nodes into 1 node for cleaner visual

[squash](https://gitbook.tw/chapters/rewrite-history/merge-multiple-commits-to-one-commit.html)


## cherry-picl


* cherry-pick
    * 


# Ref / Reset / Revert

* meaning 

ref is not a command in git,
it just a variable to relate with other things.

if HEAD means current commit node, then

1. HEAD~2
2. HEAD~~
3. HEAD^^
    * 2 commits older than HEAD
    
4. HEAD^2
    * if HEAD was merge, HEAD^1 is closer parent, HEAD^2 is farer parent.

5. HEAD@{2}
    * the last 2 operate in reflog



## reset

* meaning  

change HEAD pointer to backward commits node for re-doing something

```
$ git reset COMMIT_HASH / HEAD@{} / HEAD(ref) [--mixed] [--soft] [--hard]

```
* --mixed (default) : 
reset --mixed will remove the file changes in staging area,   
but file changes will still keep in work directory, shows like [modified]  

* --soft :
no changes in work directory and staging area,
pretty like just detach HEAD pointer

* --hard :
all changes will be removed in work directory and staging area,
so, in log we can not see the newest commits record that we commit before.


we all need to know one thing that no matter opinion with reset,  
the commits node, will not be really remove in .git,  
all the different is that visual or not.

so, this means no matter the setting of opinion,
if we doing some mistake with reset, we all could be retrieve commit node back.


## revert















* tag

