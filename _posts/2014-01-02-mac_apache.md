---
layout: page
title: "Symbolic Links and Apache on OS X"
description: ""
---
{% include JB/setup %}

### Creating Symbolic Links

I have a webapp project that I need to run in a browser locally on my develoment machine. My options to test the webapp is to either set my apache config to point to my development dir, or make a build sctipt to copy and paste the code to a test path in my existing web server root.

In this example, I am testing everything locally with no need for minification or other build steps before hosting the code. For simplicity, a symbolic link in the web server root will point to my development directory, `/Users/bciuca/dev/MyTestApp`.

The `ln -s` command will create the symbolic link, given a source and destination path (sudo is needed to write the symlink).

`sudo ln -s /Users/bciuca/dev/MyTestApp/ /Library/WebServer/Documents/testapp`

So the URL [http://localhost/testapp]() is now serving files from `/Users/bciuca/dev/MyTestApp/`.


[Further reading on symbolic and hard links in OS X](http://gigaom.com/2011/04/27/how-to-create-and-use-symlinks-on-a-mac/).


### File Permissions

To complete the setup, the correct file permissions need to be set in order for the pages to be served.

`chmod o+x ~/dev`

`chmod o+x ~/dev/MyTestApp`


### Restart Apache

Finally, restart Apache. 

`apachectl -k graceful`