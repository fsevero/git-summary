# Git Quick Commands Listing (From [Peter Etelej](https://github.com/etelej))
[Source](https://etelej.github.io/git-commands-quick-dirty-git-shortcuts/)

## Git Configuration
```shell
git config --global user.name "Your Name"
#use github email if you plan to use with github
git config --global user.email you@youremaildomain.com

#adding colors
git config --global color.diff auto
git config --global color.status auto
git config --global color.branch auto
```

## Initialize Git
```shell
git init

#Git Clone
git clone git://github.com/USERNAME/REPOSITORYNAME.git

#Add all files (new, modified,deletions) to index (stage)
git add -A # perfoms git add .; git add -u
#Add files to staging area
git add filename.txt

#List tracked files
git ls-files;

#list all untracked files
git ls-files --others  # add --directory for directories

#Unstage a file but preserve it's contents
git reset filename

#Remove files from index, stage it's deletion from HEAD
git rm filename

#Quick Commit - add all modified files and commit
git commit -am "The commit message here"

#Status of files in index vs Working directory
git status
```

To ignore files in repository directory: Add to file or directory to `.gitignore` file (each file/directory on a new line)

## Resetting and Reverting Repo

Revert to previous commit (graceful - does not delete history)

```shell
git revert HEAD
# git revert [SHA-1]
```

Reset to previous state: **Do not use if already commited to shared remote**
```shell
git reset --hard HEAD
# git reset --hard [SHA-1] # Reset completely delete history and staged
git reset --soft HEAD
# git reset --soft [SHA-1] # does not touch staging area or working tree
```

## Git Branches
```shell
#Create local branch
git branch branchname

#Move/switch to a branch
git checkout branchname

#Create and switch to branch
git checkout -b branchname

#Fetch file from another branch
git checkout branchtwo -- path/to/file

#push to a new remote branch, e.g. if remote name is origin
git push -u origin branchname

#Delete Local Branch
git branch -D branchname

#Delete remote branch
git branch -D -r origin/branchname
git push origin :branchname

#Checkout branches with latest commits #esp for cleaning old branches
git for-each-ref --sort=-committerdate --format='%(refname:short) %(committerdate:short)'
```

## Git merge
Before merging, checkout to branch you want to merge to:

```shell
git checkout master

#Merge local branch
git merge branchname

#Always generate merge commit even on fast-forward
git merge --no-ff branchname

#List branches that have been merged to current branch (e.g master)
git branch --merged

git branch --no-merged  #list branches that haven't been merged to current

#Delete merged branches
git branch --merged | xargs git branch -d
```

Merge remote branch

```shell
#Update the remote
git fetch origin
#merge remote branch
git merge origin/branchname

#SHORTCUT - Fetch and merge tracked branch
git pull
```

## Working with Remotes
**Adding remote** - Connecting local repo with a remote repo (for example a repo on github)

```shell
git remote add origin https://github.com/USERNAME/REPOSITORY.git
#git remote add [name] [repourl]

#Verify remotes - List existing remotes
git remote -v

#List all remote branches
git branch -r

#Remove remote
git remote remove origin

#Replace remote url
git remote set-url origin https://github.com/USERNAME/REPOSITORY2.git

#To check out commits on an upstream master
git log --oneline master..origin/master

#Merge remote branch eg upstream master into current branch
git merge origin/master

#Rebase (fetch remote branch and merge)
git pull --rebase remote

#Fetch all remotes
git fetch --all #git fetch [remotename] #to fetch single remote

#push to a new remote branch
git push -u origin branchname
```

## Stashing
**Stash** - Store modified tracked files and staged changes on a stach for reapplying later

```shell
#push stash onto stack
git stash

#Store stash with message
git stash save 'A custom message'

#List stored stashes
git stash list

#Reapply most recent stash
git stash apply
#for older stashes pick from list e.g git stash apply stash@{1}

#Remove stash from stack
git stash drop stash@{0}
#drops stash reference or if no parameter drops latest stash

#Clear all stashes
git stash clear

#Apply latest stash and remove from stack
git stash pop
```

## Useful Commands
**History** - Checkout the history of your commits

```shell
git log --oneline --graph --decorate --all --color

#Filter logs
git log --author=author
git log --after="MMM DD YYYY"
git log --before="MMM DD YYYY"
git log --grep REGEXP  #Commits with matches to regular expression

#list history of file
git log --follow filename

#Show changes to file
git whatchanged file

#Show author of each line in file
git blame file

#Search through repo history
git rev-list --all | xargs git grep -F 'searchstring'
#git rev-list --all | xargs git grep 'REGEX' #to search for regular expression
```

**Diff** - Compare states

```shell
#Compare two commits
git diff master..upstream/branchname
#git diff HEAD~2..HEAD~1

#Compare staged changes to your last commit
git diff --cached

#Compare working directory to your last commit
git diff HEAD
```

**Tagging**

```shell
#annotated tag
git tag -a v1.5 -m 'Message Here'

#lightweight tag
git tag v1.5.2
#to create lightweight tag; don’t supply the -a, -s, or -m option

#list tags
git tag

#show tag and related commit
git show v1.2

#Tag older commit
git tag -a v2 9fceb02 -m "Message here"
# git tag -a v1.2 [SHA-1] -m "[your message]"


#Rename tag
git tag newtag oldtag
git tag -d oldtag
git push origin :refs/tags/oldtag
git push --tags

#Adding a message to an existing tag
git tag tagname tagname -f -m "the message"
    # creates a tag of the same name with a message and overwrites old tag

#Push referenced tags along with branches
git push --follow-tags
```

**Cleaning**

```shell
#Perform clean dry-run - show files to delete
git clean -n

#Remove all untracked files from working copy
git clean -f

#Remove all untracked and ignored files and directories from working copy
git clean -fxd

#Cleanup unnecessary files and optimize the local repository
git gc #calls git prune with prunes loose objects older than 2wks

git gc --prune=all #prunes all loose objects
```

## Useful Git Files
* .gitignore
* .gitattributes
* .mailmap
* .gitmodules
* *.keep

## Guides and Tutorials on Git

[Git - the simple guide - no deep shit]()

[Official Git Documentation](http://git-scm.com/doc)

[Github's Git Training: Try Git](https://try.github.io/levels/1/challenges/1)

[Git Tutorials by bitbucket](https://github.com/fsevero/git-summary/edit/master/README.md)
