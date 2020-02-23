---
title: 改为由Travis CI部署
date: 2020-02-23 15:22:39
tags:
categories:
---

突然想起来用 [Travis CI](https://travis-ci.com/) 来替换掉之前用的[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git), 正好避免了源文件的管理混乱~~虽然本来也没几篇文章~~。

网上很多老资料用的[Custom Deployment](https://docs.travis-ci.com/user/deployment/custom/)的方法，现在Travis CI 已经支持GitHub Pages，只需要简单的配置一下即可。

Hexo提供了[设置指南](https://hexo.io/docs/github-pages)以及`.travis.yml`的配置例子，当设置好后每次提交源文件到master分支 TravisCI会自动生成页面并且部署到gh-pages分支上。

```yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```

需要注意的是该方法不适用于仓库名为`<user>.github.io`这种格式的仓库，因为GitHub 必须从master分支生成 user和organization的页面而不是gh-pages。

> User pages must be built from the `master` branch.

因此如果用的是personal或者需要修改一下官方文档上的yml文件配置。

现在Travis CI默认的依旧是dpl v1，但是在log里又会按照dpl v2 的指示弹警告，像

> deploy: deprecated key  skip_cleanup (not supported in dpl v2, use cleanup)

`cleanup`在 dpl v2中默认是设置为 false 的而 dpl v1 中仍需要手动加入`skip_cleanup: true`来避免生成目录被删除。

还有`sudo: false`[已被弃用](https://blog.travis-ci.com/2018-11-19-required-linux-infrastructure-migration)，`github_token`已经被替换为`token`等部分 dpl v2的属性又已经被支持

在基于目前的新文档的建议下，最后的`.travis.yml`设置如下

```yaml
os: linux
language: node_js
node_js:
  - 10
cache: npm
branches:
  only:
    - source # store source code of hexo in source branch
script:
  - hexo generate
deploy:
  provider: pages
  skip_cleanup: true
  token: $GH_TOKEN
  keep_history: true
  target_branch: master
  on:
    branch: source
  local_dir: public
```

