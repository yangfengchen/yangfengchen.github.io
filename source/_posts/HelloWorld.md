---
title: HelloWorld
date: 2020-06-06
tags:
---

# 搭建hexo博客

1、准备node.js、git环境

2、在github中创建一个仓库，仓库名为：xxxxx.github.io

3、创建hexo

​	npm install -g 文件名称(后文中blog指该文件夹)

​	#npm install -g hexo

​	hexo init

4、创建一个博客文章进行内容编写

​	在blog/source/_posts执行命令：

​	hexo new <模板> <文件名称>

​	

| 参数  | 参数备注         | 文件路径         |
| ----- | ---------------- | ---------------- |
| post  | 新建文章         | /source/_posts/  |
| draft | 新建草稿         | /source/_drafts/ |
| page  | 标签页，分类页等 | /source/         |

5、预览博客文章

hexo g && hexo s #生成启动服务

6、将博客部署到git上

​	在blog/_config.yml添加以下部分

​	deploy:

​		type: git

​		repository: https://github.com/xxx/xxx.github.io

​		branch: master

​	执行命令

​	hexo g && hexo d #生成发布到github上

7、更换主题

https://hexo.io/themes/

找到对应的主题下载下来放到blog/themes目录下

坑点：需要看对应主题需要修改什么配置文件

8、多端备份主要逻辑思路

一、在github上创建一个项目或者在上面的项目创建一个分支

二、把blog目录下的文件除开node_modules不copy其余都copy

三、增加.gitignore

.DS_Store
Thumbs.db
db.json
.log
node_modules/

/.deploy_git

public/
.deploy/
/public

四、提交项目

命令备忘

hexo clean #更换主题时清理

npm install hexo-asset-image --save