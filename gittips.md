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

