---
layout: post
title: "How to fix KDE Plasma OpenGL on CentOS 7 running as a VMWare guest"
author: "Joseph Tingiris"
date: 2015-02-22 22:19:59 -0400
last_modified_at: 2015-02-22 22:19:59 -0400
categories:
    - howto
    - fix
    - vmware
    - centos 7
    - kde
    - opengl
tags:
    - howto
    - fix
    - vmware
    - centos 7
    - kde
    - opengl
published: true
---

By default, CentOS 7 KDE Plasma Advanced Desktop Effects wont allow a Compositing Type of OpenGL.  This is a [bug](https://forum.kde.org/viewtopic.php?f=111&t=109963); Attempting to enable OpenGL 1.2, 2.0, or 3.1 on a VMWare (workstation) guest causes KDE to crash.  The Desktop Effect settings continously revert back to a Compositing Type of XRender.

It turns out that there's a workaround that's fairly straightfoward, with one caveat.  OpenGL 2.0 will work, but OpenGL 3.1 will not.

Here's how to use a Compositing Type of **OpenGL 2.0** qith the Qt graphics system **Raster**.

1. As root, add an KDE environment script to ''/etc/kde/env'', i.e.

    ```
    vi /etc/kde/env/force_opengl.sh

    ```

2. The content of the script should look something like this (note: KWIN_COMPOSE is ''NOT'' ZERO2, it's OH2).

    ```
    #!/usr/bin/bash

    export KWIN_DIRECT_GL=1
    export KWIN_COMPOSE=O2
    ```

3. Log out of KDE & log back in again (or reboot)

4. Click **Start -> System Settings -> Desktop Effects -> Advanced**, change the following, & click **Apply**.

    ```
    Compositing Type: OpenGL 2.0
    Qt graphics System: Raster
    ```

5. Log out of KDE & log back in again (or reboot)

6. Click **Start -> System Settings -> Desktop Effects -> All Effects**, select what you want, & click **Apply**.

7. A good effect to test & see OpenGL 2.0 in action is the ***Desktop Cube***.
