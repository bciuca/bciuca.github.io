---
layout: post
title: "Samsung SDK Setup"
description: ""
---
{% include JB/setup %}

These are some notes that I took when starting to develop Smart TV apps for Samsung TVs. The apps are HTML5 based, but do have some limitations and feature gaps from standard, desktop web browsers. The steps below outline how to setup the SDK and the emulator to test your apps.

## Prereqs

+ Installing SDK and Emulator v3.5.2 from [Samsung's download archive](http://www.samsungdforum.com/Devtools/SdkArchive).
+ The SDK requires C++ 2010 Redistributable. If you get a warning stating you have a different version, just uninstall the current version and proceed with the SDK setup.
+ [DirecX End-User Runtime](http://www.microsoft.com/en-us/download/confirmation.aspx?id=35) - must have this installed prior to starting the SDK installation.
+ Install the SDK and Emulator. You will be prompted to install Apache server, which you will need to serve your apps.

## Launching Apps

+ All applications are loaded from the path `C:\Program Files\Samsung\Samsung TV SDK(3.5.2)\apps`
  + Place your application directories here. For example, `C:\Program Files\Samsung\Samsung TV SDK(3.5.2)\apps\myApp`
  + The `myApp` dir structure will contain your index.html and other sub-directories.
+ Launch the applicable emulator (2010 - 2012), and select "Open App".
+ Here you will see a list of available applications.
