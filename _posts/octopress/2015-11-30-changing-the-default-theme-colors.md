---
layout: theme:post
title: "Changing the Default Theme Colors"
date: 2015-11-30T17:11:15-08:00
---

So it turns out that there are some minor color artifacts for the current version of
the [octopress-genesis-theme](https://github.com/octopress/genesis-theme).

I could not find a good way to override the settings without copying way too much of the
theme into the content repo.  Really this is cross-poluting the content repo, with
the theme repo's... but what can you do.

```bash title:"Copy the current theme settings"
$ octopress ink copy theme
```

```plain title:"_plugins/theme directory after 'ink copy theme' command"
config.yml
stylesheets
stylesheets/core
includes
layouts
templates
```

So then to keep the polution to a minimum, I deleted everything but the stylesheets/core[^1]
directory after which _colors.scss and _sizes.scss could be customized.  Once everything
is working locally, commit the changes to git, and deploy to github.


[^1]: Actually I had to restore config.yml from git since the "ink copy" overwrote it
