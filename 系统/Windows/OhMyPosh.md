OhMyPosh
========

## 官网

https://ohmyposh.dev/

## 安装

官网教程

https://ohmyposh.dev/docs/installation/windows

(也可以直接在Github中下载安装包)

PS：

按照上面的教程，需要先安装官网推荐的字体（我没有安装，我安装好posh之后，用命令行下载其他字体）

## 如何启用

直接执行命令行

```shell
oh-my-posh init pwsh | Invoke-Expression
```

## 每次打开Powershell自动启用

打开powershell的配置文件，然后将上面的shell写入

```shell
notepad $profile
```

## 美化

### 字体

```shell
oh-my-posh font install
```

我选择了安装`FuraMono`并将WindowTerminal的字体设置为`FuraMono NFM`

## 主题

使用命令显示所有主题

```shell
Get-PoshThemes
```

我喜欢的主题为space, 修改启动命令.

`$env:USERPROFILE`是home目录变量

```shell
oh-my-posh init pwsh --config "$env:USERPROFILE\AppData\Local\Programs\oh-my-posh\themes\space.omp.json" | Invoke-Expression
```
