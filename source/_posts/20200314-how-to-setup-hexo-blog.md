---
title: 【Hexo】1. How to Setup Blog with Hexo and Next on GitHub Pages
tags:
  - GitHub
  - Hexo
categories:
  - Development
  - Hexo
abbrlink: 54991
date: 2020-03-14 19:47:21
---
### Quick Start

This article simply talks about how to setup blog with Hexo and Next and deploy it to GitHub Pages.

### Install Node.js & npm with Homebrew

```bash
brew install node
```

### Install Hexo

```bash
npm install -g hexo-cli
```

<!-- more -->

### Create New Project

```bash
$ hexo init blog
INFO  Copying data to ~/Documents/blog
INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!
```

Following the instruction，run `npm install`

```bash
cd blog
npm install
```

### Setup Theme - [Next](https://github.com/theme-next/hexo-theme-next)

```bash
cd blog
git submodule add https://github.com/theme-next/hexo-theme-next themes/next
```

It is a little different from official document. It will be helpful in the next step([【Hexo】2. How to Deploy Hexo Project with Github Actions](https://achillessatan.github.io/posts/16107/)).

Change the `theme` in `./config.yml` to `next`.

```yml
theme: next
```

### Start Server on Local

```bash
hexo clean && hexo g && hexo s
```

### Deployment

Create a repository on GitHub with the name of `{USERNAME}.github.io`:

![GitHub](/images/20200314-how-to-setup-hexo-blog.jpg)

Install the plugin of deployment:

```bash
npm install hexo-deployer-git --save
```

Add the config of deployment in `./_config.yml`

```yml
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github
  repo: git@github.com:xxxx/xxxx.github.io.git
  branch: master
```

Excute the command of deployment：

```bash
hexo deploy -g
```

Now you can access to your own blog by `https://{USERNAME}.github.io`.

### Others

#### Plugins Recommended

- [Abbrlink](https://github.com/rozbo/hexo-abbrlink)

```bash
npm install hexo-abbrlink --save
```

```yml
Plugins:
  - hexo-abbrlink
```

- Sitemap

```bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

```yml
# Extensions
Plugins:
  - hexo-generator-sitemap
  - hexo-generator-baidu-sitemap

#sitemap
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```

- Feed

```bash
npm install hexo-generator-feed --save
```

```yml
# Extensions
Plugins:
- hexo-generator-feed

#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
```

#### Add `robots.txt` in `source` folder

```txt
# hexo robots.txt
User-agent: *
Allow: /
Allow: /archives/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://{USERNAME}.github.io/sitemap.xml
Sitemap: https://{USERNAME}.github.io/baidusitemap.xml
```

#### Comman commands

```bash
hexo new "postName" # Next Post
hexo new page "pageName" #New Page
```
