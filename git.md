# Notes on git

## Diff of the latest pull

    git diff @{1}..

## Throw out all of the changes you’ve been working on

    git checkout -f

## Remove everything git doesn't know about.

    git clean -f

## Undo a commit

    git reset --soft HEAD^

## Shows how branches will be pushed

    git remote show origin

## Making and using a patch

    git diff HEAD~1.. > ~/patch
    patch -p1 < ~/patch

## Releasing a new version

    git tag -l
    git log --no-merges --abbrev-commit --pretty=oneline v1.6.. > release.txt
    v release.txt # AMAPS Software v1.6 Release Notes
    git tag -a v1.7.27 -F release.txt
    git push --tags

## What is new since stable?

    gco master
    git log --no-merges --abbrev-commit --pretty=format:"* %s" stable.. | awk '{gsub(/refs/,"see");print}'

## Git Submodules: What is the Ideal Workflow?

    http://blog.endpoint.com/2010/04/git-submodule-workflow.html

## Add git submodule

    git submodule add git://github.com/user/submodule.git extension
    git submodule init

## Remove git submodule

    http://stackoverflow.com/questions/1260748/how-do-i-remove-a-git-submodule
    * Delete the relevant line from the .gitmodules file.
    * Delete the relevant section from .git/config.
    * Run git rm --cached path_to_submodule (no trailing slash).
    * Commit and delete the now untracked submodule files.

    
## Install submodule on a new super project

    git submodule init
    git submodule update

## New Repo Walkthrough

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

## Checkout out a previous revision (undo a git pull)

    git checkout @{"1 hour ago"}

## Rename a Branch

    git branch -m old_branch new_branch

## Set default branch

    git config --add branch.master.remote origin
    git config --add branch.master.merge refs/heads/master

## Upgrading all submodules

    git submodule foreach git pull origin master

## Save a packaged archive

    git archive -o ~/output_file.zip master