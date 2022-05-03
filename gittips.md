# Change Git Editor

```bash
git config --global core.editor notepad
# or
git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

# Git log

```bash
# display git history in a pretty format
git log --pretty=format:"%h - %an, %ar : %s"
```

# Git tags

```bash
# list local tags
git tag

# delete all local tags (windows powershell syntax)
git tag | foreach-object -process { git tag -d $_ }
# delete non required tags directy on server and fetch repo before fetch tags
git fetch
git fetch --prune --tags
# delete tags on remote
git push --delete origin tagid
```

# Git clone a single remote branch

Could be usefull to rename the local path created with the name of the branch. This way you can work on two diffreents local group of source without any risk of error/conflict when commit/push

```bash
git clone -b branchname --single-branch https://github.com/userid/repoid
```

# Git merge a single commit from one brach to another

:link: [git merge commit from another branch] https://w3guy.com/git-merge-commit-from-another-branch/

merge a commit in branch B with SHA-1 checksum of 0afc917e754e03 to branch A :

```bash
# If you are not already in branchA, checkout to the branch (git checkout branchA)
git cherry-pick 0afc917e754e03
# If there is any conflict; fix it, stage the changes and commit
```

# Git Squash 

Squash groups all selected commits into a single one

:link: [git ds la pratique](https://blog.octo.com/git-dans-la-pratique-22/)

:link: [merging vs rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

:link: [comment-squasher-efficacement-ses-commits-avec-git](https://www.ekino.com/comment-squasher-efficacement-ses-commits-avec-git/)

```bash
git rebase -i sha256
# force push to the **right branch**
git push origin master --force
# then clean branch
git branch -d mybranchname
```

- [keeping parts of your codebase private on github](https://24ways.org/2013/keeping-parts-of-your-codebase-private-on-github/)

# Git create a new repo from a clone
## The issue
When you clone a repo to a local folder, it's still pointing to the original (let say `origin`) remote repo. If you want to use this cloned repo as a basis for a new repo, then you need to 

> You may want to fork it. But fork is not allowed on your own repos. Furthermore a fork is also linked to the original repo, and you might not want to.

### Remove the reference to the GitHub repo

```shell
git remote remove origin
```

Will delete the config settings from `.git/config`

⚠️ FETCH_HEAD still pointing to github. Need to remove it with `rm .git/FETCH_HEAD`

### Assign a new remote repo to the local git folder (eg. cloned)

```shell
git remote add origin https://github.com/yourgitaccount/newrepo
```

> Do not use `git remote set-url` as it sets the url of an existing origin.

### create and empty repo on github
We can not create a repo on github from the command line (unless via API and so with a specific client tool).

Basically, go to http://github.com/yourgitaccount and the create an empty repo.

### 1st push

```shell
git push -u origin master
```

Parameter `-u` stand for `--set-upstream` to create a tracking with the remote repo.

### Update .gitignore

he files/folder in your version control will not just delete themselves just because you added them to the .gitignore. They are already in the repository and you have to remove them. You can just do that with this:

:warning: Remember to commit everything you've changed before you do this!

```
git rm -rf --cached .
git add .
```

This removes all files from the repository and adds them back (this time respecting the rules in your .gitignore).

### _references_

* https://stackoverflow.com/questions/16330404/how-to-remove-remote-origin-from-git-repo
* [basics of git explaines with scheme](https://marklodato.github.io/visual-git-guide/index-fr.html)

