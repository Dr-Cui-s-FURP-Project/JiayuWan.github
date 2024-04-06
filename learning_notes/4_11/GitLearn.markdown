# Git Learning 4/11

## 分支 branching
### 创建分支
1. 确认当前分支
```
git status
```
该命令会返回当前在什么分支上工作

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