# Git Basics

## Whenever you make changes, you will need to go through these 3 commands:

`git add *`

`git commit -m 'Brief description of what you're adding to the repository'`

`git push -u origin master`

## Setup
`git config --global user.email "sampleemail@email.com"`

`git config --global user.name "yourname"`
* These are both required to make commits, and are tied to the commits you make
* Can be overridden by running the command while in the project, without the `--global` 
* You can check settings with:

`git config --list`

## Initialize Project Repository
* While in existing project folder:

`git init`

## Pulling An Existing Repo To Local Directory

`git pull fakeurl.org/path/to/repo.git`
* Brings repo to your location in directory

## Adding Existing Files
`git add *`
* The `*` tells it to add anything in the current directory: you can also specify a certain filetype or single file or whatever (`filepath/\*.md`, or `thisfile.py`)

`git add LICENSE`
* Good software deserves a license

`git commit -m 'Brief description of what you're adding to the repository'`
* After these commands you will have made a commit, we still need to push the commits to our master

## Pushing Commits

`git remote add origin giturlexample.org/path/to/repo.git`
* Maps the remote repo to our current local repo
* You may need to enter credentials

`git remote -v`
* Verifies the link

`git push -u origin master`
* The final push

