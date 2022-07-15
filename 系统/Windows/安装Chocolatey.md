

使用powershell.exe安装

注意：在运行任何脚本之前，请检查 https://chocolatey.org/install.ps1 以确保安全。我们已经知道它是安全的，但是您应该从您不熟悉的Internet验证 任何脚本的安全性和内容。所有这些脚本都下载一个远程PowerShell脚本并在您的计算机上执行。我们非常重视安全性。 了解有关我们的安全协议的更多信息。
使用PowerShell，您必须确保Get-ExecutionPolicy不受限制。我们建议使用Bypass绕过策略来安装东西或AllSigned提高安全性。

运行Get-ExecutionPolicy。如果返回Restricted，则运行Set-ExecutionPolicy AllSigned或Set-ExecutionPolicy Bypass -Scope Process。
现在运行以下命令：

```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

来自: https://blog.csdn.net/qq_35576225/article/details/109073215

