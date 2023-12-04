# 使用 Hexo + GitHub Pages 搭建个人博客

## 前言
这是本人第一次搭站，想法是先搭建个人博客练练手。查找过市面上推荐的博客框架，发现多数都需要有自己的服务器，作为入门推荐使用免费的 GitHub Pages。决定写一篇博客记录搭建过程，也作为我博客网站的首篇文章。

## 简介

### [GitHub Pages 是什么？](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)

GitHub Pages 是由 GitHub 官方提供的一种免费的静态站点托管服务，让我们可以在 GitHub 仓库里托管和发布自己的静态网站页面。
### [Hexo 是什么？](https://hexo.io/zh-cn/)
Hexo 是一个快速、简洁且高效的静态博客框架，它基于 Node.js 运行，可以将我们撰写的 Markdown 文档解析渲染成静态的 HTML 网页。
### Hexo + GitHub 文章发布原理
在本地撰写 Markdown 格式文章后，通过 Hexo 解析文档，渲染生成具有主题样式的 HTML 静态网页，再推送到 GitHub 上完成博文的发布。

![发文流程](/images/使用Hexo+GitHubPages搭建个人博客/发文流程.svg)
### 优点与缺点
**优点：** 完全免费，静态站点，轻量快速，可按需求自由定制改造，托管在 GitHub，安全省心，迁移方便。

**缺点：** 发文不便，依赖于本地环境；更适合个人博客使用；GitHub 在国内访问速度有点不快。


## 环境搭建
Hexo 基于 Node.js，搭建过程中还需要使用 npm（Node.js 已带） 和 git，因此先搭建本地操作环境，安装 Node.js 和 Git。
### 安装软件
- **Node.js：**  https://nodejs.org/en/download
- **Git：** https://git-scm.com/downloads

安装后以管理员身份运行 Windows PowerShell，依次输入以下命令`node -v`、`npm -v`、`git --version`，出现版本号即安装成功。
![软件安装成功](/images/使用Hexo+GitHubPages搭建个人博客/软件安装成功.png)

## 连接GitHub
使用邮箱注册 GitHub 账户并完成邮箱验证，然后设置本机的用户名和邮箱
```
git config --global user.name "GitHub 用户名"
git config --global user.email "GitHub 邮箱"
```

创建 SSH 密钥，用于 GitHub 与本地仓库的连接。

输入`ssh-keygen -t rsa -C "GitHub 邮箱`然后一直回车。

进入 `[C:\Users\用户名\.ssh]` 目录（要勾选显示“隐藏的项目”），用记事本打开公钥 `id_rsa.pub` 文件并复制里面的内容。

登陆 GitHub ，进入 Settings 页面，选择左边栏的 SSH and GPG keys，点击 New SSH key。

Title 可以填任意名字，粘贴复制的 id_rsa.pub 内容到 Key 中，点击 Add SSH key 完成添加。

![创建 SSH 密钥](/images/使用Hexo+GitHubPages搭建个人博客/创建SSH密钥.png)

打开 PowerShell，输入`ssh -T git@github.com`，如果返回`Hi xxx! You've successfully authent……`即表示连接成功。

### 创建 GitHub Pages 仓库
- GitHub 主页右上角 + 号 -> `New repository`
- 仓库名字格式必须为：`用户名.github.io`
- 点击 `Create repository` 创建

![创建仓库](/images/使用Hexo+GitHubPages搭建个人博客/创建仓库.png)

### 启用 HTTPS

打开刚刚创建的 GitHub Pages 仓库，点击 `Settings` ，在左边栏点击 `Pages` 选项，找到 `Custom domain`，输入 `用户名.github.io`，然后点击 `Save`，就可以打开你的博客了，博客地址为：`https://用户名.github.io`

![启用HTTPS](/images/使用Hexo+GitHubPages搭建个人博客/启用HTTPS.png)

## 搭建 Hexo

新建文件夹用来存放 Hexo 的程序文件，如 `Hexo-Blog`，然后 PowerShell(管理员模式) 进入该文件夹。

### 安装 Hexo 博客程序

使用 npm 一键安装 Hexo 博客程序：
```
npm install -g hexo-cli
```
安装时间有点久，耐心等待。
### Hexo 初始化和本地预览
初始化并安装所需组件：
```
hexo init      # 初始化
npm install    # 安装组件
```
完成后继续输入以下命令，启动本地服务器进行预览
```
hexo g   # 生成页面
hexo s   # 启动预览
```
访问 `http://localhost:4000`，出现 Hexo 默认页面，本地博客安装成功。

![Hexo 默认页面](/images/使用Hexo+GitHubPages搭建个人博客/Hexo默认页面.png)

如果 4000 端口被占用，可以使用 `hexo server -p 5000` 更改端口号运行。

Hexo 的目录结构如下：
```
.
├── _config.yml       # Hexo配置文件
├── package.json      # 项目依赖配置文件
├── scaffolds         # 模板文件夹
├── source            # 资源文件夹
│   ├── _drafts       # 草稿文件夹
│   ├── _posts        # 文章文件夹
│   ├── categories    # 分类文件夹
│   ├── tags          # 标签文件夹
│   └── ...           # 其他资源文件夹
└── themes            # 主题文件夹

```

### 部署 Hexo 到 GitHub Pages

本地博客测试成功后，就是上传到 GitHub 进行部署，使其能够在网络上访问。

安装 hexo-deployer-git：
```
npm install hexo-deployer-git --save
```
修改 `_config.yml` 文件末尾的 `Deployment` 部分，修改成如下：
```
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git 
  branch: main
```
完成后运行 `hexo d` 将网站上传部署到 GitHub Pages。
这时访问我们的 GitHub 域名 `https://用户名.github.io` 就可以看到 Hexo 网站了。