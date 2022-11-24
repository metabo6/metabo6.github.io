---
title: github 个人博客搭建
tags: Hexo
abbrlink: cdafe1c9
date: 2022-11-24 10:22:36
cover: ../../../../img/React 移动端总结/hexo.webp
---

# github 个人博客搭建



> 好久之前就有看到过很酷炫的个人博客，今天正好有时间就来搭建个属于自己的博客网站
>
> 尝试过自己租用服务器配合 WordPress 框架搭建，但是服务器需要费用以及国内网站需备案等过于繁琐，使用 github + hexo 搭建非常方便，但是过程中也碰到很多坑，特来总结下，[参考地址](https://wushishu.xyz/post/be8880ea.html)

### 1. 安装必备搭配工具

- Node：[官网下载](https://github.com/git-for-windows/git/releases/download/v2.37.3.windows.1/Git-2.37.3-64-bit.exe) ，` node -v`  查看版本
- Git： [ 官网下载 ](https://github.com/git-for-windows/git/releases/download/v2.37.3.windows.1/Git-2.37.3-64-bit.exe)， ` git --version` 查看版本
- npm：下载 ` Git ` 后自带 `npm` ， `npm -v ` 查看版本
- hexo：下载安装全局包，`npm install -g hexo-cli`



### 2. Github 账户并配置 SSL

**1. 生成 SSH Keys**

```js
ssh-keygen -t rsa -C "你的邮箱地址"
```

**2. 绑定 github 公钥**

- 查找` .ssh` 目录 ` id_rsa.pub` 文件并复制内容，一般在  `C:\Users\admin\.ssh`
- 登录 github 账户，找到 setting => SSH and GPG keys => New SSH key，填写标题并粘贴内容绑定。

**3. 测试 ssh 是否绑定成功**

> 此处可能会报错：connect to host github.com port 22: Connection refused， 详见注意事项

```js
ssh -T git@github.com
```



### 3. 创建 Blog 文件

**1.  创建空文件夹并初始化 hexo**

> 此处也可能会报错，或一直无法初始化，解决办法见注意事项

```js
// 在新创建的空文件夹下面执行如下命名
hexo init
```

**2. 生成本地 hexo 页面**

```js
// 初始化后执行如下命令
hexo s
```

**3. 访问本地服务器**

```js
http://localhost:4000/
```

**4. 修改 -config.yml 文件**

```js
// 将该文件最下面改为如下格式，注意冒号后面带空格，符号都为英文符号
deploy:
  type: git
  repository: 你的github地址
  branch: main
```

**5. 安装 hexo-deployer-git 自动部署发布工具**

```js
npm install hexo-deployer-git --save
```

**6. 生成页面**

```js
hexo g
```

**7. 本地文件上传到Github上面**

> 中间可能会出现登录界面，可以用令牌登录。（令牌需及时保存，否则后面就看不到了）。
>
> 用户名即 github 注册账户用户名，令牌在 setting => Developer settings => Personal access tokens ，新建令牌，全部勾选后生成新令牌

```
hexo d
```



### 注意事项

 **报错1. 端口被拒绝 **

> 测试 SSH 链接出现报错  ssh：connect to host github.com port 22: Connection refused

```js
// 此报错可能是防火墙上屏蔽了22端口，github 提供了一种解决方案，允许你使用443端口进行ssh连接，大部分防火墙都会允许通过。
// 解决办法： 
// 1. 找到配置 SSH 时生成的 .ssh 文件夹，新建 config 文件(可以先新建config.txt，编辑完后重命名为 config)

// 2. 编辑 config.txt
Host github.com
User 你的邮箱
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443

// 3. 保存后重命名为 config 并再次测试 ssh 连接，出现提示输入 yes 即完美解决

```



**报错2：hexo init 报错**

> 执行 hexo init 后失败，错误内容如下

```
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
error: RPC failed; curl 28 OpenSSL SSL_read: Connection was reset, errno 10054
fatal: expected flush after ref listing
WARN  git clone failed. Copying data instead
FATAL {
  err: [Error: ENOENT: no such file or directory, scandir 'C:\Users\Blogger\AppData\Roaming\npm\node_modules\hexo-cli\assets'] {
    errno: -4058,
    code: 'ENOENT',
    syscall: 'scandir',
    path: 'C:\\Users\\Blogger\\AppData\\Roaming\\npm\\node_modules\\hexo-cli\\assets'
  }
} Something's wrong. Maybe you can find the solution here: %s http://hexo.io/docs/troubleshooting.html
```

原因：出现该错误大概率是因为 github 被墙导致网络连接失败

**解决办法：替换 GitHub 链接**

```js
// 找到 hexo 的 npm 模块中的 init.js 文件，打开文件并替换
// C:\Users\admin\AppData\Roaming\npm\node_modules\hexo-cli\lib\console\init.js

// 将 
const GIT_REPO_URL = 'https://github.com/hexojs/hexo-starter.git';
// 替换为：
const GIT_REPO_URL = 'https://github.com.cnpmjs.org/hexojs/hexo-starter.git';
```

替换完毕后再次运行 hexo init ，成功！



## butterfly 主题

### 1. 下载主题

[官网文档]()

```js
// 下载主题
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

### 2. 应用主题

```js
theme: butterfly
```

###  3. 安装插件

如果沒有 pug 以及 stylus 的渲染器，请下载安装：

```
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 4. 配置插件

> 官方推荐新建 _config.butterfly.yml 文件覆盖主题的 _config.yml
>
> 不删除原有的主题目录下的配置文件
>
> 之后配置主题在 _config.butterfly.yml 中配置即可，原有的主题配置会失效

为了减少升级主题后带来的不变，推荐在 hexo 根目录下创建新文件 `_config.butterfly.yml`，并将主题目录的 `_config.yml`内容复制进去。

hexo 会自动合并主题的配置文件，如果存在同名配置，会使用 `_config.butterfly.yml`文件的配置，其优先级较高。



### 注意事项

设置 `_config.butterfly.yml` 文件后，在里面引用其他文件如  .js、.css 文件时，路径是从 `source` 目录下开始的，且引用路径为相对路径 `/js/demo.js`，省略 .

[参考文章](https://b.leonus.cn/2022/custom.html)



## 文章链接设置

### hexo-abbrlink(推荐)

- 下载插件：`npm install hexo-abbrlink --save`
- 修改配置：`permalink: posts/:abbrlink/  `或 `permalink: posts/:abbrlink.html`
- 配置完毕后新创建的文章即会生效

```js
# 文章的 永久链接 格式
# permalink: :year/:month/:day/:title/
permalink: posts/:abbrlink.html
abbrlink:
# crc16 & hex https://post.zz173.com/posts/66c8.html
# crc16 & dec https://post.zz173.com/posts/65535.html
# crc32 & hex https://post.zz173.com/posts/8ddf18fb.html
# crc32 & dec https://post.zz173.com/posts/1690090958.html
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
# 永久链接中各部分的默认值
permalink_defaults:
# 改写 permalink 的值来美化 URL
pretty_urls:
  # 是否在永久链接中保留尾部的 index.html，设置为 false 时去除
  trailing_index: true 
  # 是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效)
  trailing_html: true 

```



## 天气插件

> 推荐使用 和风天气

[和风天气官网](https://widget.qweather.com/create-simple)

[参考文章](https://blog.csdn.net/weixin_58068682/article/details/125686850)

**注意**：务必注意 js 文件引入路径问题， 否则不会生效

#### 使用步骤：

- 打开官网注册账号并创建插件
- 生成代码后创建 `weather.js`文件并将代码复制进去
- 主题配置文件 `inject` 的 `bottom` 处引入两个 js 文件
- 在 `\themes\butterfly\layout\includes\header`路径下配置 `#he-plugin-simple`
- 在 `\themes\butterfly\source\css\_layout\head.styl` 目录下修改样式

