---
title: 如何使用 hexo 更新博客
date: 2022-07-23 15:43:03
tags:
---

1. clone 远端仓库
```
git clone https://github.com/plumrx/plumrx.github.io.git
----------------------------
正克隆到 'plumrx.github.io'...
remote: Enumerating objects: 199, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 199 (delta 8), reused 0 (delta 0), pack-reused 184
接收对象中: 100% (199/199), 694.04 KiB | 1.90 MiB/s, 完成.
```

2. 安装 yarn 依赖
进入 clone 好的文件目录下
```
cd plumrx.github.io
yarn install
-------------------------------
yarn install v1.22.10
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 🔨  Building fresh packages...
✨  Done in 12.77s.
```

3. 启动 hexo
```
yarn hexo s
----------------------------
yarn run v1.22.10
$ /Users/plumrx/projects/github-hexo-blog/plumrx.github.io/node_modules/.bin/hexo s
(node:95871) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:95871) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:95871) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:95871) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:95871) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:95871) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```


4. 新建文章  
可以在其中直接进行编辑。
```
yarn hexo new "狗屁不通文章生成器"
-----------------------------
yarn run v1.22.10
$ /Users/plumrx/projects/github-hexo-blog/plumrx.github.io/node_modules/.bin/hexo new 狗屁不通文章生成器
(node:96103) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:96103) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:96103) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:96103) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:96103) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:96103) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
INFO  Created: ~/projects/github-hexo-blog/plumrx.github.io/source/_posts/狗屁不通文章生成器.md
✨  Done in 1.31s.
```

5. 查询当前 git 状态
```
git status
位于分支 source
您的分支与上游分支 'origin/source' 一致。

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
	"source/_posts/\347\213\227\345\261\201\344\270\215\351\200\232\346\226\207\347\253\240\347\224\237\346\210\220\345\231\250.md"

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪） 
```


6. 新增日志文件至 index
```
git add .
```


7. 打包一条 commit
```
git commit -m '新增一篇博客'
-------------------
[master 9f98b6b] 新增一篇博客
 1 file changed, 4 insertions(+), 4 deletions(-)
```

8. 上传代码至远端服务器
```
git push
-----------------
枚举对象中: 5, 完成.
对象计数中: 100% (5/5), 完成.
使用 8 个线程进行压缩
压缩对象中: 100% (3/3), 完成.
写入对象中: 100% (3/3), 396 字节 | 396.00 KiB/s, 完成.
总共 3（差异 1），复用 0（差异 0），包复用 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:plumrx/gou-pi-bu-tong.git
   8310462..9f98b6b  master -> master

```


9. 使用 hexo 编译  
将 markdown 文件转为 html 文件，，存放在 public/index.html 之下.
```
yarn hexo generate
----------------------
yarn run v1.22.10
$ /Users/plumrx/projects/github-hexo-blog/plumrx.github.io/node_modules/.bin/hexo generate
INFO  Validating config
INFO  Start processing
INFO  Files loaded in 76 ms
INFO  Generated: 2022/07/22/狗屁不通文章生成器/index.html
INFO  1 files generated in 189 ms
✨  Done in 1.06s.
```

10. 发布 blog  
发布至远端服务器，git 服务器非实时部署，查看需等待几分钟
```
yarn hexo deploy  
---------------------
yarn run v1.22.10
$ /Users/plumrx/projects/github-hexo-blog/plumrx.github.io/node_modules/.bin/hexo deploy
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
[master 9e30395] Site updated: 2022-07-22 23:49:51
 1 file changed, 68 insertions(+), 21 deletions(-)
枚举对象中: 13, 完成.
对象计数中: 100% (13/13), 完成.
使用 8 个线程进行压缩
压缩对象中: 100% (4/4), 完成.
写入对象中: 100% (7/7), 2.07 KiB | 2.07 MiB/s, 完成.
总共 7（差异 2），复用 0（差异 0），包复用 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: 
remote: GitHub found 2 vulnerabilities on plumrx/plumrx.github.io's default branch (1 high, 1 moderate). To find out more, visit:
remote:      https://github.com/plumrx/plumrx.github.io/security/dependabot
remote: 
To github.com:plumrx/plumrx.github.io.git
   8e75e61..9e30395  HEAD -> master
分支 'master' 设置为跟踪来自 'git@github.com:plumrx/plumrx.github.io.git' 的远程分支 'master'。
INFO  Deploy done: git
✨  Done in 3.83s.
```
