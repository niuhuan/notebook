Win10家庭版开启gpedit
===================

方法一

管理员方式打开CMD

```cmd
for /f "delims=" %f in ('dir /b /a-d-h-s %systemroot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum') do dism /online /norestart /add-package:"%systemroot%\servicing\Packages\%f"
```

tips: 如果写成.cmd文件, `%f`应该写为`%%f`

方法二

管理员方式打开Powersehll

```powershell
$DEMO=[string[]](dir $env:SystemRoot\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum)
foreach($i in $DEMO) {dism /online /norestart /add-package:"$i";}
```

