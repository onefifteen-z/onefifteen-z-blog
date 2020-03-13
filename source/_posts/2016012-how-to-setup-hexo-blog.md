---
title: 【Hexo】Setup your own blog on GitHub
tags:
  - GitHub
  - Hexo
categories:
  - Development
abbrlink: 54991
date: 2016-01-27 19:47:21
---

### 通过 Homebrew 安装 Node.js 和 npm

```bash
$ brew install node

$ node -v
v4.2.0
```

### 安装 Hexo

```bash
$ npm install -g hexo-cli

$ hexo -v
hexo-cli: 0.1.9
os: Darwin 14.5.0 darwin x64
http_parser: 2.5.0
node: 4.2.0
v8: 4.5.103.35
uv: 1.7.5
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 56.1
modules: 46
openssl: 1.0.2d
```

<!-- more -->

### 建立项目

```bash
$ hexo init blog
INFO  Copying data to ~/Documents/blog
INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!
```

按照提示，运行 npm instal

```bash
cd blog
npm install
```

### 运行服务器

```bash
hexo server
```

### 设置主题

本文以设置NexT为例
<https://github.com/iissnan/hexo-theme-next>

```bash
cd blog
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

将_config.yml配置文件中的theme属性设置为next

```yml
theme: next
```

### 安装插件

- sitemap插件

```bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

```json
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

- 在source文件夹添加蜘蛛协议robots.txt

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

Sitemap: http://www.blog.site/sitemap.xml
Sitemap: http://www.blog.site/baidusitemap.xml
```

- feed插件

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

### 部署到GitHub Pages

```bash
npm install hexo-deployer-git --save
```

编辑_config.yml中development的部分

```yml
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github
  repo: git@github.com:xxxx/xxxx.github.io.git
  branch: master
```

本地测试：

```bash
hexo s -g
```

部署：

```bash
hexo d -g

# hexo generate -deploy

# hexo clean
# hexo generate
# hexo deploy
```

### 常用命令

```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```
