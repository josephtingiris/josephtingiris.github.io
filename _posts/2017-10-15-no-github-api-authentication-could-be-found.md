---
layout: post
title: "GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data."
date: 2017-10-15 18:00:12 -0400
last_modified_at: 2017-10-15 18:18:18 -0400
categories:
    - jekyll
    - github
    - metadata
tags:
    - fix
    - jekyll
published: true
---

Here's a solution to fix issues like this ...

```
$ bundle exec jekyll serve
Configuration file: /home/jtingiris/src/josephtingiris.github.io/_config.yml
            Source: /home/jtingiris/src/josephtingiris.github.io
       Destination: /home/jtingiris/src/josephtingiris.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 16.628 seconds.
 Auto-regeneration: enabled for '/home/jtingiris/src/josephtingiris.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

1. Create a GiHub personal acces token with only the `public_repo` scope selectd.  Here's the [GitHub Guide](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)

2. Make sure to copy the token now.  It wont be shown again.  If you loose it then you'll have to create a new one.

3. Export the token as an environment variable, e.g. via .bashrc.  i.e.
```
export JEKYLL_GITHUB_TOKEN=330103c2f8b25f4a3301b5be3e0f56371a133301
```

4. Verify the token is in your environment, i.e.
```
echo $JEKYLL_GITHUB_TOKEN
```

5. Alternatively, you can use it inline, e.g.
```
JEKYLL_GITHUB_TOKEN=330103c2f8b25f4a3301b5be3e0f56371a133301 bundle exec jekyll serve
```

All said and done, jekyll should serve without warning.

```
$ bundle exec jekyll serve
Configuration file: /home/jtingiris/src/josephtingiris.github.io/_config.yml
            Source: /home/jtingiris/src/josephtingiris.github.io
       Destination: /home/jtingiris/src/josephtingiris.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 20.059 seconds.
 Auto-regeneration: enabled for '/home/jtingiris/src/josephtingiris.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```
