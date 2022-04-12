
### 删除自己最后一次的提交记录
```shell
git reset --hard HEAD^
git push -f
```

### 空白提交
```shell
git commit --allow-empty "我是一个空白提交"
```

### git add / git checkout
git add 之后想取消add的话
```shell
git add file.txt
git checkout -- file.txt  
```

### 直接删除远端分之或标签

```shell
git push origin --delete <branchName>
```

```shell
git push origin --delete tag <tagname>
```