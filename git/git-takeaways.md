# Git course

## Section 1

Git stores key value pairs: hashes and the content of each document

#### Install git on Mac OS:

1. Install brew:

2. ````bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ````

3. Add Homebre to my PATH:

4. ````bash
   echo 'eval $(/opt/homebrew/bin/brew shellenv)' >> /Users/estebanmunchjones/.zprofile
   ````

5. 

6. Install git:

7. ````
   brew install git
   ````

8. Other topic, Extra: to check a version of an installed npm package (not this git with brew):

9. ````bash
   bnpm view <package-name> version
   ````

10. Back to home brew. To check the git version installed:

11. ````bash
    git version
    ````

12. 

#### Terminal vs Shell

terminal is a GUI, and the shell is the name on top of the terminal, in my case is `szh`. The most common is `bash`.

Change the theme: terminal -> preferences -> Pro -> click default

Similar for font-size.



#### Shell commands

-Edit documents: `nano` opens an integrated editor inside the terminal.

-see each command manual: 

````bash
man <command>
````

-Autocompletion with `tab`,

```bash
Instead of:
cd Applications

use tab
cd App + tab
```



#### MacOs command line:

1. How to open the **root directory** on finder? Go -> Home or:

````
cmd + shift + H
````

this directory has `Users` folder, and then `estebanmunchjones` folder, with my docs and so on. This is my **home directory**.

When opening the terminal, the `~` sign tells us we're at the root level of the directory specified in the terminal:

````
estebanmunchjones@Estebans-MacBook-Air ~ %
````

by, default, the terminal opens at the `home` directory.

if we print the directory, with `pwd` it yields:

````bash
/Users/estebanmunchjones
````

**the first slash `/` means that we're at the root directory**

If a slash appears on the terminal, it means the root directory:

````
estebanmunchjones@Estebans-MacBook-Air /Users %
````

2. How to navigate to the home directory?

   ````
   instead of 
   cd ../..
   
   do
   
   cd or cd ~
   ````

   

3. How to navigate to the root directory?

   ````bash
   cd /
   ````

4. How to access folder with blank spaces:

   ````
   option 1: use `tab` to autocomplete name
   
   or
   
   cd my\ folder
   ````

   a back slash and a space represents the blank space

   

#### Relative vs absolute navigation

Navigating with `cd ..` we use relative paths.

Absolute path navigation:

````
cd /Users/estebanmunchjones/Documents/Coding
````

When creating bash scripts, use relative paths, because the abosolute will break when the project folder is moved.

Create a folder:

````bash
mkdir my-dir
````

Create a file: 

```bash
touch my-file.md
```

touch can also be used to change the date a file was modified.

Create multiple files:

````bash
touch file1 file2 etc
````

Delete a file permanently:

````
rm filename
````

Delete directory permanently:

````
rmdir dirname
````

The safety net: mkdir doesn't work when there files inside the folder! we'll get this error:

````
Directory not empty
````



How to remove the directory then?:

````
rm -r dirname
````



#### Adding flags to commands

To see the size of the files:

````
ls -s
````

check the manual to see all flags related to a command:

````
man ls
````



#### Copying and moving files

With the terminal opened at the parent folder of `data` and `copied` folders, run this:

````
cp data/index.html copied/
````

How to copy multiple files?:

````
cp -r data/ copied/
````

The above command copies the content of `data` and paste it inside `copied`.

**Important:** add the slashes! to refer to an existing folder



How to move a file?

````
mv data/index.html moved/
````

Can I move all the content of a folder? NO, but you can move a folder.

How to move entire folders?

````
mv data/more-data moved/
````

How to rename a file?

```
mv file file-renamed

e.g
mv script.js script-renamed.js
```



## Section 2. Version Managment with Git - The basics

Git doesn't save all the files when committing, just the ones that changed from the previous version.

Again:

**Git = tracking changes = doesn't store files again and again!**

When creating a new branch, a fresh copy of the working directory is available, where we can add new files, etc. So, there are 2 working directories: the one associated with the master branch, and the new one linked to the new branch.

What is the working directory/tree? The files I work on vscode

What is the Repository? The `.git` hidden file, with and `index` file (staging area) and an `object` folder (commits).

How to check git version?

````
git --version
````

How to open VS Code from terminal?

1. Open VS Code, go to command pallete with 

   ````
   cmd + shift + P
   ````

   and then select 

   ````
   Shell Command: Install code command in PATH
   ````

   

2. In the terminal, run:

   ````
   code .
   ````



How to see the state of the working directory?

````
git status
````

Untracked files are in the working directory but not part of the staging area. e.g:

````
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        initial-commit.txt
````

Untracked files are in red.

How to initialize a repository?

```
git init
```



How to change the name of the branch?

````
git branch -m <name>
````



How to see hidden files on finder (toggle)?

```
cmd + shift + .
```



How to add files to the staging area?

```
git add .
```

Then, running git status again:

````
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   initial-commit.txt
````

A user an email is linked to any commit, so it's not a mess.

This data is automatically picked up by git.



How to get my working tree of the first commit?

````
git checkout first-commit-id
````

Only copy and paste the first 6

Do I loose the latests commit when checking out? NO, but git log doesn't show them.

How to undo a commit checkout?

```
git checkout branch-name

e.g
git checkout master
```

Then, the working tree is restored to the last commit, and the git status log shows all the commits

How to check all the branches?

````
git branch
````

How to create a new branch?

````
git branch branch-name (whithout spaces)
````

How to checkout to that branch?

````
git checkout branch-name
````

The new branch has the same commits as the original branch, with the same ids

How to create a branch and checkout to it in a single command?

`````
git checkout -b branch-name
`````



What info does the commit have?

````
commit 9b3d84a6679a9c3db7631219a5d725d314edd729 (HEAD -> master, second-branch)
````

Here, it says that the comit 9b.. is the HEAD of the master AND the second-branch.

What is a branch? a container of snapshots (commits).

How to merge a branch?

````
git merge branch-name
````

E.g: located at the `master` branch, we can merge the content of `third-branch`:

````
git merge third-branch
````



What is HEAD? By default, is the latest commit in a branch

What is detached HEAD. A HEAD that doesn't belong to any branch. It's a good playground, without affecting branches:

Example: switching to `second-branch` and checking out to the last commit, it yields this:

````
estebanmunchjones@Estebans-MacBook-Air Git-basics % git checkout 9b3d84a6679a9c3db7631219a5d725d314edd729
Note: switching to '9b3d84a6679a9c3db7631219a5d725d314edd729'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 9b3d84a added second .txt file
````

Is it possible to be not on any branch? YES, when on a detached head. 

````
* (HEAD detached at 9b3d84a)
  master
  second-branch
  third-branch
estebanmunchjones@Estebans-MacBook-Air Git-basics % 
````



When checking out to a previous commit, we're in detached mode, because **we checked out from a branch to the middle of nowhere.**

New commands from version 2.30 > 

instead of checking out a branch:

````
git switch branch-name

same as

git checkout branch-name
````

````
git switch -c branch-name

same as

git checkout -b branch-name
````



#### Deleting data

##### Deleting a tracked file on the working directory

Deleting files on the working tree

How to see which files are in the staging area?

```
git ls-files
```

If I delete a file, it's still on the staging area.

This is the ouptut of `git status`:

````
estebanmunchjones@Estebans-MacBook-Air Git-basics % git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    second-commit.txt

no changes added to commit (use "git add" and/or "git commit -a")
````

So, `second-commit.txt` was deleted from the working tree, but still present on the staging area, that's why it's in red, because they defer.

Important! The deleted file should alse be removed from the staging area.

How to remove the deleted working tree file from the staging area?

````
git rm deleted-file-name
````

Oops! I deleted a file i wann restore, how?

````
git restore deteled-file-name
````



##### Undoing changes on unstaged files

git status shows me what thing are ready to commit, and what differences are between the working tree and the staging area.

The staging area, if nothing is added or removed, contains the same snapshot as the HEAD (usualy the last commit).

**I changed a tracked file on the working tree, and screwed it up, and I haven't staged it**. How to get the original file? in other words: **undoing unstaged changes**

````
git checkout -- (optional --) file-name

e.g
git checkout second-commit.txt

or new command

git restore file-name
````

the optional `--`  refers to `no specific branch`. 

How to revert changes on tracked files but not staged yet?

```
git checkout .

or

git restore .
```



##### How to deleted all not tracked unstaged files and folders?(e.g when you create them and want to get rid of them right away)

How to delete an unstaged file?

```
git clean -dn
Would remove not-useful.txt
```

d for delete and n for entries, to list them

To actually delete it

````
git clean -df
````



##### **How to undo staged changes?**

````
git restore --staged file-name or git reset file-name
git checkout file-name
````

Running this from the beginning doesn't work:

````
git checkout initial-commit.txt
Updated 0 paths from the index
````

Why? because the staging area and the working tree are the same, nothing is updated when checking out.

How to do it then?

1) Replace the staging area with the HEAD:

````
git reset file-name or git restore --staged file-name
````

So, now, the staging area

Do the normal checkout:

````
git checkout file-name
````

E.g

````
git checkout initial-commit.txt
Updated 1 path from the index
````

Updating path means that the working tree was changed!



#### How to undo commits?

1)How to reset just the HEAD?

````
git reset --soft HEAD~1
````

it means, reset the the previous commit (~1).

BUT, the working tree and the staging area aren't changed at ALL. The HEAD is what changed.



2)How to reset the HEAD and the staging area?

````
git reset HEAD~1
````

BUT, the working tree remains untouched.



3) How to reset the HEAD, the staging area AND the working tree?

````
git reset --hard HEAD~1
````

If you just messed up everything, and want to get back to the last commit,

````
git reset --hard HEAD
````



#### Deleting branches

Delete a merged branch:

````
git branch -d branch-name
````

Delete not merged branch:

````
git branch -D branch-name
````

How to delete multiple branches:

````
git branch -D branch-name-1 branch-name-2
````



##### Working with a detached HEAD

If we wanna go to a previous snapshot (commit) of the project, make some changes, and merge it with 

````
git checkout commit-id
````

Then, the `git branch` yields this:

````
* (HEAD detached from eb04ac5)
  master
````

We can add a file and commit it.

Then, the `git log` yields this: (pay attention to the HEAD standalone, not linked to any branch)

````
commit 255404bc5ef7089f13469e0c70be5a6bb6aa8fa2 (HEAD)
Author: Esteban Munch Jones <estebanmunchjones@Estebans-MacBook-Air.local>
Date:   Thu Feb 25 14:01:45 2021 +0000

    added detached.txt from detached head
````

We can now checkout to the master branch:

````
git checkout master
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  255404b added detached.txt from detached head

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 255404b
````

In order to keep or merge changes, the last commit done on the detached must be added to a branch, that must be created:

````
git branch detached-branch commit-id-done-on-detached-head

or 

git switch -c detached-branch
(this command above automatically adds the last commit done to the detached head)
````

The just merge to the master branch

````
git checkout master
git merge detached-branch

Will be asked to write a message. Steps:

press "i" (i for insert)
write your merge message
press "esc" (escape)
write ":wq" (write & quit)
then press enter
````



#### Ignoring files

Just create a `.gitignore` manually on the root of the working tree, then commit it.

The files names or folders written inside the `.gitignore` file are ignored by git and can't be staged.

There are 3 types of files on a working tree: untracked, tracked, and ignored.

How to ignore multiple files ending with `.log`, for example:

````
///.gitignore

*.log
````

How to add exceptions to the * rule?

````
//.gitignore

*.log
!test3.log
````

How to ignore folders?

````
//.gitignore

/node_modules
````



When doing `git clean -df`, the ignored files are not deleted!



