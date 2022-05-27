# Git常见命令

## 1. 查看分支

### 1.1 查看本地分支

* ```
  $ git branch
  
  * master
  ```

### 1.2 查看远程分支

```
//一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令，git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响

git fetch 
git branch -r
```



### 1.3. 查看所有分支

```
git branch -a
```



## 2.本地创建新的分支

```
git branch [branch name]
```



## 3. 切换到新的分支

```
git checkout [branch name]
```



## 4.创建+切换分支

```
git checkout -b [branch name]
```

#### git checkout -b [branch name] 的效果相当于以下两步操作：

```
git branch [branch name]
git checkout [branch name]
```





## 5. 将新分支推送到github

```
git push origin [branch name]
```



## 6. 删除本地分支

```
git branch -d [branch name]
```



## 7. 删除github远程分支

```
git push origin :[branch name]
```

#### 分支名前的冒号代表删除。例如：

```
git push origin :gh-dev
```

