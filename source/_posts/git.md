---
title: Git Notes
date: 2023-12-21 20:35:19
tags:
cover_image_alt:
---

**Note**: {} means doesn't need ""

## Basic operation

`git init`: to initialize a local folder into git repo

`git status`: check the status of the current branch of a git repo, e.g. how many files are not added to the index, how many files are ready to be committed.

`git add {file name}` or `git add .`: to add a specific file or all file to index

git diff (1. no file name? 2. multiple files?)


## configuration
git config --global user.name ""
git config --global user.email ""
git config --global core.editor nvim
git config credential.helper store (then git push) # store the password

git commit -m "description" (why -m: message)

create .gitignore

git rm --cached file name to untrack the file

## branch
git branch -l(for local) {branch name} # master is main
git branch -r(for remote) {branch name} 
git branch -a(for all) {branch name} 

git checkout {branch name}

git merge {branch name}

git branch # to check how many branches you have

git branch -d {branch name}

git branch -D {branch name} # force to delete

## cooperation
    
create new repo in github

git remote add origin {link to you remote repo}

git push --set-upstream origin {branch name}

git pull


## conflict
