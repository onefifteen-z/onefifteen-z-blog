---
title: 【Hexo】2. How to Deploy Hexo Project with Github Actions
tags:
  - GitHub
  - GitHub Action
  - Hexo
categories:
  - Development
  - Hexo
abbrlink: 16107
date: 2020-03-14 20:07:21
---
### Quick Start

In the last blog [【Hexo】1. How to Setup Blog with Hexo and Next on GitHub Pages](https://achillessatan.github.io/posts/54991/), we learned how to setup up a blog web with Hexo and deploy it from local to GitHub Pages.

In this blog, it will talk about how to deploy you blog automatically with the CI tool `Github Action`.

### Prepare the Config File of the Theme

As metioned in the last blog, we are now using the `github submodule` to manage the theme `Next`. The submodule can be initialized on Github Action with command `git submodule update --init`. However, we will lose the changes we have made in the config file  `themes/next/_config.yml`. There are several ways to resolve this:

- Fork the `Next` in our repository and commit the changes of the config. But we will need to do something more when `Next` is updated.
- Copy all the things into the main config file `_config.yml`. This will make the `_config.yml` a little confusing.
- Backup the config file in the main repository and overwrite the original one in the Github Action.

<!-- more -->

### Set `DEPLOY_KEY`

Copy the SSH key to `DEPLOY_KEY` in secrects of Github Repository

```bash
cat ~/.ssh/id_rsa
```

![GitHub](/images/20200314-how-to-deploy-hexo-with-github-action-1.jpg)

### Workflow Config File

`.github/workflows/deploy.yml`

```yml
name: Hexo Deploy

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - run: mkdir -p ~/.ssh/
      - run: echo "${{secrets.DEPLOY_KEY}}" > ~/.ssh/id_rsa
      - run: chmod 600 ~/.ssh/id_rsa
      - run: ssh-keyscan github.com >> ~/.ssh/known_hosts
      - run: git config --global user.name '{USERNAME}'
      - run: git config --global user.email '{EMAIL}'
      - run: npm ci
      - run: npm i -g hexo-cli
      - run: git submodule update --init
      - run: cp deploy/_config.yml themes/next/_config.yml
      - run: hexo deploy -g
```

### Check the Result of Deployment

The action will be triggered when `master` branch was pushed. After that, we can check the result of the action in the `Action` tab of the repository.

![GitHub](/images/20200314-how-to-deploy-hexo-with-github-action-2.jpg)
![GitHub](/images/20200314-how-to-deploy-hexo-with-github-action-3.jpg)
