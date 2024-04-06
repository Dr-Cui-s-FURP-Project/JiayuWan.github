# Git Command 4/11

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
若仅查看单一文件中的修改内容
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
git reset HEAD -- example.txt
```
此指令仅仅修改暂存区，工作区修改将会被保留

## HEAD
### 定义
* HEAD是一个reference，一般指向一个分支，当checkout到一个特定的commit的时候，他会指向那个commit。
### HEAD操作
1. 查看当前HEAD所指向的内容：
```
git show HEAD
```
2. 回到上一次历史commit：
```
git checkout HEAD~
```
此指令会使HEAD指向上一次的commit，此时处于分离头状态。但他并不会改变工作历史。

3. 回到历史commit后再返回main
```
git switch main
```

## git pull
### 实质：
* git pull的实质是两个指令先后执行：
    1. git fetch
    2. git merge

### git fetch
* 该命令是从远程仓库中下载最新的历史数据到本地仓库，包括新的tag、branch

### git merge
* 该命令是将下载到的数据与本地仓库合并，将会修改本地内容

# GitHub CLI
## issue 问题
* issue是GitHub中用于任务分配、追踪进度、报告bug等行为的功能
1. 创建issue
```
gh issue create --title "Issue Title" --body "Issue description"
```

2. 查看issue以及对应编号
```
gh issue list
```

3. 查看特定issue以及其回复
```
gh issue view 1 --comments
```
该命令可查看issue编号为1的评论

4. 编辑issue
```
gh issue edit <issue-number> --title "新标题" --body "更新后的正文内容" --add-label "bug,help wanted"
```
常见的label有：
* 类型：
    * **bug**: 表示 Issue 报告了一个错误
    * **feature request**：用于请求新功能或建议项目中应添加的新特性
    * **documentation**：与项目文档相关的改进或问题
    * **enhancement**：对现有功能的改进或增强
    * **question**：请求信息或讨论
* 状态：
    * **good first issue**：适合初学者尝试贡献的简单 Issue
    * **help wanted**：表示项目维护者需要额外的帮助或欢迎贡献者参与
    * **in progress**：表示有人正在处理这个问题或功能
    * **wontfix**：表示决定不解决该问题，可能是因为它已经不再适用，或者不在项目的范围内
* 优先级：
    * **priority: high**：需要优先处理的问题
    * **priority: low**：可以推迟处理的问题
* 模块：
    * **ui/ux**：与用户界面或用户体验改进有关的问题或建议
    * **backend**：涉及后端逻辑或服务器端代码的问题
    * **frontend**：关于前端代码的问题或建议
开发者同样可以自定义label  
label也同样可以用于pull request

5. 设置里程碑 milestone
```
gh issue edit <issue-number> --milestone "v1.0"
```
须在milestone功能中首先定义一个milestone，然后再将issue或pull request连接到milestone上

6. 将issue分配给GitHub用户
```
gh issue edit <issue-number> --assignee "octocat,monalisa"
```
该指令将issue分配给用户名为ocatocat和monalisa的用户

7. 删除issue
```
gh issue close <issue_number>
```
