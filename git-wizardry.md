## PATCHES

### Create a patch from a diff

git diff master..branch-name > path/to/file.patch 

or

git diff master..HEAD > path/to/file.patch


## BRANCH WRANGLING

### Rename a branch

**on different branch**
git branch -m old-name new-name

**on same branch**
git branch -m new-name 


### Delete branch

**locally**
git branch -d branch-name    # safe delete, only if merged
git branch -D branch-name    # force delete, even if unmerged

**on remote**
git push origin --delete branch-name
