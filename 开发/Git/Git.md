
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

### http代理

.gitconfig
```
[http "https://github.com"]
proxy = socks5://127.0.0.1:10808/
```

### ssh代理

#在~/.ssh/路径下创建config空文件，然后粘贴如下内容：

windows
```
Host github.com
  ProxyCommand connect -S 127.0.0.1:1080 %h %p
```

macos
```
Host github.com
  ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
```

linux

ubuntu 安装connect-proxy之后配置文件和windows保持一致
```
sudo apt install connect-proxy
```


archlinux 安装openbsd-netcat之后配置文件和windows保持一致
```
sudo pacman -S openbsd-netcat
```
