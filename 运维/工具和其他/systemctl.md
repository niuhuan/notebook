

查看某个服务的日志

```shell
journalctl -t code-server
```

```
➜  system cat /etc/systemd/system/stalwart-mail.service
[Unit]
Description=Stalwart Mail Server Server
Conflicts=postfix.service sendmail.service exim4.service
ConditionPathExists=/opt/stalwart-mail/etc/config.toml
After=network-online.target

[Service]
Type=simple
LimitNOFILE=65536
KillMode=process
KillSignal=SIGINT
Restart=on-failure
RestartSec=5
ExecStart=/opt/stalwart-mail/bin/stalwart-mail --config=/opt/stalwart-mail/etc/config.toml
SyslogIdentifier=stalwart-mail
User=stalwart-mail
Group=stalwart-mail
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```
