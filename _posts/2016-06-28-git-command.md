---
layout:     post
title:      Git 常用命令
summary:    记录常用的 Git 命令，方便查询。
categories: life
---

- `git init`
- `git add`
- `git add --all` 
- `git commit -m"summary"`
- `git status`
- `git diff`
- `git reset --hard HEAD^/~n/commitid`版本回退
- `git reflog` 查看命令历史
- `git checkout file` 取消工作区的修改/从版本库恢复删除的文件到最新版本
- `git reset HEAD file` 将暂存区回退到工作区
- `rm file` 从文件管理器中删除文件
- `git rm file` 从版本库中删除文件(记得 git commit)
- `git remote add origin git@server-name:path/repo-name.git` 关联远程库
- `git push -u origin master` 第一次推送 master 分支的所有内容
- `git branch` 列出所有分支
- `git push origin master` 将本地更新推送到远程主机
- `git pull origin master` 从远程主机拉取最新文件