---
layout: post
title: "Hello World"
date: 2015-11-30T12:11:54-08:00
---

So I'm going to use these github pages as a place to stash random programming and
configuration tips.  It's probably too ambitious to think it will turn
out to be useful for myself, let alone anyone else.  Having said that, might as well
document setting up this blog since many of the google results for using Octopress on
Github are outdated.  Although, there's lots of good information on their website
<http://jekyllrb.com/docs/home/>.

Initial Setup
-------------

```bash
$ mkdir ~/blog
$ cd ~/blog
$ vi Gemfile
```

```ruby title:"Gemfile"
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
    gem 'octopress', '~> 3.0.11'
    gem 'octopress-image-tag'
    gem 'octopress-video-tag'
    gem 'octopress-quote-tag'
    gem 'octopress-codefence'
    gem 'octopress-gist'
    gem 'octopress-genesis-theme'
    gem 'octopress-solarized'
end
```

```bash title:"Installing ruby and the gem bundle program" start:4
$ sudo dnf install rubygem-bundler
$ bundle install
$ octopress init .
$ vi _config.yml
$ vi .gitignore
$ mkdir -p _plugins/theme/
$ vi _plugins/theme/_config.yml
```

An idea of what to put into the \_config.yml[^1] and .gitignore[^2]
files can be seen below.  The main thing of note at this point is that very little is
needed in your ~/blog directory.  Almost everything, including the default theme will
be held in gems not checked into our blog source directory.  That's why you have to
create a separate _plugins/theme/_config.yml[^3] file to override the title
which is set in the theme gem.

```bash title:"Create your first post"
$ octopress new post "Hello World" -D octopress
New post: _posts/octopress/2015-11-30-hello-world.md
$ vi _posts/octopress/2015-11-30-hello-world.md
```

After entering some content in Markdown format, you check out your results by running
a jekyll server instance and then going to <http://localhost:4000> with your web browser

```bash title:"Running a local instance of your blog"
$ jekyll serve -w
Configuration file: /home/sean/blog/_config.yml
            Source: /home/sean/blog
       Destination: /home/sean/blog/_site
      Generating...
                    done.
 Auto-regeneration: enabled for '/home/sean/blog'
Configuration file: /home/sean/blog/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

Committing to Git
-----------------

Once you're happy with this initial blog entry it's time to commit all your source files
to Git and push them to Github.  This repository will only store the source files, not
the generated content.  First create a repository named "blog" on Github, and then:

```bash title:"Commit to Git"
$ git init
$ git add .
$ git commit -m "Initial Checkin"
$ git remote add origin git@github.com:loops/blog.git
$ git push -u origin master
```

Example Files
-------------

[^1]: example \_config.yml below
```yaml title:"_config.yml"
title: Loop's Tech Tips
email: sean@eztux.com
description: >
  You're not likely to find much of any practical value for yourself
  here on this site.  It's meant mostly as a dumping ground for
  random programming and configuration items.  If anything you read
  here breaks something for you, you get to keep both halves.  Cheers!
url: "http://loops.github.io"
twitter_username: linux_or_bust
github_username:  loops

# Don't include these files when we build our site
exclude: ['Gemfile.lock', 'Gemfile']

# Select our desired Markdown processing engine
markdown: kramdown

# Configure the Kramdown engine to enable its Github features
kramdown:
  input: GFM

# You could use the popular Redcarpet markdown engine instead
# It has a few extensions to do things like convert plain URL's to links
redcarpet:
  extensions: ["tables", "autolink", "strikethrough", "space_after_headers", "with_toc_data", "fenced_code_blocks"]

# Octopress settings
post_ext: md
page_ext: html
post_layout: theme:post
page_layout: theme:page

# Make sure these gems are loaded, when we produce our static site
gems:
  - octopress-genesis-theme
  - octopress-solarized
  - octopress-image-tag
  - octopress-video-tag
  - octopress-quote-tag
  - octopress-codefence
  - octopress-gist
```

[^2]: example .gitignore below
```plain title:".gitignore"
_site
*.swp
.ink-cache
.sass-cache
.code-highlighter-cache
```

[^3]: _plugins/theme/_config.yml exmple below
```yaml title:"_plugins/theme/_config.yml to override options set by the theme gem"
title: Loop's Tech Tips
subtitle: It's a big world
```
