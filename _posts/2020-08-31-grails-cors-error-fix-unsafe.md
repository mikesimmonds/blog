---
layout: post
title: Grails cors error fix unsafe
date: 2020-08-31 01:55 +0200
tags: 'ears'
---
Definitely check before going to prod, but you need to add this to application.yml: 
grails:
  cors:
    enabled: true