
设置端口规则
```shell
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```

在debian开机自动设置规则
```shell
# 将当前规则保存到文件
iptables-save > /etc/iptables
# 创建开机自动加载规则的脚本
cat > /etc/network/if-pre-up.d/iptables << EOF
/sbin/iptables-restore < /etc/iptables
> EOF
```

