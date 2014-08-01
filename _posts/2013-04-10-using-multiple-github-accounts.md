---
layout: post
title: "Using Multiple GitHub Accounts"
description: ""
category: dev
tags: [github]
---
{% include JB/setup %}

#### Why would you need more than one GitHub account?
Ideally, having a single GitHub account with contributions to multiple organizations would be the simplest way. As it turns out, my current job came with a separate GitHub account. So now I have two GitHub accounts, one for personal projects and another for work. The problem I was running into was trying to keep my commits tied with the right GitHub ssh key. Luckily, git and ssh are configurable enough to get this sorted out. 

Through some simple googling, I found [Jeffery Way's blog post](http://net.tutsplus.com/tutorials/tools-and-tips/how-to-work-with-github-and-multiple-accounts/). The steps I outlined below is what I did to get this setup on my Mac. More details can be found on Jeffery's blog.

The following steps assume that you already have an existing key you use with one of your github accounts. For the purpose of this writeup, the default `id_rsa` key is associated with your personal GitHub account. 

#### Creating and adding ssh keys
A ssh key will be needed to use with the work GitHub account. In your `~/.ssh` directory, run the following ssh command:
`ssh-keygen -t rsa -C "me@mycompany.com"`

Rename the key to something else, for this example I went with, `id_rsa_mycompany` and `id_rsa_mycompany.pub` (private and public keys respectively).

Add the key to ssh:
`ssh-add ~/.ssh/id_rsa_mycompany`

#### Setting up GitHub with your new ssh key
Now add the new public key to the work GitHub account (this will be under your GitHub account settings). Open the `id_rsa_mycompany.pub` public key file in a text editor and copy the text. Or just copy directly from the terminal:
`cat ~/.ssh/id_rsa_mycompany.pub | pbcopy`

#### Configuring ssh
Configure ssh host names to use for work and personal. The config file is in `~/.ssh/config` or you can create one if needed: `touch ~/.ssh/config`

The first host is the personal account and the second entry is for the work account. You can name the `Hostname` whatever you like.
    Host github.com 
      Hostname github.com 
      User git 
      IdentityFile ~/.ssh/id_rsa
    Host github-mycompany
       Hostname github.com
       User git
       IdentityFile ~/.ssh/id_rsa_mycompany

#### Configuring git repo
In the respective git repos, configure the your email address.

    git config user.email "me@mycompany.com"

#### Testing it out
To test that the keys were added ok, you can run the following ssh commands.

    ssh -T git@github.com
    Hi bciuca! You've successfully authenticated, but GitHub does not provide shell access.

    ssh -T git@github-mycompany
    Hi bciuca-mycompany! You've successfully authenticated, but GitHub does not provide shell access.

 
