---
layout: post
title: "Installing and building an existing project with npm and grunt on CentOS 7"
author: "Joseph Tingiris"
date: 2017-10-16 21:01:19 -0400
last_modified_at: 2017-10-16 21:01:19 -0400
categories:
    - centos 7
    - npm
    - grunt
tags:
    - howto
    - centos 7
    - npm
    - grunt
    - javascript
published: true
---

This is the basic process to install and build an existing project with npm and grunt on CentoOS 7.

[npm](https://www.npmjs.com/) is the package manager for JavaScript
```
yum install epel-release npm
```

[GRUNT](https://gruntjs.com/) is the JavaScript Task Runner
```
npm install -g grunt-cli
```

A typical project has a `package.json` and a `Gruntfile.js` in a `project_dir`, i.e.
```
cd project_dir
npm install
grunt
```
