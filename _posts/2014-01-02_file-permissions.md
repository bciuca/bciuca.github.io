---
layout: post
title: "Apache Webserver on Mac"
description: ""
category: 
tags: [Apache]
---
{% include JB/setup %}

# SymLinks

I have a Github project that I needed to run in a browser. My options to test my code were to either set my apache config to point to my Github dir, or I could have just copy and pasted the code to a test path in my existing web config.

For my workflow, I find it easier to test as I code. To accmomplish this I created a symlink:

<pre>
<code>
bciuca-macbook:~ bciuca$ sudo ln -s /Users/bciuca/dev/MyTestApp/ /Library/WebServer/Documents/testapp
</pre>
</code>

## File Permissions

To complete the step, I needed to set the correct file permissions:

<pre>
<code>
bciuca-macbook:~ bciuca$ chmod o+x ~/dev
bciuca-macbook:~ bciuca$ chmod o+x ~/dev/MyTestApp
</pre>
</code>
