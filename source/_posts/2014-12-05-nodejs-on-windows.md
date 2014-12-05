title: 在 Windows 上安装 node.js 和 SourceTree
date: 2014-12-05 08:55:14
tags:
- Windows
- nodejs
- sourcetree
- git
- bitbucket
---

本文将以 [demo-ds](https://bitbucket.org/creditcloud-demo/demo-ds) 为例介绍如何在 Windows 7 下搭建 node 开发环境。

首先访问 [node.js 官网](http://nodejs.org/) 点击 INSTALL 下载安装包，可以直接下载运行。

![node.js 官网](/archives/2014-12-05-nodejs-on-windows/1.png)

下载完成后的安装选项勾选上协议，然后一路下一步用默认值就可以。完成后，打开命令行，node 和 npm 命令都可以用了。npm 是 node 自带的包管理工具。

![node 和 npm 命令](/archives/2014-12-05-nodejs-on-windows/2.png)

我们还需要安装前端的包管理工具 bower，在命令行运行

    npm --registry=http://r.cnpmjs.org install -g bower

即可（这里使用了淘宝的 npm 镜像 cnpmjs.org，如果你访问国外网站通畅，以上命令可以简写为 `npm i -g bower` 使用 npmjs.org 源）。安装完成后可以运行 `bower -v` 确认 bower 已安装。

我之前遇见过提示 AppData/Roaming/npm 目录不存在的错误，此次写作本文时未遇见，如果有此情况，找到相应的 Roaming 目录下建立空文件夹 npm 即可。

接下来安装 SourceTree，也是到 [SourceTree 官网](http://www.sourcetree.com/) 下载 Windows 安装包，根据提示一路点下一步安装，如果提示需要安装 .net 4.5 运行环境，也按提示安装上，直到出现下面的画面：

![安装 SourceTree](/archives/2014-12-05-nodejs-on-windows/3.png)

1、3 选项必选，2 可以不选。

![提示创建 .gitignore](/archives/2014-12-05-nodejs-on-windows/4.png)

会提示创建全局的 .gitignore 文件，点 yes。

之后会提示用 bitbucket 或 github 等帐号登录，帮助你签出第一个项目，这里可以跟随它的向导，也可以跳过。这里先跳过。

安装好 SourceTree 后，bitbucket 项目页面上的 clone in SourceTree 按钮就可以点击，点击后签出到相关目录，这里我们用默认的 Documents/。

![Clone in SourceTree](/archives/2014-12-05-nodejs-on-windows/5.png)

![Clone in SourceTree](/archives/2014-12-05-nodejs-on-windows/6.png)

点击 SourceTree 工具栏右边的 Terminal，打开一个命令行默认在项目目录下，在此命令行中可以用 git 命令。

![terminal in SourceTree](/archives/2014-12-05-nodejs-on-windows/7.png)

第一次使用 SourceTree 的命令行工具，我们需要先运行一行命令：

    echo "PATH=\"/c/Program Files/nodejs:\$HOME/AppData/Roaming/npm:\$PATH\"" >> ~/.bash_profile

![first time echo PATH](/archives/2014-12-05-nodejs-on-windows/8.png)

这行命令让 SourceTree 的 terminal 以后能找到 node 程序，之后再重新点击 terminal 打开一个新命令行窗口，node, npm 以及之前我们全局安装的 bower 都可以用了：

![node in SourceTree terminal](/archives/2014-12-05-nodejs-on-windows/9.png)

现在就可以安装我们项目所依赖的 npm 包，在命令行运行

    npm --registry=http://r.cnpmjs.org install

此命令会读取 package.json 下的依赖包自动安装，之后进入 assets 目录安装前端库

    cd assets
    bower install

此命令读取 bower.json 里的依赖自动安装前端库，安装好后再回到项目根目录，可以运行了

    cd ..
    node index.js

可能会触发 windows 的防火墙，通过即可。

![firewall for node](/archives/2014-12-05-nodejs-on-windows/10.png)

至此，node 开发环境已经安装完成，现在可以访问 demo 的默认端口 http://127.0.0.1:1234/ 查看效果。开发过程中保持运行 node 的命令行窗口开启，需要退出时在命令行 ctrl+c 即可。

之后的开发环境运行方式、文件组织、编码规范等，请参考实际项目的 readme。


tips：  
如果你不想每次都打 `npm --registry=http://r.cnpmjs.org` 但因为网络问题想要都使用 cnpmjs 的镜像，可以运行一次

     echo 'alias cnpm="npm --register=http://s.cnpmjs.org"' >> ~/.bash_profile

之后只要运行 `cnpm i` 即可，cnpm 会被替换为 `npm --register=http://s.cnpmjs.org`, `install` 可以简写为 `i`。
