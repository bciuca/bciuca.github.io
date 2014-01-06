---
layout: post
title: "Symbolic Links and Apache on OS X"
description: ""
---
{% include JB/setup %}

## Procedure

1. Create the link

   `sudo ln -s /Users/bciuca/dev/MyTestApp/ /Library/WebServer/Documents/testapp`

2. Set directory permissions

   `chmod o+x ~/dev/MyTestApp`



### Background

I have a webapp project that I need to run in a browser locally on my develoment machine. My options to test the webapp is to either set my apache config to point to my development dir, or make a build sctipt to copy and paste the code to a test path in my existing web server root.



#### Symbolic Links

In this example, I am testing everything locally with no need for minification or other build steps before hosting the code. For simplicity, a symbolic link in the web server root will point to my development directory, `/Users/bciuca/dev/MyTestApp`.

The `ln -s` command will create the symbolic link, given a source and destination path (sudo is needed to write the symlink).

`sudo ln -s /Users/bciuca/dev/MyTestApp/ /Library/WebServer/Documents/testapp`

So the URL [http://localhost/testapp]() is now serving files from `/Users/bciuca/dev/MyTestApp/`.


[Further reading on symbolic and hard links in OS X](http://gigaom.com/2011/04/27/how-to-create-and-use-symlinks-on-a-mac/).

In order for Apache to serve the files, you must set the correct file permissions listed above in the Procedure step 2.

