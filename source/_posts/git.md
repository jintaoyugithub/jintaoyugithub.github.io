---
title: Git Notes
date: 2023-12-21 20:35:19
tags:
cover_image: /imgs/coverImgs/git.jpg
---

**Note**: {} means doesn't need ""

## Things you should know

**.git**: .git folder store all the changes of the repo.

**different space of git**:

- **workspace**: the current branch which we're eidting.

- **stage/index**: store the status of out current workspace, which files are changes which aren't.

- **repository**: local repo, store all the branches and files.

- **remote**: remote repo like github.

The relation between them is shown below:

![relation between these four space](/imgs/gitNotes/img1.jpg)

**Authorization**: there are **two** ways you can authorize the access of the remote github repo to you local computer
- **ssh keys**(old): seems like github has canceled the ssh option.
- **personal acess tokens**(new)

## Basic operation

**git init**: to initialize a local folder into a git repo

**git status**: check the status of the current branch of a git repo, e.g. how many files are not added to the stage, how many files are ready to be committed.

**git add {file name}** or **git add .**: to add a specific file or all file to stage.

**git commit -m "message you wanna add"**: try to commit the changes with a message.

**git remote add origin {link to your repo}**: add the github repo addr as the origin in order to push new changes to the remote repo.

**git remote -v**: to check any remote repo that I've connoted to this local repo.

**git push -u origin master**: push the local repo to remote repo.

- -u means **set-upstream**, it makes the current remote repo as default therefore you can use **git push** for short.

- **origin** is the addr of the remote repo, which means origin is, for example, https://github.com/jintaoyugithub/jintaoyugithub.github.io.git

**git log**: show the commit history.


## Configuration
**add user name**: git config --global user.name ""

**add user email**: git config --global user.email ""

git config --global core.editor nvim

git config credential.helper store (then git push) # store the password

**untrack some files as default**: you need to create a file call **.gitignore** and then use the command **git rm --cached {file name}**: to untrack the file.


## Branch

**git branch**: chech the status of the branches, e.g. how many branches you have, which branch you're current in.

**git branch {new branch name}**: create a new branch

extra commands:
- -l {branch name}: for local branch
- -r {branch name}: for remote branch
- -a: for all branches
- -d {branch name}: delete the branch
- -D {branch name}: force delete

**git checkout {branch name}**: to switch to the branch we want.


## Merge

**git diff**: compare what's different between this time and last time in the same branch.

**git diff {branch name}**: to see what's different from current branch to the branch that we specified.

**git merge {branch name}**: to merge the specified branch to the current branch.

**Note**: a more common way to merge two branches is to push the code to the seprate branch individually and make a PR in github.

## Conflict

You better to have a nice code editor to see the confilcts.

## Undo

**git reset**: to undo the **add** action, which means get the files back from you add them to the stage last time.

**git reset HEAD~{number}**: reset the current branch **number** steps back from the current **HEAD** pointer.

**git reset {hash of that commit}**: go back to a specifc commit.

**git reset --hard HEAD/{hash of the commit}**: reset the branch pointer to the specified commit and forcefully discard all the local changes and modifications you've done in the current workspace.

