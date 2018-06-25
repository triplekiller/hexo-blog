---
title: Update Hexo Theme
date: 2018-01-24 15:26:59
tags: [hexo]
categories: Network
---

热门的Hexo主题一般更新很快，我们可以定期将自己感兴趣的或者影响较大的patch合入到自己的主题仓库中，具体步骤如下：

### Sync a fork

按照[GitHub帮助文档](https://help.github.com/articles/syncing-a-fork/)，将自己的[Hexo主题仓库](https://github.com/triplekiller/hexo-theme-indigo.git)的tracked分支更新到最新。

``` bash
$ cd themes/indigo
$ git remote add upstream https://github.com/yscoder/hexo-theme-indigo.git
$ git fetch upstream card
$ git checkout -t origin/card
# Use "git merge upstream/card" if do not want to rewrite the history of card
$ git rebase upstream/card
$ git push origin card
```

### Merge dev branch

同步完tracked分支后，将tracked分支的改动merge到自己的开发分支上，可以使用git merge/rebase/cherry-pick等方法，一般使用git rebase。

``` bash
$ git checkout card-triplekiller
$ git rebase card
# Resolve conflicts and then run "git rebase --continue"
$ git push origin card-triplekiller
$ cd ../../
# Bump up submodule revision
$ git add themes/indigo
$ git commit -s -m "indigo: Merge upstream commits"
$ git push origin master
```

### Git repos for Hexo blog

| Repo        | URL |
| ---         | --- |
| hexo source | https://github.com/triplekiller/hexo-blog.git |
| hexo site   | https://github.com/triplekiller/triplekiller.github.io.git |
| hexo theme  | https://github.com/triplekiller/hexo-theme-indigo.git |
