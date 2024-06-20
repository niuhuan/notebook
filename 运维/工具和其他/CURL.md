

### 发送邮件

```shell
curl --ssl \
  "smtp://smtp.qq.com:587" \
  --mail-from "form@qq.com" \
  --mail-rcpt "to@qq.com" \
  --user "from@qq.com:auth_code_or_password" \
  --upload-file body.txt
```

```text
From: From User <from@qq.com>
To: To User <to@qq.com>
Subject: an example.com example email
Date: Mon, 7 Nov 2016 08:45:16

Dear To,
I am from.
Welcome to this example email. What a lovely day.
```

##


```shell
curl -I -s  https://www.baidu.com/ | head -n 1 | awk '{print $2}'
```
