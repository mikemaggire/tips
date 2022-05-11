Some Tips to work with Git

# Git Basics

## Git Clone

```bash
# go to the folder that must contain the cloned repo
cd ~/repos
# clone via ssh from a private serer
git clone ssh://{username}@{gitserverip:port}/{gitpath}/{reponame}
# clone via ssh from github
git clone git@github.com:/{hithub_username}/{reponame}.git
# go to cloned repo
cd {reponame}
```

SSH will look for the user's ~/.ssh/config file. 

```
Host github.com
Hostname ssh.github.com
Port 443
IdentityFile ~/.ssh/id_rsa.github
IdentitiesOnly yes
```

Don't forget to add your SSH key to the ssh agent

```
ssh-add ~/.ssh/id_rsa.github
```

After cloning, you may need to change the url 

```bash
git remote set-url origin git@github.com:{gitusername}/{yourrepo}.git
```

## Git user config

```bash
# list config inthe current path
git config --list
# list global config on this machine
git config --global --list
# setup local names
# I recommend not to 
git config user.name "{mygit_username}"
git config user.email "{mygit_useremail}"
```
## Git log

```bash
# display git history in a pretty format
git log --pretty=format:"%h - %an, %ar : %s"
```

## Git tags

```bash
# list local tags
git tag
# create a local tag with the last local commit (copy all sources)
git tag {vx.y.z}
# push the tag to the remote
git push origin {vx.y.z}

```

## Squash

Squash groups all selected commits into a single one.

```bash
git rebase -i {sha256}
# force push to the **right branch**
git push origin master --force
# then clean branch
git branch -d {mybranchname}
```

- [git ds la pratique](https://blog.octo.com/git-dans-la-pratique-22/)
- [merging vs rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [comment-squasher-efficacement-ses-commits-avec-git](https://www.ekino.com/comment-squasher-efficacement-ses-commits-avec-git/)
- [keeping parts of your codebase private on github](https://24ways.org/2013/keeping-parts-of-your-codebase-private-on-github/)

# Git Advanced

## Update .gitignore

he files/folder in your version control will not just delete themselves just because you added them to the .gitignore. They are already in the repository and you have to remove them. You can just do that with this:

:warning: Remember to commit everything you've changed before you do this!

```
git rm -rf --cached .
git add .
```

This removes all files from the repository and adds them back (this time respecting the rules in your .gitignore).

## Git Tags

Delete all local tags (windows powershell syntax)

```bash
# delete all local tags (windows powershell syntax)
git tag | foreach-object -process { git tag -d $_ }
# delete non required tags directy on server and fetch repo before fetch tags
git fetch
git fetch --prune --tags
# delete tags on remote
git push --delete origin tagid


## Clone a single remote branch

Could be usefull to rename the local path created with the name of the branch. This way you can work on two diffreents local group of source without any risk of error/conflict when commit/push

```bash
git clone -b branchname --single-branch https://github.com/userid/repoid
```

## Merge a single commit from one brach to another

:link: [git merge commit from another branch] https://w3guy.com/git-merge-commit-from-another-branch/

merge a commit in branch B with SHA-1 checksum of 0afc917e754e03 to branch A :

```bash
# If you are not already in branchA, checkout to the branch (git checkout branchA)
git cherry-pick 0afc917e754e03
# If there is any conflict; fix it, stage the changes and commit
```

## Create a new repo from a clone

The issue: when you clone a repo to a local folder, it's still pointing to the original (let say `origin`) remote repo. If you want to use this cloned repo as a basis for a new repo, then you need remove reference to the remote and rebuild a new one.

**1. Remove the reference to the GitHub repo**

```bash
git remote remove origin
# ⚠️ FETCH_HEAD still pointing to github
rm .git/FETCH_HEAD
```

**2. Assign a new remote repo to the local git folder (eg. cloned)**

```shell
git remote add origin https://github.com/yourgitaccount/newrepo
```

> Do not use `git remote set-url` as it sets the url of an existing origin.

**3. Create and empty repo on github**

We can't create a repo on github from the command line (unless via API and so with a specific client tool).

Basically, go to http://github.com/yourgitaccount and then create an empty repo.

**4. 1st push**

```shell
git push -u origin master
```

Parameter `-u` stand for `--set-upstream` to create a tracking with the remote repo.

# Git Config

## Git Editor

```bash
git config --global core.editor notepad
# or
git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

# Git Bare

### List repos

```bash
# ssh connect to the remote server 
# go to git repos
cd ~/gitrepos
# list all repos
ls -a 
# go to a specific repo 
cd ~/gitrepos/myrepo.git
# list all files on the last commit/pushed version
git ls-tree HEAD
```

### Create a new bare repo

```bash
# ssh connect to the remote server 
# go to git repos
cd ~/gitrepos
# Init the new repo
git init --bare {mynewrepo.git}
# show its content
git ls-tree --full-tree -r HEAD
# check what your repository's HEAD is currently referencing
git symbolic-ref HEAD
# update your repository's HEAD to a valid ref
git symbolic-ref HEAD refs/heads/{master}
```
