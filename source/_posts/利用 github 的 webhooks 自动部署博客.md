# 利用 github 的 webhooks 自动部署博客
[利用 github 的 webhooks 自动部署博客](https://mp.weixin.qq.com/s/dr3GPbbFL89BLzf-b7a8ug) 

## 背景

博客原是 WordPress 搭建的，由于某些个人无法接受的原因，准备转成静态博客，之前转过，使用的是 HEXO，基本成功了，但是是部署在 github pages 上面的，如今准备部署在国内的阿里云服务器上。

## 问题

使用 HEXO 搭建的博客平台，进行写博客的流程是：

> 1、本地使用 Markdown 编写博客， 2、本地编译成 html 文件， 3、将 html 文件上传至服务器，一般免费的有 github page，gitee 等

所以此处需要改造的点其实很简单，就是把 github 仓库中的 html 文件全部下载至服务器上，然后利用 web 服务器做个反向代理即可，比如 nginx。

后续的部署流程：

> 1、本地写完博客，编译完成后上传至 github 仓库 2、登陆到服务器上面进行 git update 操作

问题就此暴露出来了，每次写完博客都要登陆下服务器去更新下 html 文件，我的天，这谁顶得住。

## 解决

于是自然而然想到了 git 仓库的 web 钩子，即当 git 仓库收到 push 或者其他命令时，会自动调我们的钩子程序（其实就是向我们指定的地址发送一个 post 请求），而我们只需要让我们的这个程序去跟新下服务器上面的代码（html 文件）即可。

流程示例图见下图：

![](https://mmbiz.qpic.cn/mmbiz_png/6J78Wg8WMA9V1fzj4FfRyvZcb2IjlPaBria3Vyiaeibq8KBvnOO9V1w1kIONBYMO57NTDMMBqaQjaE89mpbWfZe9A/640?wx_fmt=png)

从上面可以出，我们只剩 2 件事需要做，一是配置下 github 的 webhook，二是在我们的服务器上写一个 web 程序来处理 github 发送过来的请求。

### 1、webhook 配置

配置比较简单，需要注意的是，秘钥虽然是非必填的，但是最好还是加上，为了安全。![](https://mmbiz.qpic.cn/mmbiz_png/6J78Wg8WMA9V1fzj4FfRyvZcb2IjlPaBwl7BTOPxFABsSicic8CQibaZiam0bBAy3KYPibSqJSI8JQXeQL3ianzZgubw/640?wx_fmt=png)

请求的数据，每个平台都不一样，在此以 github 为例，数据内容见下图：**注：请求头中有一个很重要的签名参数，该参数前部分为加密方式，后部分为的签名，而明文就是我们在 webhook 中配置的秘钥。这点很重要哦。** ![](https://mmbiz.qpic.cn/mmbiz_png/6J78Wg8WMA9V1fzj4FfRyvZcb2IjlPaBIVApVrfwWA9ibVoGicqHfBWJVDbkTgew0FU78dzpcEkVmOcz1Q8LRN2g/640?wx_fmt=png)

### 2、程序编写

写一个简单是 web 程序。

本人主要是编写 java 代码的，如果要用 java 来写的话，比较麻烦，所以考虑选择一款解释性语言来编写，其实用啥语言都无所谓啦，毕竟代码处理逻辑很简单，选用自己熟悉的就行，例如 Python、PHP、nodejs 等都行。

此处选择的是使用 Python 来编写，web 框架是比较轻量的 Flask，操作 git 使用的是 gitPython，所以需要安装下：

> pip install flask pip install gitpython

代码比较简单，不多赘述，主要就是两点：

> 1、签名校验 2、git update

详细代码如下：

1.  `from flask import  Flask, request,  Blueprint, jsonify, current_app`
2.  `from git import  Repo`
3.  `import hmac`


5.  `app =  Flask(__name__)`


7.  `@app.route("/api/github_hook", methods=['POST'])`
8.  `def github_web_hook():`


10. `header_signature_origin = request.headers.get('X-Hub-Signature')`
11. `if header_signature_origin is  None:`
12. `return  '你没有权限访问！'`


14. `hash_type, header_signature = header_signature_origin.split('=')`
15. `if hash_type !=  'sha1':`
16. `return  '不支持的加密方式！'`


18. `secret = str.encode("http://www.jetchen.cn")`


20. `hashhex = hmac.new(secret, request.data, digestmod='sha1').hexdigest()`
21. `if hmac.compare_digest(hashhex, header_signature):`
22. `repo =  Repo('/data/blog-test/blog.github.io/')  # 获取本地 git 仓库`
23. `# repo = Repo('D:\project\mixed\\blog.github.io\\') # 获取本地 git 仓库`
24. `# origin = repo.remotes.origin # 获取远程库 & 远程分支`
25. `# origin.pull('--rebase') # 拉代码`
26. `remote = repo.remote()`
27. `remote.pull('master')`


29. `if  'after'  in request.json:`
30. `commit = request.json['after'][0:6]  # pull 的最新的 commit`
31. `print('Repository updated with commit {}'.format(commit))`


33. `else:`
34. `return  '签名校验失败！'`


36. `return jsonify({}),  200`


38. `if __name__ ==  "__main__":`
39. `app.run(port=8001)`
