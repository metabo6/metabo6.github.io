---
title: Git 总结
tags: Git
abbrlink: f1991798
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/git.webp
---

# Git 总结

是什么？

- 分布式版本管理工具

SVN：集中式版本管理工具



## 分布式 VS 集中式

分布式：

- 每个用户都在本地拥有一个完整的仓库

  优点：

  - 如果任意服务器崩溃了，可以立即从用户手中 clone 一份完全一样的代码仓库出来，不用担心服务器崩溃
  - 可以本地提交

集中式：

- 由一个代码服务器集中管理代码

  优点：

  - 代码必须存储在服务器上，如果没有服务器则无法提交和更新，代码安全性高

  缺点：

  - 如果服务器崩了，就废了



结论：前端领域更多的使用 Git 而不是 SVN。



## Git 基本操作

### 初始化仓库到推送至远程

1. 初始化仓库

   在当前目录下生成一个 .git 目录

   当前目录下所有的文件都处于 Untracked 未追踪的

   ```bash
   git init
   ```

   

2. 添加至暂存区

   将当前目录所有文件都添加到暂存区

   ```bash
   git add .
   ```

   

3. 提交至本地仓库

   ```bash
   git commit -m "提交日志"
   ```

   

4. 推送至远程仓库

   1. 在 Gitee 或 Github 等平台创建一个远程仓库

   2. 配置 SSH Keys

   3. 关联远程仓库

      ```bash
      # 添加关联远程仓库
      git remote add origin 仓库地址
      # 删除关联远程仓库
      git remote remove origin
      # 查看关联的远程仓库
      git remote -v
      ```

      

   4. 推送至远程仓库

      ```bash
      git push -u origin 分支名
      ```

      

### 查看日志及版本管理

1. 查看日志

   ```bash
   git log
   git log --oneline
   git log -3 # 查看最近三次提交记录
   ```

2. 版本回退

   ```bash
   # --hard 慎用, 会清除所有未提交的代码 (代码只要未提交就无法还原)
   git reset --hard CommitID
   # --soft 不会清除未提交的代码
   git reset --soft CommitID
   ```

   - 需求：我辛辛苦苦写了 300 多行 bug，想全部删除该怎么办？


   - 解决方案：

     ```bash
     # HEAD~0 表示最新版, HEAD~1 表示上一个版本, 以此类推
     git reset --hard HEAD~0
     ```

3. 撤销版本回退

   ```bash
   # 查看操作日志
   git reflog
   ```

### 常用命令

- git  init ：项目初始化
- git  add  . ：所有文件添加到暂存区
- git  commit  -m  "XXXX"：暂存区文件添加到本地仓库
- git  status  -s：查看所有文件状态
- git  log/reflog  -n  --oneline：查看提交日历;
- git  reset  --hard  ID：版本切换;
- git branch：查看本地全部分支
- git branch -m oldmaster newmaster：修改分支名，前面是旧分支名，后面是新的
- git branch XXX：创建分支（根据主分支创建）
- git checkout XXX：切换分支
- git checkout -b XXX：创建并切换分支
- git merge XXX：主分支合并功能分支
- git branch -D XXX：删除本地分支
- git  remote  add  origin   https/ssh地址：添加变量存储仓库地址
- git  push  -u  origin  master：推送给远程仓库分支 （第一次推送需要加 -u）
- git  remote  -v ：查询变量中存储的地址
- git  remote  remove  origin ：删除已关联的远程仓库地址
- ssh-keygen -t rsa -b 4096 -C "邮箱地址"：生成本地 SSH 文件
- ssh -T git@gitee.com：检测 SSH 是否绑定成功
- git clone  SSH/HTTPS地址：克隆仓库默认分支
- git  pull  origin  分支名称：拉取远程仓库分支里面最新的代码
- git  remote  show  origin：查看远程仓库分支
- git  checkout  远程分支：跟踪分支(主分支拉，跟踪分支拉取在切换)
- git clone -b 分支名 分支地址：克隆该仓库指定分支，且会在本地创建同名分支





## 代码提交流程

前提：一般情况下，不允许向主分支 push

工作环境的分支：

- master（线上稳定版本的代码）
- dev（开发分支）
- staging（测试服务器分支）
- login（业务分支...）

既然不能向主分支 push 代码，如何将代码合并到对应的分支呢？

解决方法：PR （Pull Request）



PR 提交流程

1. 在本地新建一个分支，开始写代码
2. 功能完成后，提交到本地
3. 将分支推送至远程仓库
4. 去远程仓库新建一个 PR
5. 邀请你的老大来 review (检查) 你的代码
6. 老大觉得代码通过了，并测试完成后，就同意合并到目标分支




提交代码的日志规范：

1. feat：新增功能
2. fix：修复 Bug
3. docs： 修改文档
4. chore： 添加或修改依赖库（yarn add / yarn remove）
5. style：修改代码风格
6. refactor： 重构
7. perf： 性能优化、体验优化
8. test： 测试用例
9. build： 添加一些打包需要用的依赖



提交日志的模板：

```
📃 docs(Git): 新增了代码提交规范

写了很多规范名词的释义
```

