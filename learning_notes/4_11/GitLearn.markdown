# Git Learning 4/11

## 分支 branching
### 创建分支
1. 确认当前分支
```
git status
```
该命令会返回当前在什么分支上工作，同时返回在提交区的文件状态

2. 创建一个新分支
```
git branch feature-new
```
该命令会创建一个名为[^feature-new]的分支

3. 切换分支
```
git checkout feature-new
```
该命令会跳转到[^feature-new]分支上开始工作

或者使用
```
git checkout -b feature-new
```
直接创建新分支并且跳转到该分支上，取代2，3步操作

### 完成开发后合并分支
1. 添加并提交新文件
```
git add .
git commit -m "Add new functionality"
```

2. 合并分支
    1. 首先切换回[^main]分支
    ```
    git checkout main
    ```
    2. 合并[^feature-new]分支
    ```
    git merge feature-new
    ```

3. 删除分支
```
git branch -d feature-new
```
一般在完成一个功能开发后，都会删除分支以获得更整洁的仓库空间


## 差异 diff
### 查看add前，当前工作区和未暂存文件的修改内容
```
git diff
```
若镜像查看单一文件中的修改内容
```
git diff example.txt
```

### 查看add后，已暂存文件与上一次commit之间的内容区别
```
git diff --staged
```

### 查看特定两次commit之间的内容差异
1. 查看commit历史
```
git log
```
以获取具体两次commit的哈希值，并且按下[^q] quit 来返回命令行
2. 比较两次提交间的差异
```
git diff [older-commit-hash] [newer-commit-hash]
```

## 撤销更改
### 放弃当前更改，并且回到上一次commit时的状态
```
git restore example.txt
```
此命令会撤销当前所作的所有更改，并且回到上次commit时的状态，不可逆转

### 将文件从暂存区中移除，但保留工作区修改
```
git reset 
