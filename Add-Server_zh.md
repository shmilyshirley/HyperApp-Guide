# 服务器管理

## 添加服务器

![管理服务器截图](https://github.com/waylybaye/HyperApp-Guide/raw/master/images/manage-server.png "添加服务器")

1. 打开服务器页面，在这里可以显示基本的服务器运行状态，您可以随时下拉刷新更新重新检查服务器状态
2. 添加服务器时您可以使用 root 账号登录也可以使用具有 sudo 权限的非 root 账号登录。如果您使用密码登录请填写密码，如果使用私钥登录密码可以留空。
    * Name: 为您的服务器起个名字
    * Host: 您服务器的公网IP或者域名
    * Port: SSH的端口，一般是22
    * User: 您登陆用的用户名
    * Password: 你的密码，如果使用密钥登录此处可以留空，请参考下面
3. 如果您使用私钥登录，那么请将您的私钥复制到剪切板中（iCloud剪贴板同步或者使用其它同步App）然后点击 "Paste Privatekey from Clipboard" 从剪贴板中复制私钥，如果您的私钥是加密的那么请在在 Passphrase 中输入，否则留空。 关于密钥的更多信息请参考 [常见问题](#常见问题)

* Linode/Vultr/DigitalOcean 等厂商一般可以使用 root 账号和密码直接登录
* 如果您使用 Google CloudEngine 或者 AWS EC2 等默认禁止 root 登录的账号，请查看下面的教程怎样配置私钥登录，然后重复上面的第3步骤在 HyperApp 中链接。



## 如何配置 Linode/Vultr/Digital Ocean 服务器？

这几家服务商一般可以直接使用root密码的登录，您只要填写 `root` 账号和密码即可。

注意：Digital Ocean的某些镜像在您第一次登录时需要修改密码，所以请使用其它工具登录并修改密码后再添加到 HyperApp 中


### Google Cloud Engine

*注：这篇文档只讲述如何链接到 GCE 而不会讲述如何创建 GCE 账号并且创建服务器。*


![管理服务器密钥截图](https://github.com/waylybaye/HyperApp-Guide/raw/master/images/gce-keys.png "管理服务器密钥")


#### 使用密钥登录

在这个页面您可以添加已有的密钥到 GCE 中，您可以点击修改，然后将您上面 `id_rsa.pub` 文件的内容复制到里面去， **GCE 会自动识别里面的用户名，显示在用户名一列，这个就是您登录服务器需要的用户名**。


当您将公钥上传上去后，就可以用您的私钥在您的计算机上直接登录 GCE 服务器了。您可以在计算机上进行测试：


`ssh -i ~/.ssh/id_rsa 上一步显示的用户名@GCE.PUBLIC.IP`


一般来说您可以省略 `-i ~/.ssh/id_rsa` 参数因为 ssh 会自动检查这个默认路径，但如果您生成的密钥在其它地方就需要用 `-i` 参数指定了。


如果您在计算机上测试没有问题，那么可以复制私钥的内容到手机上，并在 HyperApp 添加服务器窗口中点击复制密钥来登录了。


## 关于密钥登录的常见问题

**公钥和私钥在什么地方？**  
一般来说默认的密钥在您的 `~/.ssh/` 目录下（Mac、Linux），其中 `id_rsa` 是私钥, `id_rsa.pub` 是公钥。

**公钥和私钥的格式是什么？**  
1. 公钥一般以 `ssh-rsa *****` 相比私钥内容会更短
2. 私钥一般是下面的格式：  

```
-----BEGIN **** PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,D64916783E629696
***************************************
***************************************
-----END **** PRIVATE KEY-----
```

其中 **** 是 OpenSSH 或者 RSA，注意 `EC` 的密钥可能会认证失败。


**怎样判断私钥是否有加密？**  
您可以查看私钥第二行是否有 **ENCRYPTED** 的字样，如果有则是加密的。  


**我不知道 Mac 上 `id_rsa` 的密码怎么办？**  
如果您使用 Mac 那么 `id_rsa` 的密码保存在钥匙串中，您可以打开：`应用程序 - 实用工具 - 钥匙串访问`，并在右上角搜索 "id_rsa" 获取其密码。  
