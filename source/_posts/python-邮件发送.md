---
title: python-邮件发送
date: 2022-11-17 23:23:56
tags:
---


# 使用 python 发邮件  

## 1. SMTP 协议  
SMTP（Simple Mail Transfer Protocol ）：简单邮件传输协议。python 默认支持该协议，使用该协议可以构造纯文本和带附件的邮件。该协议其中包含两个模块：smtplib（发邮件）、email（构造邮件）。

## 2. 纯文本邮件

### 2.1 邮箱设置  
#### 2.1.1 QQ邮箱:  
1. 先设置邮箱授权码  
设置 -> 账户 -> POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务 -> 开启POP3/SMTP服务  
{% asset_img 开启pop3协议.jpg 开启pop3协议 %}   
<center>图1.开启pop3协议</center>
2. 配置邮件客户端  
按提示，编辑短信，获取授权码。  
{% asset_img 短信验证.png 短信验证 %}   
<center>图2.短信验证</center>
<br>
{% asset_img 获取授权码.png 获取授权码 %} 
<center>图3.获取授权码</center>  



**注意⚠️**：千万不要泄漏密码或授权码，会被用来发垃圾邮件 (T▽T) ，一大早被发了几十封！ 
{% asset_img 授权码盗用后果.jpeg 授权码盗用 %} 
<center>图4.授权码盗用后果</center> 

**紧急处理：**  
1. 关闭QQ邮箱授权码：设置->账户->关闭所有协议 。 
2. 修改QQ密码：因为只有修改密码，之前的认证码才会失效。
3. 排查泄漏源头，将其消灭。

**经验教训：**  
1. 不要泄露认证码、密码相关信息在公开的代码哭、博客或日志中。下图是我上传博客v1.0的时候，没有将认证码脱敏。  
{% asset_image 认证码泄漏源头.png 认证码泄漏源头 %}
<center>图5.认证码泄漏源头</center> 

2. 一旦发现泄漏问题，及时止损，先解除之前令牌的有效性，再将源头泄漏点修补好。



#### 2.1.2 Outlook邮箱  
POP 服务器名称：outlook.office365.com
默认端口：995  
加密方法：TLS  

{% asset_img outlook开启POP3.png outlook开启POP3 %}
<center>图6.outlook开启POP3</center> 

### 2.2 邮件编辑与发送  

``` py
# 调用 SMTP 发件服务
import smtplib
# 导入纯文本的邮件模板类
# MIMEText 类被用来创建主类型为 text 的 MIME 对象
from email.mime.text import MIMEText

# 配置发送参数
# QQ邮箱服务器
smtp_server = 'smtp.qq.com'
sender = '**********@qq.com'
auth_code = 'e****f'
receiver = '**********@qq.com'
# QQ邮箱服务器默认端口号
port = 465

# 编写邮件正文
body='这是该封邮件的内容，仅供参考。'
msg = MIMEText(body, 'html', 'utf-8')
# 发件人名称展示
msg['from'] = 'plumrx'
msg['to'] = receiver
msg['subject'] = 'text email demo'

# 登录采用SSL方式，安全套接字层，可确保互联网连接安全，保护两个系统之间发送的任何敏感数据，防止犯罪分子读取和修改任何传输信息。
# 调用发件服务
smtp = smtplib.SMTP_SSL(smtp_server, port)
# 登录
smtp.login(sender, auth_code)
# 发送邮件，以字符串拼接
smtp.sendmail(sender, receiver, msg.as_string())
# 关闭邮件服务
smtp.quit()
```  

{% asset_img 纯文本邮件.jpg 邮件结果 %}
<center>图7.纯文本邮件</center> 






## 3. 带附件邮件  
发送带附件邮件需要用到 MIMEMultipart 类，可以生成包含多个部分的邮件体。
1. 导包
2. 配置发送邮件相关参数（5个）:  
发件所属邮箱服务、发件人、收件人、认证码、默认端口号
3. 读取附件文件
4. 构造邮件实例，定义三要素：  
发件人、收件人、主题  
5. 构建邮件体，包含附件文件格式  
6. 调服务，发邮件

```py
import smtplib
# 纯文本文件模板
from email.mime.text import MIMEText
# 带附件
from email.mime.multipart import MIMEMultipart
import os

# 发邮件相关参数，隐私信息配置在环境变量中
smtp_server = 'smtp.qq.com'
sender = os.getenv('SENDER')
receiver = os.getenv('SENDER')
auth_code = os.getenv('AUTH_CODE')
port = 465

# 读取准备好的附件文件
filepath = r"./email_fujian.txt"
with open(filepath, 'rb') as fp:
    mail_body = fp.read()

# 构造邮件实例三要素
msg = MIMEMultipart()
msg['from'] = sender
msg['to'] = receiver
msg['subject'] = "Theme of email with attach"

# 构建邮件体
body = MIMEText(mail_body, "html", "utf-8")
msg.attach(body)
att = MIMEText(mail_body, "base64", "utf-8")
att['Content-Type'] = 'application/octet-stream'  # 返回的是一个二进制文件
# filename 为邮件中展示的附件名
att['Content-Disposition'] = 'attachment;filename="text.txt"'
msg.attach(att)

try:
    smtpObj = smtplib.SMTP()
    # 链接邮箱服务器
    smtpObj.connect(smtp_server)
    # 调用发件服务
    smtpObj.login(sender, auth_code)
    print("邮件发送成功")
except:
    smtpObj = smtplib.SMTP_SSL(smtp_server, port)
    smtpObj.login(sender, auth_code)

# 发送邮件
smtpObj.sendmail(sender, receiver, msg.as_string())
smtpObj.quit()

```

{% asset_image 带附件邮件结果.png 带附件邮件结果 %}
<center>图8.带附件邮件结果</center>
<br>
{% asset_image 邮件附件.png 邮件附件 %}
<center>图9.邮件附件</center>