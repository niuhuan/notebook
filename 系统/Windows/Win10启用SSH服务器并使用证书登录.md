Win10启用SSH服务器并使用证书登录

1. 可选功能添加SSH服务器
2. 启动一次SSH服务, 在C:\ProgramData\ssh下生成sshd_config
3. 将下两行注释掉
    ```text
    #Match Group administrators
    #       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
    ```

注册表

\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH 中的 DefaultShell 项改为 C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe (powershell.exe 的位置), 
可以修改默认shell程序
