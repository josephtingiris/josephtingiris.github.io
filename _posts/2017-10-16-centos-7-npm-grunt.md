---
layout: post
title: "Installing and building an existing project with npm and grunt on CentOS 7"
author: "Joseph Tingiris"
date: 2017-10-16 21:01:19 -0400
last_modified_at: 2017-10-21 18:37:19 -0400
categories:
    - centos 7
    - npm
    - grunt
tags:
    - howto
    - centos 7
    - npm
    - grunt
    - epel
    - javascript
published: true
---

This is the *basic* process to install and build an existing GRUNT project with npm on CentoOS 7.

1. Install npm via CentOS EPEL

    [npm](https://www.npmjs.com/) is the package manager for JavaScript

    ```
    yum install epel-release npm
    ```

2. Set your npm prefix to somewhere you have write access to *bin*, *etc*, & *lib*.  The *bin* directory should be in your $PATH, i.e.

    **It is *not* recommended to run npm as root.**

    ```
    npm config set prefix '~/'
    export PATH=~/bin:$PATH
    ```


3. Install GRUNT

    [GRUNT](https://gruntjs.com/) is the JavaScript Task Runner

    ```
    npm install -g grunt-cli
    ```

4. Install project_dir

    A typical GRUNT project has a `package.json` and a `Gruntfile.js` in a `project_dir`, i.e.

    ```
    cd project_dir
    npm install
    grunt
    ```
