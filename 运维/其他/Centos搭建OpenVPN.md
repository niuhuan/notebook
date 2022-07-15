Centos搭建Openvpn

### 安装bin
```shell
dnf install openvpn easy-rsa
```

### 配置服务器
/etc/openvpn/server/server.conf
```shell
daemon
# server
mode server
proto udp
local 0.0.0.0
lport 1194
cipher BF-CBC
comp-lzo
dev tun
server 172.16.0.0 255.255.0.0
ifconfig-pool-persist /var/log/openvpn/ipp.txt
# certs
ca /etc/openvpn/server/pki/ca.crt
cert /etc/openvpn/server/pki/issued/server.crt
key /etc/openvpn/server/pki/private/server.key
dh /etc/openvpn/server/pki/dh.pem
# 增加的内容
client-config-dir /etc/openvpn/ccd # 固定IP使用
client-to-client # 允许客户端之间通信
keepalive 10 120
persist-key
persist-tun
resolv-retry infinite
duplicate-cn
```

### 将macmini这个客户端的ip固定为101

mkdir /etc/openvpn/ccd
编辑 /etc/openvpn/ccd/macmini
```shell
ifconfig-push 172.16.0.101 172.16.0.102
```

胡乱写会报错, 用里面的命令行找出可用的子网
```text
There is a problem in your selection of --ifconfig endpoints [local=172.16.0.7, remote=172.16.0.8].  The local and remote VPN endpoints must exist within the same 255.255.255.252 subnet.  This is a limitation of --dev tun when used with the TAP-WIN32 driver.  Try 'openvpn --show-valid-subnets' option for more info.
```

```text
C:\Program Files\OpenVPN\bin>openvpn --show-valid-subnets
On Windows, point-to-point IP support (i.e. --dev tun)
is emulated by the TAP-Windows driver.  The major limitation
imposed by this approach is that the --ifconfig local and
remote endpoints must be part of the same 255.255.255.252
subnet.  The following list shows examples of endpoint
pairs which satisfy this requirement.  Only the final
component of the IP address pairs is at issue.

As an example, the following option would be correct:
    --ifconfig 10.7.0.5 10.7.0.6 (on host A)
    --ifconfig 10.7.0.6 10.7.0.5 (on host B)
because [5,6] is part of the below list.

[  1,  2] [  5,  6] [  9, 10] [ 13, 14] [ 17, 18]
[ 21, 22] [ 25, 26] [ 29, 30] [ 33, 34] [ 37, 38]
[ 41, 42] [ 45, 46] [ 49, 50] [ 53, 54] [ 57, 58]
[ 61, 62] [ 65, 66] [ 69, 70] [ 73, 74] [ 77, 78]
[ 81, 82] [ 85, 86] [ 89, 90] [ 93, 94] [ 97, 98]
[101,102] [105,106] [109,110] [113,114] [117,118]
[121,122] [125,126] [129,130] [133,134] [137,138]
[141,142] [145,146] [149,150] [153,154] [157,158]
[161,162] [165,166] [169,170] [173,174] [177,178]
[181,182] [185,186] [189,190] [193,194] [197,198]
[201,202] [205,206] [209,210] [213,214] [217,218]
[221,222] [225,226] [229,230] [233,234] [237,238]
[241,242] [245,246] [249,250] [253,254]

```

### 注册客户端
sign完要重启服务
```shell
# 进入设置文件夹
cd /etc/openvpn/server/
# 第一次复制工具过来
cp -rf /usr/share/easy-rsa/3.0.8 /etc/openvpn/server/easy-rsa
# 生成并注册req
./easy-rsa/easyrsa gen-req macmini nopass
./easy-rsa/easyrsa sign client macmini
```

### 启动服务和开机自起
```shell
systemctl start openvpn-server@service
systemctl enable openvpn-server@service
```

ps: 查看service
```shell
[root@niuhuan ~]# rpm -ql openvpn |grep service
/usr/lib/systemd/system/openvpn-client@.service
/usr/lib/systemd/system/openvpn-server@.service
/usr/lib/systemd/system/openvpn@.service
```

ps: 手动启动
```shell
openvpn /etc/openvpn/server/server.conf
```

### 客户端配置
ovpn
```text
client
proto udp
remote server.domain.com
port 1194
dev tun
nobind
comp-lzo
cipher BF-CBC
remote-cert-tls server

keepalive 10 120
persist-key
persist-tun
resolv-retry infinite

<ca>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</ca>

<cert>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</cert>

<key>
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
</key>


```

如果配置了ta.cer, 应该去掉 " remote-cert-tls server  ", 并且增加 <tls-auth> 

### windows的防火墙会导致不能互相访问

https://superuser.com/questions/1106907/windows-firewall-doesnot-allow-to-connect-from-vpn

```
Open Windows firewall with advanced security
Click inbound rules on the left
Click New rule on the right
Click Custom rule
Specify programs or leave as all programs
Specify ports or leave as all ports
Click "These ip addresses" under remote IP
Click "This ip address range"
Type From "10.8.0.1" To "10.8.0.254"
Close and click Next, then leave as "Allow the connetion"
Apply to all profiles
Name it & Finish
```


### 只用账号密码验证

服务器
```text
client-cert-not-required
username-as-common-name
script-security 3
auth-user-pass-verify /etc/openvpn/checkpsw.sh via-env
```

客户端
删掉cert和key
增加auth-user-pass

### 建立多个网段并互通

新建两个config

1.config
```text
# 设置
lport 1194
server 172.218.1.0 255.255.255.0
# 增加
push "route 172.218.2.0 255.255.255.0"
```

2.config
```text
# 设置
lport 1195
server 172.218.2.0 255.255.255.0
# 增加
push "route 172.218.1.0 255.255.255.0"
```

设置防火墙
```shell
firewall-cmd --add-masquerade
firewall-cmd --add-masquerade --permanent
```

ps:
- firewall-cmd --query-masquerade
- http://blog.chinaunix.net/uid-20329764-id-5859459.html