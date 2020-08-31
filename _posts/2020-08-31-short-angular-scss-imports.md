---
layout: post
title: Short Angular scss imports
tags: angular scss angular.json
date: 2020-08-31 01:57 +0200
---
How to get
@import “variables”

You just need to add 

"stylePreprocessorOptions": {
  "includePaths": [
    "src/assets/global-scss/"
  ]
},
To:
angular.json: build.options.stylePreprocessorOptions

 They key thing here is that you import a folder not a file.
You import the global-scss folder, which contains variables.scss