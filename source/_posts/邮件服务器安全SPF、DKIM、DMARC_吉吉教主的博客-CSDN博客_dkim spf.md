# 邮件服务器安全SPF、DKIM、DMARC_吉吉教主的博客-CSDN博客_dkim spf
[邮件服务器安全 SPF、DKIM、DMARC\_吉吉教主的博客 - CSDN 博客\_dkim spf](https://blog.csdn.net/qq_27979907/article/details/109599415?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-109599415-blog-121948995.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-109599415-blog-121948995.pc_relevant_default&utm_relevant_index=2) 

## SPF

SPF 是指 Sender Policy Framework，是为了防范垃圾邮件而提出来的一种 DNS 记录类型，SPF 是一种 TXT 类型的记录。  
SPF 记录的本质，是**向_收件人_宣告**：本域名的邮件从清单上所列**IP**发出的都是合法邮件。  
下面例子中，tencent.com 宣告：spf.mail.tencent.com 、spf.mail.qq.com 是合法的，其他不合法。  
![](https://img-blog.csdnimg.cn/20201110151103981.png#pic_center)

## DKIM

DKIM 技术通过在每封电子邮件上**增加加密标志**，收件服务通过非对称加密算法解密并比较加密的 hash，判断邮件伪造。

DKIM 的基本工作原理是基于密钥认证方式，他会产生 1 组钥匙，公钥 (public key) 和私钥(private key)。公钥将发布在 DNS 中，私钥会存放在寄信服务器中。发件时，发送方会在电子邮件的标头插入**DKIM-Signature**及电子签名信息，详见下方截图。  
收件人邮件服务器收到邮件后，会通过 DNS 获得 DKIM 公钥，利用公钥解密邮件头中的 DKIM 信息中的哈希值，收件服务器再计算收到邮件的哈希值，两个值进行比较，如果一致则证明邮件合法，则传递邮件。如果验证为不合法，则判定为垃圾邮件。 由于数字签名是无法仿造的，因此这项技术对于垃圾邮件效果极好。

以下是一个 DKIM 签名的例子。其中 b 字段是利用私钥对邮件头、邮件体做的数据签名；bh 是 body 的 hash。  
![](https://img-blog.csdnimg.cn/20201110154032417.png#pic_center)

![](https://img-blog.csdnimg.cn/20201110154218276.png#pic_center)
DNS 获取的公钥信息，其中 p 字段是公钥。  
![](https://img-blog.csdnimg.cn/20201110162242506.png#pic_center)

## DMARC

DMARC（Domain-based Message Authentication, Reporting and Conformance）的目的是给电子邮件域名所有者保护他们的域名的能力，**SPF 和 DKIM 缺少反馈机制**, 这两个协议未定义如何处理 "仿冒邮件"。DMARC 的主要用途在于设置 “处理策略”, 当收件服务接收到来自某个域未通过身份验证的邮件时，应执行的 dmarc 规定的处理机制。  
下图例子中，域名拥有者通过 DNS 发布 DMARC 策略，当收件者收到 “仿冒邮件时” 执行以下策略：  
p:none 不采取对域名的标记。  
rua：发送综合反馈的邮件地址。  
ruf：发送消息详细故障信息的邮件地址。  
![](https://img-blog.csdnimg.cn/20201110171213524.png#pic_center)
测试方法：[https://dmarcian.com/dmarc-inspector/?domain=example.com#](https://dmarcian.com/dmarc-inspector/?domain=example.com#)
