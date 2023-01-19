# GitHub real fast

There is a ton of info on the web about Git; this is just the bare
minimum you need to get rolling.

## GitHub

> GitHub, Inc. is an Internet hosting service for software development and version control using Git. It provides the distributed version control of Git plus access control, bug tracking, software feature requests, task management, continuous integration, and wikis for every project. Headquartered in California, it has been a subsidiary of Microsoft since 2018. ~  Wikipedia

- **Internet hosted**: your code is kept remotely. If you lose your local copy, it is in the cloud
- **version control**: every change committed to your Git repository is saved. You can revert back to any point in that history at any time.
- **distributed**: multiple contributors can work on the same code base from multiple machines. The changes each developer makes are deconflicted, approved, and tracked.
- **continuous integration**: pushing code to GitHub can trigger automated running of code to test, build, deploy, and more.

### GitHub repositories

A repository is where your code lives on GitHub.com

```{figure} img/github_repo.png
The GitHub repository that builds this very website.
```

### Clone from GitHub

See [GitHub Docs: Cloning a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository?platform=windows)

You can clone a repository with the `<> Code` dropdown menu.
If you have SSH keys configured, prefer that. Otherwise, use HTTPS.

For example, to clone [USAFA-ECE/helloLed](https://github.com/USAFA-ECE/helloLed)

Select the remote URL from the dropdown:

```{figure} ../lab/img/lab0_githubclone.png
Getting the remote URL
```

## Git

Git is a version control system. It allows you to commit your changes to history
and then push those changes to a remote repository.
You can also merge changes to collaborate with others
(beyond the scope of this course).

All the below examples will work in either Git Bash or Powershell
(if Git is installed on the Windows system. Git comes installed by default on
most Linux distributions).

### Clone

`git clone <remote url>` creates a local deep copy of the entire git project.

The code is placed in a folder with the project name.
A hidden `.git/` folder contains all the info to revert
back to points in the commit history.

```bash
git clone https://github.com/USAFA-ECE/helloLed.git
```

### Remote

`git remote` allows you to interact with the address of your non-local repository.

For example, to see the remote after cloning helloLed:

```powershell
git remote -v
origin  git@github.com:USAFA-ECE/helloLed.git (fetch)
origin  git@github.com:USAFA-ECE/helloLed.git (push)
```

You can also add, change, or remove remotes.
See [GitHub Docs: Managing remote repositories](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories).

### Branch

`git branch` shows which branch you are currently working on.

Default branch names are `master` or `main`.
For this course, you can push everything to `main` **but** better practice
is to create a branch for a new feature and then merge it in when complete.

This shows two branches: `main` and `workflow`. The `*` indicates that the user is currently on the `main` branch.

```bash
git branch -v
* main     eae5f1a Autofind _tb entities
  workflow 0fcc837 Revamp build script and docs
```

### Status

`git status` shows the current state of changes: untracked, unstaged, and staged.

This shows the current state *for this repo as I write it*.

Dude... meta...

```bash
 git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   book/appendix/github.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        book/appendix/img/
```

### Add

`git add <file>` adds a file to staging, from where it can then be committed.

From the status above, I want to add a file in the `img/`:

```bash
git add book/appendix/img/

# Now I can see the file
git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   book/appendix/github.md
        new file:   book/appendix/img/github_repo.png
```

### Commit

`git commit` is how I commit my changes to the version history.

It will only commit what is staged. In this example, I want to commit the .png
but not the .md file.

```bash
git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   book/appendix/img/github_repo.png

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        book/appendix/github.md
```

```bash
git commit

# This prompts me for a message to save with the commit
# I entered "Add github repo screenshot"
[main 4ddeb1c] Add github repo screenshot
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 book/appendix/img/github_repo.png
 ```

### Push

`git push` allows me to push my changes to the remote repository.

```bash
# "origin" is my remote
# "main" is my branch
# I could also use `git push` and "origin main" would be implied.
git push origin main

Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 114.00 KiB | 3.35 MiB/s, done.
Total 6 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:USAFA-ECE/ece281-book.git
   9bf376d..4ddeb1c  main -> main
```

After the push, I can see the changes on GitHub.com

```{figure} img/github_history.png
The new commit, after being pushed to remote.
```

### Pull

`git pull` is how you pull down updates from remote.

This is helpful if you switch machines and then need to pull changes
so that you have the lates version.

```bash
git pull origin main

From github.com:USAFA-ECE/ece281-book
 * branch            main       -> FETCH_HEAD
Already up to date.
```
