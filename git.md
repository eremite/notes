# Notes on git

## Diff of the latest pull

```bash
git diff @{1}..
```

## Throw out all of the changes you’ve been working on

```bash
git checkout -f
```

## Remove everything git doesn't know about.

```bash
git clean -f
```

## Undo a commit

```bash
git reset --soft HEAD^
```

## Shows how branches will be pushed

```bash
git remote show origin
```

## Making and using a patch

```bash
git diff HEAD~1.. > ~/patch
patch -p1 < ~/patch
```

## Releasing a new version

```bash
git tag -l
git log --no-merges --abbrev-commit --pretty=oneline $(git tag -l | tail -n 1).. > release.txt
# edit release.txt
git tag -a v2.0 -F release.txt
git tag -a v$(date +"%Y.%m.%d") -F release.txt
git push --tags
```

## What is new since stable?

```bash
gco master
git log --no-merges --abbrev-commit --pretty=format:"* %s" stable.. | awk '{gsub(/refs/,"see");print}'
```

## Git Submodules: What is the Ideal Workflow?

```bash
http://blog.endpoint.com/2010/04/git-submodule-workflow.html
```

## Add git submodule

```bash
git submodule add git://github.com/user/submodule.git extension
git submodule init
```

## Remove git submodule

```
http://stackoverflow.com/questions/1260748/how-do-i-remove-a-git-submodule
* Delete the relevant line from the .gitmodules file.
* Delete the relevant section from .git/config.
* Run git rm --cached path_to_submodule (no trailing slash).
* Commit and delete the now untracked submodule files.
```
    
## Install submodule on a new super project

```bash
git submodule init
git submodule update
```

## New Repo Walkthrough

```bash
ssh git.custombit.com
cd /var/repos
APP=myapp
mkdir $APP.git
cd !$
git --bare init
echo "My App Name" > description
ln -s /var/repos/post-receive-hook hooks/post-receive
find objects -type d -exec chmod 02770 {} \;
cd ..
sudo chown -R :staff $APP.git
sudo chmod -R g+s $APP.git

# In the directory of the code
git init
git add .
git commit -m "Initial commit"
git remote add origin ssh://git.custombit.com/var/repos/${PWD##*/}.git
git config --add branch.master.remote origin
git config --add branch.master.merge refs/heads/master
git push origin master
```

## Checkout out a previous revision (undo a git pull)

```bash
git checkout @{"1 hour ago"}
```

## Rename a Branch

```bash
git branch -m old_branch new_branch
```

## Set default branch

```bash
git config --add branch.master.remote origin
git config --add branch.master.merge refs/heads/master
```

## Upgrading all submodules

```bash
git submodule foreach git pull origin master
```

## Save a packaged archive

```bash
git archive -o ~/output_file.zip master
```

## Delete a remote branch

```bash
# http://gitready.com/beginner/2009/02/02/push-and-delete-branches.html
git push origin :deleted_branch
```

## Show the history of a file

```bash
# http://www.readysetrails.com/index.php/2111/5-mistakes-that-make-you-look-like-a-rails-n00b/
git whatchanged -p --abbrev-commit --pretty=medium
```

## Show a diff of a stash

```bash
git stash show -p
```

## Include diff when writing commit message

```bash
git commit -v
```

## Pull Requests

```bash
git checkout -b new_feature
#make your commits
git push -u origin new_feature

# To get someone else's remote branch
git fetch origin
git checkout --track origin/another_feature
```

Log in to github and compare the branches and create a pull request.

## Set default heroku app

```bash
git config heroku.remote staging
```

## Checkout a remote branch

```bash
git checkout --track remote_name/remote_branch_name -b local_branch_name
```
