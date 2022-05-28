# github不能使用账户密码登陆了？ github的token使用（超详细）_CPerdst的博客-CSDN博客_github token怎么用
 然后使用**git remote**来查看自己的 url，**git branch -v** 查看自己的分支，使用**git remote set-url &lt;your_url> https&#x3A;//&lt;your_token>@github.com/<username>/<repo>.git** 来更新自己的 url
 然后使用**git remote**来查看自己的 url，**git branch -v** 查看自己的分支，使用**git remote set-url &lt;your_url> https&#x3A;//&lt;your_token>@github.com/<username>/<repo>.git** 来更新自己的 url
使用**git push &lt;your_url>** 来更新 github
使用**git push &lt;your_url>** 来更新 github
最近想把自己写的几个小程序上传到[github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020)上面，但是 github 那端已经不让使用账号密码进行验证登录了，所以在此做一个自己总结的 github token 使用教程来记录一下，以防以后再不会用了。

* * *

# **一，生成自己的 token**

首先选择 setting 进入设置，然后在进入 Developer setting，选择生成私人 token。

![](https://img-blog.csdnimg.cn/eafc63688afe412e90e0f573d9e00c7a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_4,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/31dfd534fbc441ed9e21f136af19f08b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_20,color_FFFFFF,t_70,g_se,x_16)

note 随便写，天数尽量选长点时间，选上 repo 才能用 git 指令操作自己的 repositories。

![](https://img-blog.csdnimg.cn/d2b6864f2642494ab9966e4037ac1b60.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_18,color_FFFFFF,t_70,g_se,x_16)

# **二，使用 token**

可以从自己的 github 上面 clone 一个项目，例如：

![](https://img-blog.csdnimg.cn/9f2737c42eb046bc8e590dbb7999b34a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_20,color_FFFFFF,t_70,g_se,x_16)

 值得注意的是它的格式是这样的： 

你在 github 上的原始 url： **[https://github.com/](https://github.com/)<username>/<repo>.git**

而你现在需要 clone 的则是：**https&#x3A;//&lt;your_token>@github.com/<username>/<repo>.git**

**&lt;your_token>**是你自己刚刚生成的 token，

**<username>**是自己设置的，你可以从这里查看（是下面的（**CPerdst**））：

![](https://img-blog.csdnimg.cn/3ed5ab76946245e8b8940d89e9bab460.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_9,color_FFFFFF,t_70,g_se,x_16)

 如果正常的话，现在你已经将自己在 github 上的项目下载下来了（如果没有，建议开一下代理，毕竟 github 是国外的网站），现在就可以继续写自己的项目了，我这里为了演示就直接随便创造一些文件来代替。

现在进入自己的项目：

![](https://img-blog.csdnimg.cn/0e3420774bea444ebc9cfef8ac17962b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_20,color_FFFFFF,t_70,g_se,x_16)

 使用**git status**查看当前的状态

![](https://img-blog.csdnimg.cn/a0cfda817d0e4664b5f114008ee06b3f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_18,color_FFFFFF,t_70,g_se,x_16)

使用**git add ./**来确定更改，然后使用**git commit -m 'your update message'**添加提交信息。

![](https://img-blog.csdnimg.cn/f843b38b139d4522aaa0c6ae05f36102.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfNDI5MTU1MjY=,size_20,color_FFFFFF,t_70,g_se,x_16)

 然后使用**git remote**来查看自己的 url，**git branch -v** 查看自己的分支，使用**git remote set-url &lt;your_url> https&#x3A;//&lt;your_token>@github.com/<username>/<repo>.git** 来更新自己的 url

使用**git push &lt;your_url>** 来更新 github

补充：如果当使用 git push 的时候没有显示账户密码可以使用 **git config --system --unset credential.helper** 更新。

或者使用 git reset 重置一下

引自： [(21 条消息) github 开发人员在七夕搞事情：remote: Support for password authentication was removed on August 13, 2021.\_星空 - CSDN 博客\_github 开发人员在七夕搞事情](https://blog.csdn.net/weixin_41010198/article/details/119698015 "(21 条消息) github 开发人员在七夕搞事情：remote: Support for password authentication was removed on August 13, 2021.\_星空 - CSDN 博客\_github 开发人员在七夕搞事情") 
 [https://blog.csdn.net/qq_42915526/article/details/122362565?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-122362565-blog-120060010.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-122362565-blog-120060010.pc_relevant_antiscanv3&utm_relevant_index=1](https://blog.csdn.net/qq_42915526/article/details/122362565?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-122362565-blog-120060010.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-122362565-blog-120060010.pc_relevant_antiscanv3&utm_relevant_index=1)
