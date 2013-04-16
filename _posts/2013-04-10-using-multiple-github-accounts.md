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
<pre>
    <code>
        Host github.com 
           Hostname github.com 
           User git 
           IdentityFile ~/.ssh/id_rsa
        Host github-mycompany
           Hostname github.com
           User git
           IdentityFile ~/.ssh/id_rsa_mycompany
    </code>
</pre>

#### Configuring git repo
Configure your git repo settings to use the configured ssh hostnames. The config file can be found in the `.git` directory of your git repo.
<pre>
    <code>
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
    </code>
</pre>

While you can have global git user preferences set, such as email, I configured my email in the project git config to use my work address.

For my personal repo, I just use my default global user settings and the usual git ssh addresses (i.e. git@github.com). So there is really nothing more to add to your other git config unless you need further customization.

#### Testing it out
To test that the keys were added ok, you can run the following ssh commands.

<pre>
    <code>
        ssh -T git@github.com
        Hi bciuca! You've successfully authenticated, but GitHub does not provide shell access.

        ssh -T git@github-mycompany
        Hi bciuca-mycompany! You've successfully authenticated, but GitHub does not provide shell access.
    </code>
</pre>

 
