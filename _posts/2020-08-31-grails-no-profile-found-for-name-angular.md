---
layout: post
title: Grails No profile found for name [angular]
tags: grails angular error
date: 2020-08-31 01:55 +0200
---
Message:
Error occurred running Grails CLI: No profile found for name [angular].

This is due to a problem with the build/.dependencies file located within your Grails project root. If you delete or rename this file, then run grails again, the dependencies will be corrected and you can run your app.

To show the build folder in IntelliJ: click the cog icon and ’show excluded files’. 

https://github.com/grails/grails-core/issues/10898#issuecomment-356548131