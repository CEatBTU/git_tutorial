# Tutorial: Supporting Collaborative Working with Version Control using git

## Introduction

Working collaboratively on an open-source project can be very difficult. How to ensure the different project partners see each other's changes? How to manage conflicts? How to keep track of the changes? How to ensure that the source code evolves over time, and doesn't break apart as multiple partners modify different parts of the same set of files?

*Version Control Systems (VCSs)* try to address this exact requirement. VCSs are like ledgers that keep track of changes (in the source files) that happen over time.
VCSs thus allow for saving modifications to files over time, inspecting those changes, reverting them when things go bad (e.g. a bug shows up), and also attributing each changes to the responsible partners! Examples of VCSs systems include *svn*, *mercurial/hg*, and, scope of this tutorial, also *git*. 

*git* is not a toy, and was developed by none else than Linus Torvalds in 2005 to support the collaborative development of the Linux kernel, and its use has since then spread to many open-source projects hosted in numerous platforms such as [github] and [bitbucket]. Unlike previous VCSs, *git is distributed*, meaning that no *centralized* repository acts as a coordinator (i.e., there is no master-slave relationship); rather, each individual copy of a *repository* (the set of files that are being kept track of) is complete on its own.

### Objectives

There are tons of online tutorials on git, but few enable a hands-on experience as this one. In this hands-on tutorial, an introduction to the basic concepts and commmands of *git* will be presented. By the end of this tutorial, you should be able to understand the following concepts and commands, and perform the associated tasks:
- **init** a new repository using `git init`;
- **clone** a **repository** using `git clone`;
- **inspect** a **repository** using `git status` and `git log`
- working with files, **commiting** changes and **pushing** these changes to a **remote repository** using `git add`, `git commit`, and `git push`

## Preliminaries Pt 1: A few quick clarifications before starting

- `git` is the name of the tool. [github] \(and also [bitbucket]) are platforms supporting the hosting of **git repositories**.
- A **repository** is a, roughly speaking, a folder with contents that git keeps track of. git does this by using a `.git` folder inside the repository to store metadata related to the repository (including the ledger information).
- The official page for the git tool, and also an excellent source of materials and references, is https://git-scm.com/ 

## Preliminaries Pt 2: Installing git

git is supported both in Windows as well as in Linux, and can be used with or without a Graphical User Interface (GUI).  

- **Windows installation**: download git from https://git-scm.com/download/win. The Windows version comes with a command line mode and also a GUI. 
- **Linux**: use your package manager to download the equivalent of the git package, usually named simply **git**. The linux implementation usually comes only with the command line mode, but newer versions are also shipping with a GUI named [gitk](https://git-scm.com/docs/gitk).

This tutorial will focus on the utilization of git via command line, as it enables a broader range of customizations and provides a solid foundation for using the GUI later on.

## Lesson 1: Initializing an empty repository

To create a new git repository, simply `cd` (change directory) to the folder containing the files you want to put under version control. Inside this directory, run `git init`. You will notice that a new `.git` folder was created inside: this contains all metadata for your git repository, which has just been created.

Notice that, at this stage, files are not yet under version control. To do so, files must first be **added** to the **staging area** and **committed**. This will be presented in Lesson 4.

## Lesson 2: Cloning an existing repository

The command `git clone <repository_url>` does a **clone** operation, which is exactly what its name suggests: it creates, *locally*, an exact copy (clone) of the repository whose path is given by `<repository_url>`.

This repository provides a good platform to try this command. Try cloning the source files for this repository by executing `git clone git@github.com:CEatBTU/git_tutorial`. This will create in your current directory a `git_tutorial` folder, which is an exact copy of the repository located at `github.com/CEatBTU/git_tutorial`.  `cd` into it and notice the existence of a `.git` folder.

## Lesson 3: Inspecting a git repository

Two fundamental commands for inspecting the files and status of a repository are `git status` and `git log`.

## Viewing the current repository status

`git status` gives you an overview of the current status of the repository. After cloning it, you should see something that looks like this:

```bash
[johndoe@vm git_tutorial]$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

A few important information here:

- A *branch* represents, roughly speaking, a fileset within the repository (one repository can have multiple sets of files, i.e. branches). The default *branch*, by convention, is always called master. It can be used, for example, for separating filesets associated with a *stable* and a *development* version of the same code. For now, we just take the master branch for granted - I will cover branches in a future update to this post.
    - Note: being pedantic, a *branch* is actually a pointer to a particulate state of your repository. But, for didatic purposes, let's treat it as a set of files at the moment.
- *This branch* is up to date with `origin/master`. **origin** is the name of a *remote* (the name of a remote location) and **master** is the name of the *branch* in the *remote* **origin.** This message tells you that your files are synchronized with the remote repository, ***to the best of your git client's knowledge.***
    - Note: *git* does not work in an automatic way like cloud synchronization clients (e.g. Dropbox, OneDrive). Synchronization must be made explicitly via some commands that will be covered in Lesson 4, such as `git fetch`, `git pull` and `git push`. That means that a change may also have happened in your remote location, but yet git is unaware of that.
- The last message tells you that your *working tree* (the set of files that are being kept track of) is clean; that is, nothing has been modified, and nothing is *staged for commit*.
- A *commit* is a transaction on the git history. Roughly speaking, git performs version control efficiently by tracking only the changes between different commits. A commit then represents a set of files that were changed, along with those changes.

## Viewing the current repository log (all commits)

`git log` allows you to view the latest *commits* that were done in the repository. When running it, you should see something that looks like this:

```bash
[johndoe@vm git_tutorial]$ git log
commit 172345d78eb8f615c1bba579f411dddc17ccf7e3
Author: Marcelo Brandalero <marcelo.brandalero@emailhidden.de>
Date:   Fri Apr 10 11:37:20 2020 +0200

    Create file2.txt

commit 2ef6d1d322c6bfefa60a3eea1ce9c47b2b27b3e8
Author: Marcelo Brandalero <marcelo.brandalero@emailhidden.de>
Date:   Fri Apr 10 11:36:52 2020 +0200

    Create file1.txt

commit 84a3d776e0706630306cce5e6ef414bceab3592e
Author: Marcelo Brandalero <marcelo.brandalero@emailhidden.de>
Date:   Fri Apr 10 11:36:02 2020 +0200

    Create README.md
```

This history is sorted as the most recent commit on top, the least recent on the bottom. This command is useful to check the latest changes introduced to the current file set.

The first line indicates that the most recent commit was done by me on Apr 10 18:31:08 2020 +0200, and this commit has a hash value *172345d78eb8f615c1bba579f411dddc17ccf7e3*. This is a unique identifier for the commit, and is useful when needing to access the state or just some files from this commit later on.

Let's go ahead and start working with the files.

## Lesson 4: Working with files in the repository

You should see in the `tutorial` folder two files. Go ahead and:

- modify *file1.txt* as you like
- create a new *file3.txt* as you like

Now run `git status .` in the root of the repository again. You should see something like this

```bash
[johndoe@vm git_tutorial]$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   tutorial/file1.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	tutorial/file3.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Files in your git repository may be in a few different situations. This is a good time to introduce the **staging area.** As written earlier, a *commit* represents a set of transactions, a set of modifications in files which will be tracked and labeled with a commit hash. The **staging area** is the place where you put the files that will be *committed*. So the process is like this:

1. Modify or create a file
2. Add it to the staging area
3. Commit

Step 1 above has just been completed. Now the two files are in the following situation:

- *file1.txt:* this is a file that git was already tracking before, as it was included in a previous commit. i.e., it is part of the current commit, also called the HEAD of the repository. Since git detected a change, it lists this file as modified, but not staged for commit.
- *file3.txt*: git has detected that this file is new (it is not part of the HEAD of the repository), and git is not tracking changes to it. That's why its status is listed as untracked.

To add these files to the staging area, run `git add tutorial/file1.txt tutorial/file3.txt`. You should now see the following updated status:

```bash
[johndoe@vm git_tutorial]$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   tutorial/file1.txt
	new file:   tutorial/file3.txt
```

Before we continue, I leave here a figure taken from the [Pro Git book, written by Scott Chacon and Ben Straub](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) which provides an overview of how the lifecycle of files in a git repository.

![git staging area.](https://git-scm.com/book/en/v2/images/lifecycle.png)

## Lesson 5: Committing your changes

**Commit** represents the process of completing a transaction in your repository and registering those changes, so that, in the future, you may return to these. 
**commit** only updates your local copy of the repository, not the remote one. Separating these two things is a very nice feature of git: it allows one to work on your own speed in implementing features and solving bugs, making intermediate commits as you wish, and allowing you to update the remote copy only when you have a stable version. 

You commit changes to a repository by using the command `git commit -m "Your commit message."` . Depending on your coding style, you may use different types of commit messages. Notice that the set of file changes is automatically tracked by git, so it is probably more useful to include in your commit message a summary of what was changed or why it was changed. For example, while changes in your code will be tracked by *git,* tracking the reason for that is up to you. Does the current commit fix a bug or implement a new feature? Is it part of the solution? These are all useful information to include in the commit message. 

After running the above command, you should see the following output:

```bash
[mbrandalero@vm git_tutorial]$ git status
[master 1d14291] "Your commit message".
 2 files changed, 4 insertions(+)
 create mode 100644 tutorial/file3.txt
```

Notice the following:

- *1d14291* is a shortened version of the hash associated with the current commit. This is the *id* of the commit and, as mentioned earlier, can be used later on to rever to this specific "snapshot" (the commit) of the file set.
- two files were changed, and the "4 insertions" message means that 4 lines were added to git. git tracks changes on a line-by-line basis, which makes it perfect for efficiently storing source code and data set, and not so efficient for binary files.

## Lesson 6: Pushing your changes

**Push** is the process of actually sending your local changes to the repository to the remote location. Which remote location? The one you just cloned the repository from, or a new one you can register. In order not to break this tutorial, the next steps show how to **register a new remote** using [github] and upload these changes to the new place.

### Registering a new remote

Your new remote can be located in one of the available online repository hosting services such as github or bitbucket, or can even be in another server. We'll stick with the simple approach of uploading your code to github.

First, [register/log in here](https://github.com/login) and follow the steps on the website to create a new repository. You will first have to fill a screen that looks like this (after finding the "create repository" button which can be seen all over the website):

![](img/github_1.png)

The steps above should be pretty self-explanatory. For the purposes of this tutorial, make sure you **don't** check the "Initialize this repository with a README" box. After you create the repository, you'll land upon such a page:

![](img/github_2.png)

Well, the steps are already describe here. So what we want to do is push an existing repository (your local, updated repository) to this newly-created remote one. We will do one small modification to this command: since you cloned the local repository from the web, it already has a remote with the name `origin`. Go ahead then and execute the `git remote add new-origin <your_remote_address.git>` to register the new remote.

### Pushing your changes to the new remote

Finally, just run `git push -u new-origin master` to push your local changes to your newly-created remote repository. You should see your repository online after the command executes, along with something that looks like the following lines in the terminal output:

```bash
[johndoe@vm git_tutorial]$ git push
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 2 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 450 bytes | 450.00 KiB/s, done.
Total 5 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:johndoe/git_tutorial
   f36c568..1d14291  master -> master
```

## Conclusions

In this short tutorial, a brief introduction to *git* was presented. You should know how to use the following basic commands:

- `git clone`
- `git log`, `git status`
- `git add`, `git commit`, `git remote`, `git push`


### TO-DO:

An improved version of this tutorial will show how to work with branches (`git branch`), revert to past commits (`git checkout`) and merge code (`git merge`) from different branches.
