```
winget install nushell --scope machine
powershell Set-ItemProperty -Path 'HKLM:\SOFTWARE\OpenSSH' -Name DefaultShell -Value ' C:\Program Files\nu\bin\nu.exe'

code $nu.config-path
$env.config.show_banner = false
$env.config.shell_integration.osc133 = false
```