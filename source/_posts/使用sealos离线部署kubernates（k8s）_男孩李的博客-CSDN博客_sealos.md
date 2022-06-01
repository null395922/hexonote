# 使用sealos离线部署kubernates（k8s）_男孩李的博客-CSDN博客_sealos
[使用 sealos 离线部署 kubernates（k8s）\_男孩李的博客 - CSDN 博客\_sealos](https://blog.csdn.net/lovebaby1689/article/details/121319500) 

 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[男孩李](https://blog.csdn.net/lovebaby1689) ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCurrentTime2.png)
 于 2021-11-14 16:44:20 发布 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes2.png)
 1631 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect2.png)
 收藏  4 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

sealos 是个 golang 的二进制工具，直接下载拷贝到 bin 目录即可, release 页面也可下载。
**一条命令部署 Kubernetes 高可用集群**

### **1. 下载并安装 sealos**

> $ wget -c [https://sealyun.oss-cn-beijing.aliyuncs.com/latest/sealos](https://sealyun.oss-cn-beijing.aliyuncs.com/latest/sealos) && \\
>     chmod +x sealos && mv sealos /usr/bin 

### **2. 下载离线资源包**

>  $ [wget](https://so.csdn.net/so/search?q=wget&spm=1001.2101.3001.7020) -c [https://sealyun.oss-cn-beijing.aliyuncs.com/2fb10b1396f8c6674355fcc14a8cda7c-v1.20.0/kube1.20.0.tar.gz](https://sealyun.oss-cn-beijing.aliyuncs.com/2fb10b1396f8c6674355fcc14a8cda7c-v1.20.0/kube1.20.0.tar.gz)

### 3. 安装一个三 master 的 kubernetes 集群

> $ sealos init --passwd '123456' \\
> 	\--master 192.168.0.2  --master 192.168.0.3  --master 192.168.0.4  \\
> 	\--node 192.168.0.5 \\
> 	\--pkg-url /root/kube1.20.0.tar.gz \\
> 	\--version v1.20.0

 参数含义

\| 

\***\* 参数名 \*\***

 \| 

\***\* 含义 \*\***

 \| 

\***\* 示例 \*\***

 \|
\| 

passwd

 \| 

服务器密码

 \| 

123456

 \|
\| 

master

 \| 

k8s master 节点 IP 地址

 \| 

192.168.0.2

 \|
\| 

node

 \| 

k8s node 节点 IP 地址

 \| 

192.168.0.3

 \|
\| 

pkg-url

 \| 

离线资源包地址，支持下载到本地，或者一个远程地址

 \| 

/root/kube1.20.0.tar.gz

 \|
\| 

version

 \| 

[资源包](https://www.sealyun.com/goodsDetail?type=cloud_kernel&name=kubernetes "资源包")对应的版本

 \| 

v1.20.0

 \|

增加 master

> sealos join --master 192.168.0.6 --master 192.168.0.7
> sealos join --master 192.168.0.6-192.168.0.9  # 或者多个连续 IP

 增加 node 

>  sealos join --node 192.168.0.6 --node 192.168.0.7
>  sealos join --node 192.168.0.6-192.168.0.9  # 或者多个连续 IP

删除指定 master 节点 

> sealos clean --master 192.168.0.6 --master 192.168.0.7
> sealos clean --master 192.168.0.6-192.168.0.9  # 或者多个连续 IP

删除指定 node 节点 

> sealos clean --node 192.168.0.6 --node 192.168.0.7
> sealos clean --node 192.168.0.6-192.168.0.9  # 或者多个连续 IP

 清理集群

> sealos clean --all

4\. 安装验证

执行 kubectl get node 命令查询，出现如下截图，说明安装成功。

![](https://img-blog.csdnimg.cn/b6bc2927767d47f5a12701aa22868b5c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA55S35a2p5p2O,size_20,color_FFFFFF,t_70,g_se,x_16)

**5. 安装过程中可能遇到的问题**

![](https://img-blog.csdnimg.cn/1c1bfc7ad85542b2ab81ed8079653a93.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA55S35a2p5p2O,size_20,color_FFFFFF,t_70,g_se,x_16)

\***\* 解决办法：\*\***

初始化 Kubernetes 问题（端口占用）

kubeadm reset // 重置，清理环境

netstat -tlnp|grep 6443 // 查询占用的端口号

lsof -i :6443|grep -v "PID"|awk '{print"kill -9",$2}'|sh // 清除掉占用的端口号进程

同时也可参考下面这篇博客：

[https://blog.csdn.net/u013004700/article/details/81326706](https://blog.csdn.net/u013004700/article/details/81326706)
