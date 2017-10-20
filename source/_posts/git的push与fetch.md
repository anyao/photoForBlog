---
title: git的push与fetch
date: 2017-08-27 20:50:06
tags:
    - git
    - 备忘
---
1、push
首次：
```
git push -u origin master
```
日常：
```
$ git push origin master
```
<!-- more -->
2、fetch
```
$ git remote -v                 // 查看远程分支
$ git fetch origin master:temp      // 从远程获取最新版本到本地
$ git diff temp                 // 比较本地仓库与下载的temp分支
$ git merge temp                    // 合并temp分支到本地的master分支
$ git branch -d temp                // 删除分支
```

3、放弃本地修改 强制更新
```
$ git fetch --all
$ git reset --hard origin/master
```

----------
**git 备忘：**
1、有关远程库
```
$ ssh-keygen -t rsa -C "youremail@example.com"      // 新机创建ssh --id_rsa.pub
$ git remote add origin git@github.com:annyao/repo.git  // 关联远程库
$ git clone git@github.com:annyao/repo.git
```

2、其他
[https://www.liaoxuefeng.com/wiki/](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[http://www.cnblogs.com/xueweihan/](http://www.cnblogs.com/xueweihan/p/5703144.html#%E5%B1%95%E7%A4%BA%E6%89%80%E6%9C%89%E7%9A%84%E5%88%86%E6%94%AF%E5%85%B3%E8%81%94%E7%9A%84%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93)