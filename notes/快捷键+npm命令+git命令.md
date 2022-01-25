## npm语法

```js
npm是什么
npm install 安装模块
npm uninstall 卸载模块
npm update 更新模块
npm outdated 检查模块是否已经过时
npm ls 查看安装的模块
npm init 在项目中引导创建一个package.json文件
npm help 查看某条命令的详细帮助
npm root 查看包的安装路径
npm config 管理npm的配置路径
npm cache 管理模块的缓存
npm start 启动模块
npm stop 停止模块
npm restart 重新启动模块
npm test 测试模块
npm version 查看模块版本
npm view 查看模块的注册信息
npm adduser  用户登录
npm publish 发布模块
npm access 在发布的包上设置访问级别
npm package.json的语法
 
```

## git

```js
//常用的操作命令
1. git  init      在要被管理的项目的根目录下使用此命令,会生成一个.git的隐藏文件
2. git status     用来查看当前文件的状态 
3. git  add   文件名称    用于将文件从工作区提交到暂存区 	
			git add *   提交所有的文件
			git add all 提交所有的文件
			git add -A  提交所有的文件
4. git  commit  -m  日志记录   将文件从暂存区提交到本地仓库，将文件进行托管
5. git  log    查看提交日志,也就是查看做了哪些操作
6. git reset --hard 日志版本号   可以回退到任何一个历史版本
7. git log  --oneline  以精简的方式来查看操作记录
8. git reflog 查看所有的操作日志，包括回退的操作记录也会查看的到
修改git密码  git config  再输入一次新密码
查看用户名和邮箱地址  git config user.name  git config user.email
```

```js
配置当前的用户操作信息，使用如下代码：
git config --global user.email 'pipixia@pipi.com'
git config --global user.name '皮皮虾'

删除之前配置好的用户信息：
git config --global --unset user.email 'pipixia@pipi.com'
git config --global --unset user.name  '皮皮虾'
```

侧分支的创建与删除

```js
git branch  分支名称    表示在当前分支下面创建一个新的分支
git branch -r 查看远程分支
git branch -a 查看本地分支和远程分支
git  branch  -d | -D  branchname   删除branchname分支。D表示强制删除
git checkout 分支名称   表示在当前分支切换到另一个分支 
git checkout -b 分支名称   表示在当前分支创建一个侧分支主直接切换到新创建的分支下面

git checkout  master  切换回主分支
git merge dev   是将dev分支合并到当前分支
git  branch -d 分支名称   表示删除侧分支
git pull origin master  子分支中拉取主分支代码
 git branch --set-upstream-to=origin/hxh_dev sm_dev   本地分支与远程分支的关联
 git remote remove origin 解除本地分支与远程分支的关联
 git push origin 分支名称    提交到远程分支，远程分支的名称与本地名称相同
```

将本地初始化好的文件提交上远程服务器

```js
1. 在某个要提交的项目的根目录下使用命令 git init  先进行版本控制的初始化，会生成一个.git的隐藏文件
2. git  add *  将此项目的所有文件提交到暂存区
3. git commit -m  日志记录  将暂存区的文件提交到仓储区 
4. 有可能会出现配置信息 
	 git config --global user.email 'pipixia@pipi.com'
	 git config --global user.name  '皮皮虾'
	 
5. git push https://github.com/wangsk69/qianduan32.git maste 提交到远程仓库	
```

### 一般最常用

```js
//在github上新建一个仓库
//第一次提交
gid add .
git commit -m 项目初始化
git remote add origin https://github.com/zhuangjiasheng123/codeSave.git
git push -u origin master

//接下去的提交
git pull 
gid add .
git commit -m 日志
git push

git reflog   //查看日志 最强大
git reset --hard HEAD^     //还原到上一个版本
git reset --hard  版本号        //还原到具体的某个版本

git checkout .  //还原样式
```



## 常用的几个终端操作命令

```
touch  index.html   创建了一个名称为index.html的文件

touch  css/index.css   images/a.gif   是在当前对应的文件夹中创建对应的文件，前提是文件夹必须存在
```

```
rm -r  文件夹名称   递归删除文件夹，就是先删除此文件夹中的文件，再来删除空文件夹
```

## 关于git的操作

```
git init 初始化
git add . 
git commit -m
git remote add origin +远程仓库地址
git push -u origin master 将本地的master分支推送到origin主机的远程master分支，同时指定origin为默认主机，后面就可以不加任何参数使用git push了
分支

 git branch -a 查看所有分支
 git branch    查看当前使用分支

git branch dev  新建本地分支dev
git checkout dev切换到A分支
git checkout -b dev 新建远程分支
git push origin dev:dev 本地dev推送到远程dev分支

删除本地分支：git branch -d 分支名（remotes/origin/分支名）
强制删本地：git branch -D 分支名
删除远程分支：git push origin --delete 分支名（remotes/origin/分支名）

git push --set-upstream origin A 
新增对应的远程分支并且push到里面，这个远程分支可以是与A分支同名，也可以是主分支或其他分支。此时A分支没有关联其他的分支，也不会影响到msater
git branch --set-upstream-to=origin/其他分支名称 A
上面语句是两个分支的相关连，相关连后A分支push的时候需要选择提交到哪个远程分支

git remote rm origin  删除远程 Git 仓库
git remote add origin git@github.com:FBing/java-code-generator 添加远程 Git 仓库
git push origin 远程分支  提交到远程分支
```

