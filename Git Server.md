# Git Server HOWTO

This HOWTO explains how to easily setup a git server on Linux.

## Install Ubuntu Server

Make sure you enable OpenSSH and you have a user account that can access
the server through SSH.

In the rest of this document I'll use the following example names:
- "marc" the user
- "workstation" the user's workstation
- "server" the server (which also has a "marc" user account)

## Create a bare repository (on the server)

Create a new directory on the server to host your Git repositories. I'll use
/home/marc/repo as an example.

Create a /home/marc/repo/TestProject.git directory on the server and create
a bare repository:

    mkdir /home/marc/repo/TestProject.git
    cd /home/marc/repo/TestProject.git
    git init --bare

This will create an empty repository which will be used for sharing. "--bare"
means this is NOT a client Git repository.

## Configure Git user name and email (on the workstation)

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

## Create a working repository (on the workstation)

On the workstation, create a project folder for your test project and create a README.md
file:

    mkdir /home/marc/projects/TestProject
    cd /home/marc/projects/TestProject
    touch README.md

Initialize a working Git repository:

    git init

Check to see the status of this repository:

    git status

You should see we have an untracked README.md file. Add it:

    git add README.md

If you check the status again, you should see that Git sees README.md as a new file. Now we
can commit it:

    git commit -m "added readme"

Our workstation repository is now uptodate.

## Adding a remote and pushing changes to it

If we want to sync our workstation repository with the one on the server, we need to add
a remote on the workstation:

    git remote add server marc@server:repo/TestProject.git

You can see the remote with the following command:

    git remote -v

This should list the remote for fetch and push.

Now try to push your local changes to the server:

    git push server master

It will ask your password on the server and then should proceed to push the changes to the
server. "master" stands for the "master" branch.

We can check to see the changes it made on the server, by logging on to the server, going
to the TestProject.git folder and requesting a commit log:

    cd /home/marc/repo/TestProject.git
    git log

This should show the commit.

## Caching credentials

If you don't want to enter your password with every push to the server, you can cache
your credentials with the credential helper:

    git config --global credential.helper cache

By default, your credentials will be cached for 15 minutes.

You can also change the timeout (in seconds):

    git config --global credential.helper 'cache --timeout=3600'

## Installing cgit

Cgit is a fast web frontend for Git. Ubuntu has a nice package for it. Install it along
with Apache2:

    sudo apt install apache2 
    sudo apt install cgit

Now you can configure /etc/cgitrc. Set the scan-path to let cgit search for all your
repositories, so you don't have to specify them manually:

    css=/cgit-css/cgit.css
    logo=/cgit-css/cgit.png

    scan-path=/home/marc/repo

Access your repositories on http://server/cgit

## Create backup's with snapshots

todo

That's all :-)