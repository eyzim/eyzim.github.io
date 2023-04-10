# [Java] JavaMail 使用 Gmail 和 Mailhog 作為 SMTP Server


## SMTP 協定

{{< admonition type=info >}}

SMTP（Simple Mail Transfer Protocol）是大多數電子郵件系統所需的網路協定，允許電子郵件在網際網路上進行傳輸，每當使用者端傳送郵件時，將電子郵件從一個郵件伺服器傳送到另一個郵件伺服器。

{{< /admonition >}}

最基礎版本的 JavaMail，設定 host, port 和是否使用認證即可。

```java
Properties properties = new Properties();

properties.put("mail.smtp.host", smtpServer);
properties.put("mail.smtp.port", smtpPort);
properties.put("mail.smtp.auth", authentication ? "true" : "false");
properties.put("mail.smtp.starttls.enable", tls ? "true" : "false");
if (authentication) {
	session = Session.getInstance(properties,
				new javax.mail.Authenticator() {
					protected PasswordAuthentication getPasswordAuthentication() {
						return new PasswordAuthentication(adminAccount, adminPassword);
					}
				});
			}
else
{
    session = Session.getInstance(properties, null);
}
```

## Gmail

1. Gmail 需要設定 App service 專用密碼，不能直接輸入登入用帳號密碼，[操作說明](https://support.google.com/accounts/answer/185833)
2. Client 端設定參數:

    - Server Host: smtp.gmail.com
    - Server Port: 465
    - Account: abc@gmail.com
    - Password: 根據第一點 Gmail 要求所產生的十六位專用密碼
    - Auth 和 TLS 必須開啟

到了 Gmail，因為 Gmail 採用 TLS 加密，我們還需要加入 socket factory

```java
if(TLS)
{
	properties.put("mail.smtp.socketFactory.port", smtpPort);
	properties.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
	properties.put("mail.smtp.socketFactory.fallback", "false");

	MailSSLSocketFactory sf = new MailSSLSocketFactory();
	sf.setTrustAllHosts(true);
	properties.put("mail.smtp.ssl.trust", "*");
	properties.put("mail.smtp.ssl.socketFactory", sf);
}

```

## [MailHog](https://github.com/mailhog/MailHog)

MailHog 是一個位於本機端的 SMTP Server 及 Client，他會接收其他 App (我們送出的) SMTP request，接收後不會轉發，而是送到 MailHog 自己架設的 Web page (可以想像成 Gmail client 端)，可以看到我們送出的 SMTP request 接收到信件時的模擬畫面。

1. 先在本機端 clone mailhog 的 docker

    ```bash
    docker pull mailhog/mailhog
    ```

2. Run docker

    ```bash
    docker run -d -p 1025:1025 -p 8025:8025 mailhog/mailhog
    ```

    Port 1025 作為 SMTP Server 收聽端，Port 8025 作為 SMTP Mail web page 的呈現端口

3. 開啟 docker 需要使用的兩個端口權限

    ```bash
    sudo ufw allow 1025
    sudo ufw allow 8025
    ```

4. 確認開啟後，可以到 [localhost:8025](http://10.1.116.65:8025/) 檢查 web page 是否開啟

## Reference

1. Mailhog docker: https://hub.docker.com/r/mailhog/mailhog/

