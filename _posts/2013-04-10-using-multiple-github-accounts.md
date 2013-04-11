---
layout: post
title: "Using Multiple GitHub Accounts"
description: ""
category: dev
tags: [github]
---
{% include JB/setup %}

## Working with multiple GitHub accounts
I have two GitHub accounts, one for personal projects and another for my current job. The problem I was running into was trying to keep my work and personal repos separate on my machine. Luckily, git and ssh are configurable enough to get this sorted out. Through some simple googling, I found [Jeffery Way's blog post](http://net.tutsplus.com/tutorials/tools-and-tips/how-to-work-with-github-and-multiple-accounts/). The steps I outlined below is what I did to get this setup on my Mac. More details can be found on Jeffery's blog post.

The following steps assumes that you already have an existing key you use with one of your github accounts. For the purpose of this writeup, the default `id_rsa` key is associated with your personal GitHub account. 

1. A ssh key will be needed to use with the work GitHub account. In your `~/.ssh` directory, run the following ssh command:
`ssh-keygen -t rsa -C "me@mycompany.com"`

2. Rename the key to something else i.e.:
`id_rsa_mycompany` and `id_rsa_mycompany.pub`

3. Add the key to ssh:
`ssh-add ~/.ssh/id_rsa_mycompany`

4. Now add the new public key to the work GitHub account (this will be under your GitHub account settings). Open the `id_rsa_mycompany.pub` public key file in a text editor and copy the text. In Mac's terminal, you can use the following  command to copy the content of the file to your clipboard:
`cat ~/.ssh/id_rsa_mycompany.pub | pbcopy`

5. Configure ssh host names to use for work and personal. The config file is in `~/.ssh/config` or you can create one if needed: `touch ~/.ssh/config`

   The first host is the personal account and the second entry is for the work account. You can name the `Hostname` whatever you like.

   ```
   Host github.com 
       Hostname github.com 
       User git 
       IdentityFile ~/.ssh/id_rsa
   Host github-mycompany
       Hostname github.com
       User git
       IdentityFile ~/.ssh/id_rsa_mycompany
   ```

6. Configure your git repo settings to use the configured ssh hostnames:
    
   ```
   [core]
       repositoryformatversion = 0
       filemode = true
       bare = false
       logallrefupdates = true
       ignorecase = true
   [remote "origin"]
       fetch = +refs/heads/*:refs/remotes/origin/*
       url = git@github-mycompany:OrgName/corporate-stuff.git
   [branch "xdev"]
       remote = origin
       merge = refs/heads/xdev
   [user]
       email = me@mycompany.com
   ```
    
7. To test that the keys were added ok, you can run the following ssh commands.
   ```
   ssh -T git@github.com
   Hi bciuca! You've successfully authenticated, but GitHub does not provide shell access.
   
   ssh -T git@github-mycompany
   Hi bciuca-mycompany! You've successfully authenticated, but GitHub does not provide shell access.
   ```
 
