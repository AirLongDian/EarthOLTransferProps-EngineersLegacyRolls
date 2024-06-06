# Git简明教程
![image.png](https://i.loli.net/2021/02/27/QWVMKGvEspFelx4.png)

### 1. 什么是git
Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。分布式相比于集中式的最大区别在于开发者可以提交到本地，每个开发者通过克隆（git clone），在本地机器上拷贝一个完整的Git仓库。

**概括：**git是一个项目版本管理工具，用来合作开发和做版本迭代以及必要的时候进行方便的版本回滚。
<!--more-->
### 2. git本地基本操作
1. 初始化（生成）一个本地git仓库  
新建一个目录（文件夹），并在其中打开终端，执行
```
git init
```
2. 把修改后的文件添加到暂存区
```
git add filename	//添加filename到暂存
git add .	//添加当前目录（添加文件夹内所有文件）到暂存区
```
3. 把暂存区内的文件提交成一个新的版本
```
git commit -m "message"	//把暂存区提交为一个新版本，标注信息，信息为message
```
4. 查看工作区，暂存区中文件的状态
```
git status //查看工作区和暂存区文件信息
```
5. 查看分支信息
```
git branch //查看当前有哪些分支以及当前在哪个分支
```
6. 新建一个分支
```
git branch branchname	//创建一个名为branchname的分支
```
7. 切换分支
```
git checkout branchname	//切换到branchname分支
```
8. 查看提交历史
```
git log --oneline	//查询历史（简略信息）
git log	//查询历史（详细）
git log --oneline	//查询历史（简略信息）(生成图像)
```
9. 分支合并
```
git merge branchname	//把branchname分支合并到当前分支
```
10. 冲突解决  
合并后修改冲突的文件后提交一个新版本
11. 回退到某一版本
```
git log  //查看历史（查看每个版本commit编码）   

git reset --hard [要回退到的commit编码]
```
<font color=red>下次push提交因为版本变旧需要加参数</font>`-f`
```
git push -f
```
13. 删除中间的某一次提交
```
git show        \\查看提交信息
git revert [要删除的commit编码]
git revert -m 要保留的分支编号（show里列出来的第几个） [要删除的commit编码]     \\如果要撤销的操作是分支合并则加`-m`

```
### 3.远程托管操作
1. 添加远程仓库
```
git remote add origin links	//添加远程仓库links并且起名为origin（自己随便起）
```
2. 推送更新
```
git push origin master	//把master分支推送到origin（上面设置的名字）
```
3. 拉取更新
```
git pull origin master	//把远程仓库origin的master分支拉取到本地
```
4. 克隆远程仓库
```
git clone links	//把远程仓库links克隆到本地
```