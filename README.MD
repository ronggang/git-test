# 说明
练习 Git 相关操作
## 准备工作
安装 Git，可到官网：https://git-scm.com/ 下载

## 第一步 初始化 Git 库
在需要进行版本的目录下执行以下命令：
```
git init
```
即可创建一个Git版本库，成功后有如下提示：
```
Initialized empty Git repository in G:/git-test/.git/
```
说明已经创建一个空的“仓库”，现在就可以开始使用了。

## 第二步 添加文件和提交
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

## 第三步 创建分支
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

## 第四步 合并分支
当在分支下内容修改完成后，可以将当前内容合并到主分支（master）
```
git checkout master
git merge dev
```
在进行合并操作时，需要先切换到 master 分支，再进行合并操作