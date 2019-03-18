# 说明
练习 Git 相关操作，详细教程请参考 [廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
## 准备工作
安装 Git，可到官网：https://git-scm.com/ 下载

## 一 初始化 Git 库
在需要进行版本的目录下执行以下命令：
```
git init
```
即可创建一个Git版本库，成功后有如下提示：
```
Initialized empty Git repository in G:/git-test/.git/
```
说明已经创建一个空的“仓库”，现在就可以开始使用了。

## 二 添加文件和提交
当文件修改完成后，需要进行两个步骤
1. 添加文件
```
git add README.MD
```
2. 提交当前修改
```
git commit -m "添加一个说明文件"
```
不管是新增文件和修改文件内容，都被视为“修改”。

## 暂存区
当使用 git commit 命令时，是将“暂存区”内容提交至仓库；那么 git add 的作用就是将需要的文件添加至“暂存区”，有人将“暂存区”比喻成“购物车”，相当的形象！

## 三 创建分支
通常情况下，git 创建的仓库会有一个默认分支：master，一般的操作都在这个分支下进行；
但平时需要添加功能或需要对一个成熟功能进行升级时，需要创建一个分支来进行操作；
可使用以下命令快速创建并切换到一个分支
```
git checkout -b dev
```
相当于两条命令
```
git branch dev
git checkout dev
```
可使用 git branch 命令查看当前正在使用的分支

## 四 合并分支
当在分支下内容修改完成后，可以将当前内容合并到主分支（master）
```
git checkout master
git merge dev
```
在进行合并操作时，需要先切换到 master 分支，再进行合并操作。
在不使用其他参数的情况下，git 会使用 Fast forward 模式进行合并，这种模式在删除分支后会丢失分支信息，此时可以采用以下方式：
```
git merge --no-ff -m "提交内容描述" dev
```
即增加一个 --no-ff 参数来禁用 Fast forward 模式，-m 的意思是合并后直接提交。

## 五 解决冲突
通常情况，可以直接对分支进行合并操作，但也有不少情况是不能直接合并的，比如不同时间不同用户修改了同一个文件，此时如果尝试合并时，可能会出现冲突，即git不知道以哪个版本的文件为准，所以需要我们进行手工合并。

git 会以 `<<<<<<<` `=======` `>>>>>>>` 三种标识来标记冲突的位置，此时需要人工合并后保存提交。

## 六 工作现场 stash
git 的 `stash` 功能可以将当前工作现场“暂存”起来，从而去做一些其他修改，如修复一个紧急BUG

保存现场
```
git stash
```
查看当前已“暂存”的现场列表
```
git stash list
```
恢复已“暂存”的现场
```
git stash pop
```
## 七 BUG管理
可以通过创建临时分支的办法来对BUG进行修复和管理，解决完BUG后，提交合并，然后删除临时分支。

```
git checkout master
git merge --no-ff -m "合并已修复的BUG #1" issue-1
git branch -d issue-1
```

## 八 标签管理
git 可以在任何时间对任何提交打上标签，标签的好处的方便记忆，他的作用和 commit id（就是每次提交时，git生成的一串长长的字母）一样，可以理解为别名；通常用于版本号管理，如 v0.1 发布，会打一个 v0.1 的标签。
1. 创建标签
```
git tag v1.0
```
默认标签是打在最新提交的commit上，但有时候可能需要对指定的 commit 进行标签，可以使用以下命令：
```
git log --pretty=oneline --abrev-comit
```
找到需要打标签的commit id，如 6972298，然后使用：
```
git tag -a v0.1 -m "标签说明测试：版本 0.1 发布" 6972298
```
`-a` 是添加标签，`-m` 是给标签加上说明，最后是 `commid id`。

同时git 还支持“私钥”标签，详细方法可参考：[标签管理->创建标签](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001376951758572072ce1dc172b4178b910d31bc7521ee4000)

2. 查看标签
* `git tag` 可以列出当前所有标签
* `git show v0.1` 可以列出指定标签的详细内容

3. 同步标签至远程库
默认情况下，标签不会自动同步（推送）至远程库，需要手工同步：
```
git push origin v1.0
```
也可以将本地所有的标签推送至远程库：`git push origin --tags`

4. 删除标签
在标签没有同步至远程库之前，可以使用 `git tag -d v0.1` 来删除指定的标签；

但如果标签已同步至远程库，那么除了删除本地标签外，还得将远程库删除：
```
git push origin :refs/tags/v0.9
```

## 高级应用
1. cherry-pick
可以将某一个 commit 合并到当前分支，如
```
git checkout dev
git cherry-pick 123456 # 假设 123456 为 master 下的一个commit
```
进行以上操作后，commit 123456 的内容将会合并到 dev 分支，并生成一个新的提交

2. 检出指定分支下的某个文件或某个目录下所有文件
```
git checkout 分支名称 -- "目录名称/文件名称"
```
如：
```
git checkout master -- "path1/*"
git checkout dev -- "path 2/file.name"
```
目录之所以用引号，是因为目录中如果包含空格的话用引号不会出错。

3. 以服务器内容强制覆盖本地内容
```
git fetch --all
git reset --hard origin/master
git pull origin master --allow-unrelated-histories
```
`--allow-unrelated-histories` 参数可避免因为删除了历史记录中的一些没用的文件后无法合并的错误，提示为：`fatal: refusing to merge unrelated histories`
## 完结
掌握以上内容后，基本可以使用 git 对源码进行管理，更多有用的功能，请移步 [廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

完。
##感谢 commit只是为了练习 写的很好
