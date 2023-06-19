Win10/11家庭版开启Hyper-V
=======================

保存成cmd在管理员模式下运行, 重启后在控制面板中可以启用Hyper-V

```cmd
pushd "%~dp0"  
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt  
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"  
del hyper-v.txt  
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```