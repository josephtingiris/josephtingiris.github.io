---
layout: post
title: "2017-10-22-neovim-on-centos-7.md"
author: "Joseph Tingiris"
date: 2017-10-22 09:02:33 -0400
last_modified_at: 2017-10-22 09:02:33 -0400
categories:
    - howto
    - install
    - neovim
    - centos 7
tags:
    - howto
    - install
    - neovim
    - centos 7
tags:
published: true
---

# Another holy war?

There are countless text editors out there, and I know programming languages and programmers.  They're fiercely loyal to their favorites.  The [editor war](https://en.wikipedia.org/wiki/Editor_war) has been going on since the 70's.

Like it or not;  If you're a computer professional then you will eventually have to learn something about [vi](https://en.wikipedia.org/wiki/Vi) or [emacs](https://en.wikipedia.org/wiki/Emacs).  They share [The Oldest Rivalry in Computing](http://www.slate.com/articles/technology/bitwise/2014/05/oldest_software_rivalry_emacs_and_vi_two_text_editors_used_by_programmers.html).

Most people either love them or hate them.  I think the latter gave rise to the use of a plethora of other editors such as [pico](https://en.wikipedia.org/wiki/Pico_(text_editor)), [nano](https://en.wikipedia.org/wiki/GNU_nano), and the ever expanding choices of more modern [IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment).

This post is really about setting up and using [neovim](https://github.com/neovim/neovim) on [CentOS 7](https://www.centos.org/).  I'm both excited and tentative at the thought.  I spend a considerable amount of time in an editor, particularly vim, and this is a topic that interests me.

# But IDE X is so great!

The problem, for someone like me, is that it takes quality time to learn how to use an editor proficiently.  It takes even more time to integrate it with different development environments. Constantly switching from one set of tools to another can be extremely time consuming.

If you've ever been stuck working on a machine without *your* editor then you know where I'm coming from.  Like so many others, I had to learn vi out of necessity (and because it is way better than ed).  While I've used emacs for some time too, and fully recognize there are many others, none are as ever-present as vi.

I program a lot, in different languages.  Throughout the course of my life & career I've used countless IDEs.  Most focus on a single language very well and few cater to a polyglot.  They can be good, or bad, depending on your point of view and what you want to do.

Personally, I find it extremely cumbersome to be required to use a mouse.  It's akin to torture; [Carpal tunnel syndrome](https://en.wikipedia.org/wiki/Carpal_tunnel_syndrome) is real and yes, I feel it.  To this day, for any number of reasons, I still find myself going back to vi(m) for a large portion of what I do on a daily basis.  

Honestly, I've preferred using [vim](http://www.vim.org/) for decades.  It's like the lowest common denominator; It's always there.  It rarely changes.  The vim plugin/script ecosystem is constantly growing. The community never ceases to amaze me.

Yet, I still find myself checking out other IDEs and migrating to the newer ones like [atom](https://atom.io/) and [sublime](https://www.sublimetext.com/).  Like others, surely, I'm still searching for ***[The One](http://matrix.wikia.com/wiki/The_One)*** IDE that can handle all ***[The Source](http://matrix.wikia.com/wiki/The_Source)***

Either **neovim** is very cleverly named, or the author really likes ***The Matrix***.

Or maybe ...

![neo is not the one](http://www.memebucket.com/mb/2012/08/What-If-Neo-Is-Not-The-One-852.png)

... I'm just having fun with it. :lol:

The [neovim glitter](https://gitter.im/neovim/neovim) referred me here when I asked.  https://en.wiktionary.org/wiki/neo-

So, I guess it's ***newvim***

Seriously.  For the most part, [vim](http://www.vim.org/) with [scripts](http://www.vim.org/scripts/) is a very capable IDE for most programming languages. Configured appropriately, vim is far from a 'simple' text editor like vi. The [about](http://www.vim.org/about) page describes it like this.  *Vim is an advanced text editor that seeks to provide the power of the de-facto Unix editor 'Vi', with a more complete feature set.*

It also says, this.  
Most of the machines I've been working with lately are x86_64 CentOS or RHEL 7.  Many are production servers that were installed with the [Minimal ISO](https://www.centos.org/download/).  While it's not entirely obvious (or easy to search), mounting the [latest ISO](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso]) readily confirms the **only** editor that's installable is *vim-minimal*.  I've also encountered this working in initrd and initramfs environments.  It's been that way for years.

So, the writing is on the minimal Linux wall.  The traditional editor 'holy war' is over.  vi(m) won.


Frankly, I'll probably be programming as long as I can type because I love it.  I've still got at least another 15+ years until I can retire.  I also hope the next generation(s) of programmers will build on and improve what's been built before us.

About every year, or so, I take a look at the current state of the IDE and what's available.  Over the past couple years,

