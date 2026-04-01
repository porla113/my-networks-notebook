# Git
* [Setup](#setup)
* [General](#general)
* [Remote](#remote)
* [Branching](#branching)
* [Remove](#remove)
* [Caution](#caution)
## Setup

|Command|Description|
|---|---|
|`git config --list`|List config.|
|`git config --global user.name "John Doe"`|Set user name.|
|`git config --global user.email "johndoe@example.com"`|Set email, must match with Github account.|
|`git config --global init.defaultBranch main`|Set the default branch to main.|
|`git init`|Initialize a reository.|
## General

|Command|Description|
|---|---|
|`git config --version`|(-v) Show version.|
|`git config --help`|(-h) Show help.|
|`git config --list`|(-l) List configuration.|
## Commit

|Command|Description|
|---|---|
|`git status`|Check the status of files.|
|`git add *.txt`|Track/add files to stage.|
|`git commit -m "message"`|Commit with message.|
|`git commit --amend -m "message"`|Add changes to the latest commit with a new message / Change commit message (local only).|
|`git commit --amend --no-edit`|Add changes to the latest commit and keep message (local only).|
## Remote

|Command|Description|
|---|---|
|`git push -u origin main`|Push a local branch to the remote (first time).|
|`git push`|Push a local branch to the remote.|
|`git pull`|To fetch changes a remote branch and merge into local branch.|
|`git fetch`|To download changes from a remote branch without merging.|
## Branching

|Command|Description|
|---|---|
|`git branch`|List all local branches.|
|`git branch <new-branch>`|Create a new branch.|
|`git checkout <branch-name>`|Switch working dir to the specified branch.|
|`git switch <branch-name>`|New way of branch switching.|
|`git branch -m <new-branch-name>`|Rename the current branch.|
|`git branch -d <branch-name>`|Delete a local branch (safe delete).|
|`git branch -D <branch-name>`|Force delete a local branch.|
## Remove

|Command|Description|
|---|---|
|`git rm <file-name>`|Remove a file.|
|`git rm -r <directory-name>`|Remove a directory and files inside.|
## Caution

|Command|Description|
|---|---|
|`git reset --hard HEAD`|Revert to the latest commit.|
|`git clean -ndf`|List untracked files and directories to be remove.|
|`git clean -df`|Force to remove untracked files and directories.|