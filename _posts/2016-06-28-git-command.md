---
title:      Git 常用命令
summary:    记录常用的 Git 命令，方便查询。
categories: Git
tags:       Git
---

- `git init`
- `git add`
- `git add --all` 
- `git commit -m "<Commit Message>"`
- `git status` 
- `git diff`
- `git reset --hard HEAD^/~n/<commitid>` 版本回退 
- `git revert HEAD/<commitid>` 撤销提交，并生成一次新的提交
- `git reflog` 查看命令历史
- `git checkout --<filename>` 取消工作区的修改/从版本库恢复删除的文件到最新版本
- `git checkout <branchname>` 切换分支
- `git reset HEAD <filename>` 将暂存区回退到工作区
- `rm <filename>` 从文件管理器中删除文件
- `git rm <filename>` 从版本库中删除文件(记得 git commit)
- `git remote add origin git@server-name:path/repo-name.git` 关联远程库
- `git push -u origin <branchname>` 第一次推送 master 分支的所有内容
- `git branch` 列出所有分支
- `git push origin <branchname>` 将本地更新推送到远程主机
- `git pull origin <branchname>` 从远程主机拉取最新文件