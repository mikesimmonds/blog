---
layout: post
title: npm link Angular lib to project
tags: angular npm libraries library
date: 2020-08-31 01:57 +0200
---
You will need an angular lib and a separate Angular application, presumably in different locations

1. Build the lib you just made - using ng build from the workspace root
2. cd into the dist/<lib-name> folder
3. Run: npm link
4. This will create a symlink to the global npm module on your machine
5. Now we need to tell our app where to find the library...
6. Cd into the app root
7. Npm like <library-name>

Tips:
* Always develop libraries with the watch flag so it is rebuilt instantly and the changes are reflected in the app. Remember we are linking the dist folder, not the source code so you have to build the lib again to see the changes reflected in your application
