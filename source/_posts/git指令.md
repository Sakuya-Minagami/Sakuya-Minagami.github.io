---
title: Git指令
---
[一些报错](https://zhuanlan.zhihu.com/p/435185713)
[一些git问题](https://zhuanlan.zhihu.com/p/423632361)
[git多账号](https://blog.csdn.net/weixin_42139800/article/details/122100628)
[gitlab机制](http://www.360doc.com/content/17/0811/18/46248428_678467141.shtml)
### 到根目录打开git-bash
### 设置用户名和邮箱
```
git config --global user.name "用户名"
git config --global user.email "邮箱"
```
### 初始化
```
git init
```
### 新建kh.md文档并写入备注qwq
```
echo "qwq" > kh.md
```
### 添加文件到工作区
```
git add kh.md
```
### 提交文件到本地仓库
```
git commit
git commit -a //跳过添加直接提交
git commit -m "qwqq" //附带备注信息
```
之后进入vim编辑器
操作：插入，结束插入，退出
```
i
esc
:wq enter
```
### 查看版本
```
git log
```
### 创建.gitignore
```
touch .gitignore
```
在.gitignore写拒绝追踪的文件名加后缀即可
### 新建分支，查询分支，切换分支，删除分支,合并分支
```
git branch sakuya // git branch -b sakuya
git branch
git checkout sakuya
git branch -d sakuya //用D时不提示直接删除
git merge sakuya //合并到当前分支
```
# 成功结果
```shell
$ git push -u origin MAR-CO
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 12 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 423 bytes | 423.00 KiB/s, done.
Total 5 (delta 4), reused 0 (delta 0), pack-reused 0
remote:
remote: ========================================================================
remote:
remote:                    涉密不上网，上网不涉密
remote:
remote: ========================================================================
remote:
remote:
remote: To create a merge request for MAR-CO, visit:
remote:   https://git.hit.edu.cn/hitwhoj/hitwhfp/-/merge_requests/new?merge_request%5Bsource_branch%5D=MAR-CO
remote:
To https://git.hit.edu.cn/hitwhoj/hitwhfp.git
   a7a16f6..bf40338  MAR-CO -> MAR-CO
branch 'MAR-CO' set up to track 'origin/MAR-CO'.

$ git push -u origin MAR-CO -f
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 12 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (12/12), 2.46 KiB | 838.00 KiB/s, done.
Total 12 (delta 8), reused 0 (delta 0), pack-reused 0
remote:
remote: ========================================================================
remote:
remote:                    涉密不上网，上网不涉密
remote:
remote: ========================================================================
remote:
remote:
remote: To create a merge request for MAR-CO, visit:
remote:   https://git.hit.edu.cn/hitwhoj/hitwhfp/-/merge_requests/new?merge_request%5Bsource_branch%5D=MAR-CO
remote:
To https://git.hit.edu.cn/hitwhoj/hitwhfp.git
 + bf40338...d5ba1dd MAR-CO -> MAR-CO (forced update)
branch 'MAR-CO' set up to track 'origin/MAR-CO'.

```
# 遇事不决重建仓库
```
git init
git add .
git commit -m "test"
git remote add orgin ssh
git push -u origin master
```
## 备用
``git remote rm origin``删除远程仓库分支
``rm -rf .git``取消git初始化
# 配置两个账号的ssh
```bash
$ ssh-keygen -t rsa -C "ARO-CO"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/matebook/.ssh/id_rsa):
/c/Users/matebook/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/matebook/.ssh/id_rsa
Your public key has been saved in /c/Users/matebook/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:m4jziqPxmJUZJov7c2yELnGxjdHIa/bGKqblfyzTaGg ARO-CO
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|                 |
| . o             |
|  = .            |
|. oO    S        |
|o+O+o. . o       |
|+*=*o+. o        |
|o@E.%o+          |
|O**Xo=.          |
+----[SHA256]-----+

```
成功案例
[流程](https://zhuanlan.zhihu.com/p/423632361)
[图文样例](https://www.likecs.com/show-204219700.html#sc=400)
# 一些报错
## 本地分支过旧
```
error: failed to push some refs to ‘http://xxx/backend.git’
hint: Updates were rejected because a pushed branch tip is behind its remote
hint: counterpart. Check out this branch and integrate the remote changes
hint: (e.g. ‘git pull …’) before pushing again.
hint: See the ‘Note about fast-forwards’ in ‘git push --help’ for details.
```
## Everything up-to-date
[简书](https://www.jianshu.com/p/26f4dcfaea8e)
```
git branch dev// 创建新分支

gti add .
git commit -m '修改'
git merge dev
git push -u origin master
git checkout dev
```
## Your branch and have diverged
本地分支和远端分支冲突，就硬推
```
git push origin branch_xxx -f
```