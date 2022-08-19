# 利用hugo建站


# 如何利用hugo建立主题为LoveIt的个人博客

个人主机：
* 操作系统：Windows 10
* git版本：`git version 2.37.1.windows.1`
  
# 1 下载hugo
在hugo的release界面选择合适的版本进行下载<https://github.com/gohugoio/hugo/releases>

建议下载`hugo_extended_0.x.x_Windows-64bit.zip`，原因后面会说

此处我们先下载`hugo_0.101.0_Windows-64bit.zip`,下载步骤如下：
* 下载`hugo_0.101.0_Windows-64bit.zip`到桌面，解压到`D:\blog\bin`
* 设置环境变量`PATH`，将`D:\blog\bin`加入环境变量`PATH`
* 打开cmd,输入`hugo version`,即可查看安装好的hugo的版本
* `git version`查看安装的git的版本

至此，hugo的下载完成。

****
# 2 安装
## 2.1 创建我们自己的项目

打开cmd 输入如下命令
```shell
hugo new site my_website
cd my_website
```
这里的**my_website**就是我们用hugo的`new`命令新建的项目（网站）文件夹

此时我们进入了项目文件夹`my_website`

## 2.2 安装主题
这里我选择的是LoveIt主题，他的仓库是：<https://github.com/dillonzq/LoveIt>

我们下载主题的`最新版本.zip`文件到桌面，然后解压到创建的`/my_website/themes/`文件夹中,注意将解压后的`LoveIt-0.2.xx`文件夹更名为`LoveIt`，方便配置文件的查询

## 2.3 基础配置
参考<https://hugoloveit.com/zh-cn/theme-documentation-basics/#basic-configuration>，将`/my_website`下的`config.toml`的内容替换为上面网址中对应的内容，保存文件。

运行命令
```shell
hugo serve
```
访问<http://localhost:1313/>，可以观察在本地启动的博客网站

## 2.4 创建文章
```shell
hugo new posts/first_post.md
```
可以在`/my_website/content/posts`中找到first_post.md

此时运行
```shell
hugo serve --buildDrafts
```
即可查看含有第一篇文章的页面

# 3 套用更完善的配置文件
利用
<https://hugoloveit.com/zh-cn/theme-documentation-basics/#3-%E9%85%8D%E7%BD%AE>提供的toml文件。

修改`enableGitInfo`为false，修改`name = "Qianli's blog"`使左上角的导航出现

运行
```shell
hugo serve --buildDrafts
```
得到完整的启动在本地的网站

# 4 将本地网站挂载到github-pages上
经历了如上三步，我们只能在本地启动个人博客网站，这样让人很不爽，那么我们该怎么让别人也能看到我们的博客网站呢？ **我们可以将本地博客部署到github-pages上**

## 4.1 创建存储静态页面的仓库
在github中新建一个repo，repo的名字是`github用户名.github.io`
![创建repo](/new_repo.png)
![初始仓库](/initial_repo.png)

## 4.2 使用hugo命令生成包含网站资源的public目录
运行
```shell
hugo --baseUrl="https://Mqleet.github.io/" --buildDrafts
```
然后
```shell
cd public
```
初始化
```shell
git init
```
将所有资源加入index
```shell
git add .
```
提交
```shell
git commit -m "first commit"
```
增加远程定义
```shell
git remote add origin https://github.com/MqLeet/Mqleet.github.io.git
// 这里之前用ssh增加远程定义失败了，所以改用https来远程连接
```
第一次push需要加`-u origin master`，后面再提交可以直接push
```shell
git push -u origin master
```
然后github授权，即可将本地仓库push到github去。
![成功挂载](/success_push.png)

此时，在浏览器中即可使用`github用户名.github.io`访问挂载在github-pages上的个人博客啦！

## 4.3 在4.2中可能会遇到如下报错
```
error: failed to transform resource: TOCSS: failed to transform "scss/main.scss" (text/x-scss): this feature is not available in your current Hugo version

```

参考官方文档<https://gohugo.io/troubleshooting/faq/#i-get-this-feature-is-not-available-in-your-current-hugo-version>的解决方案：下载带有extended版本的hugo即可解决

至此，1处的伏笔也已收回


# 5 写完文章后如何上传：

在my_website文件执行命令行指令：
`hugo --baseUrl="https://Mqleet.github.io/" --buildDrafts` 生成public文件

然后打开需要提交的文件public
`cd public`

对public文件执行

`git add .`


`git commit -m "first commit"` 其中"first commit"是本次提交的名字，可以重复使用

`git push` 将缓冲区的内容push到仓库去

如果遇到如下报错：
```
fatal: unable to access 'https://github.com/MqLeet/Mqleet.github.io.git/': OpenSSL SSL_read: Connection was aborted, errno 10053
```

原因：Git默认限制推送的大小，运行命令更改限制大小即可

解决方法：`git config --global http.postBuffer 524288000`

****
# 结语：
至此，我们由原始配置得到了一个比较粗糙的网站，想得到更精致的个人博客网站，可以参考<https://hugoloveit.com/zh-cn/>的文章，修改配置文件中的参数来设置~
