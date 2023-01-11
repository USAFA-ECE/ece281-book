# BCE 0: Get started with Git

This is a "Before Class Exercise." You can wait to complete it with one of the labs or In Class Exercises, but doing this early will save you time later.

```{contents}
:local:
:depth: 2
```

Using proper version control techniques is an expectation for this course, and you should expect yourself to use it for *all* of your courses that involve writing code.

## Overview

This exercise will get you ready to use Git for version control of your projects. It will prepare you to use GitHub Classroom to remotely store and submit your code.

GitHub is the most popular site for hosting open source code. It also allows for hosting private code. The fact that we are using it is *not* an endorsement by USAFA or USG.
Note that there are other options. For example:

- [GitLab](https://about.gitlab.com/) has fantastic integrated DevOps and project management. It is often the best choice for self-hosted options.
- [Bitbucket](https://bitbucket.org/) integrates well with the rest of the Atlassian suite.

Git is software that allows for distributed version control between multiple collaborators.

## Create GitHub account

Use a **personal email** to create account on [GitHub](https://github.com/). The account name should be professional, can include part or all of your real name, but can also be a screen name. Keep in mind it will be public and GitHub functions as sort of a social media for developers.

Go to [email settings](https://github.com/settings/emails) and *add and verify* your **USAFA email**.

Check the box "Keep my email address private" and copy the generated `@users.noreply.github.com` email for later use.
Check the box "Block command line pushes that expose my email."

Use your GitHub account to sign in to [GitHub Classroom](https://classroom.github.com/)

**Optional** [Get student benefits](https://education.github.com/benefits) for your account.

## Git

Git is software that allows for distributed version control between multiple collaborators. It was created to manage development of the Linux kernel, but is now the most widely-used version control system in the world.

```{note}
If you are going to learn to do anything useful for the Air Force or Space Force with software, you need to know how to use Git.
```

[Git Flow](https://docs.github.com/en/get-started/quickstart/github-flow) with large teams can be a little overwhelming at first, but for this course you need to understand that:

1. You have a git repository that your code lives in.
2. You make local changes to your code.
3. You *stage* those changes to be committed.
4. You *commit* those changes.
5. You *push* those changes to the remote repository (in our case, GitHub.com).
6. Others can view those changes on the remote repository.
7. Directly pushing works fine if you are the only collaborator, but as soon as there is a second person contributing to the same code, you must use [branches](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches) to avoid conflicts.

### Install Git

Install [Git Bash for Windows](https://gitforwindows.org/).

*TODO: What does this look like for MissionNet imaged laptops?*

Default options usually work pretty well, but:

- Select Components: use defaults, but make sure "Add a Git Bash Profile to Windows Terminal" is checked
- Choosing the default editor: Use Vim if you like computers and cyber, use Nano otherwise.
- Initial branch names: the tide is going to use "main" instead of "master", but up to you.
- Git from the command line and also from 3rd-party software
- Use external OpenSSH, since it comes with PowerShell
- Use native Windows Secure Channel library
- Checkout Windows-style, commit Unix-style line endings
- Git Crededntial Manager

## Configure git

Open up Terminal (PowerShell) or Git Bash (stripped-down Linux).
If in PowerShell, make sure git is installed.

```powershell
git --version
```

We have to tell our local git environment who we are. Use your GitHub username and the noreply email you found when you setup GitHub

```powershell
git config --global user.name <github user name>
git config --global user.email "<github noreply email>@users.noreply.github.com"
```

### Setup SSH key

This is optional, but will keep you from having to type your password over and over.

Go to [GitHub SSH Keys](https://github.com/settings/keys) follow their guide "generating SSH keys."

```{hint}
SSH keys use a public/private keypair to cryptographically prove your identity.

By default, SSH keys are stored in C:\Users\user.name\.ssh in Windows or
/home/username/.ssh/ in linux.

The private key will be named id_algorithm and the public key will be id_algorithm.pub by default.
```

Some cadets were having issues with ed25519. Try RSA instead:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

1. Generate an SSH key, put it in the default location
2. Print the **public** key and upload to GitHub
3. Guard the private key, fiercely

Check that your key works

```bash
ssh -T git@github.com
# Hi user! You've successfully authenticated, but GitHub does not provide shell access.
```

## Complete the Git Basics assignment

Your instructor will provide you an invite link in Teams to a "Git Basics" assignment. Completion of this assignment will walk you through all the steps for basic git operations.

### Install an IDE (optional)

An integrated development environment (IDE) is software for building applications that combines common developer tools into a single graphical user interface (GUI).

- [VS Code](https://apps.microsoft.com/store/detail/visual-studio-code/XP9KHM4BK9FZ7Q) is an incredible IDE and can be installed from the microsoft store or [downloaded](https://code.visualstudio.com/Download).
- [Notepad++](https://notepad-plus-plus.org/) is simple and allows for opening a wide variety of file types.

We recommend you install and learn both of these, but if you feel tough, you can just use notepad.exe

### Complete the assignment

Accepting the assignment will create a private copy of the template repository. Your instructor will be able to see your repository, but your other classmates will not.

[clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) your repository using SSH if you have an SSH key, or HTTPS otherwise.
If you are using VS Code, you will see an option on the welcome page to clone from directly within VS Code. Or [use the command palette](https://learn.microsoft.com/en-us/azure/developer/javascript/how-to/with-visual-studio-code/clone-github-repository?tabs=create-repo-command-palette%2Cinitialize-repo-activity-bar%2Ccreate-branch-command-palette%2Ccommit-changes-command-palette%2Cpush-command-palette).

> Read the README and finish "Steps to complete assignment"
