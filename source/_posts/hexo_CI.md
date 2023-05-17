---
abbrlink: '0'
date: 2021-08-29 23:13:43.287202+08:00
title: 利用Github的Action功能实现自动部署到Git Page

---
# 利用Github的Action功能实现自动部署到Git Page
关键步骤如下：
1. 在Github建立Hexo项目仓库和Github项目仓库
2. 分配一对部署用的密钥
   1. 其中Hexo项目需要将私钥添加到*Repository Secret*
   2. 在Git Page仓库中添加公钥到Deploy Key，并允许写权限
3. 在Hexo项目仓库下的`.github/workflows/hexo_build.yml`中编写Action脚本
   1. 使用`push`事件（表示每次push到Github时执行该Action）
   2. 需要`checkout`并从头安装`npm`环境
   3. 需要利用*Repository Secret*特性来生成私钥，以便部署中的`deploy`推送到Git Page的仓库中
4. 大功告成，每次`push`都会自动运行部署命令部署到Git Page