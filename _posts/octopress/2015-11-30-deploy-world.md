---
layout: post
title: "Deploy World"
date: 2015-11-30T14:11:28-08:00
---

To host the statically generated content on github, you'll need to first create a
Github repository named \<yourname>.github.io.  Then create an octopress _deploy.yml
file.

```yaml title:_deploy.yml
method: git
site_dir: _site
git_url: git@github.com:loops/loops.github.io
git_branch: master
```

You'll have to have your ssh keys setup properly to allow pushing so that the
following command can generate and deploy content to your Github page:

```bash title:"Deploy"
$ octopress deploy
```

This will create the static content in a .deploy directory before uploading it
for free hosting on Github.  You'll want to add this .deploy directory to your
.gitignore file.

You should be able to see your blog site working on <http://loops.github.io> now.
Check out the /feed/index.xml that I added to the repo so that RSS subscriptions
work.

