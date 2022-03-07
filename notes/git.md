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

#### origin的意思

你clone本地仓库的时候被clone的远端仓库默认被称为 origin。 所以如果你想向/从这个远端仓库push/pull 的时候，用 origin 指代这个远端仓库。

#### 配置当前的用户操作信息

git config --global user.email 'pipixia@pipi.com'
git config --global user.name '皮皮虾'

#### 删除之前配置好的用户信息

git config --global --unset user.email 'pipixia@pipi.com'
git config --global --unset user.name  '皮皮虾'

#### 更新远程分支

git remote update origin --prune

#### 第一次提交

```js
git init 初始化
gid add .
git commit -m 项目初始化
git remote add origin https://github.com/zhuangjiasheng123/codeSave.git
git push -u origin 分支名称
```

#### 每次提交都需要输入账号密码问题

git config --global credential.helper store

#### 本地分支与远程分支关联

如果当前分支与多个主机存在追踪关系，那么git push --set-upstream origin **master**（省略形式为：**git push -u origin** **master**）将本地的master分支推送到origin主机（--set-upstream选项会指定一个默认主机），同时指定该主机为默认主机，后面使用可以不加任何参数使用git push

#### 忽略某个文件的提交

git update-index --assume-unchanged /path/to/file

#### 取消忽略某个文件的提交

git update-index --no-assume-unchanged /path/to/file

#### git stash使用

**git stash stash@{1}**

恢复git栈中最新的一个stash@{num}

**git stash pop stash@{1}**

删除指定的一个进度，默认删除最新的进度

**git stash drop stash@{1}**

恢复被隐藏的文件，但是git栈中的这个不删除

**git stash apply stash@{1}**

删除所有存储的进度

**git stash clear**

查看所有被隐藏的文件列表

**git stash list**

#### 修改提交

git commit --amend -m "新的修改提交信息"

#### git merge origin master和git merge origin/master的区别

git merge branchA branchB, branchB 一般默认为当前branch，所以

```js
git merge origin master //将origin merge 到 master 上
git merge origin/master //将origin上的master分支 merge 到当前 branch 上
```

一般进行merge操作时，最好先checkout到你希望进行merge操作的分支，也就是branchB上，然后再进行

```awk
git merge branchA //默认为当前branch，即branchB
```