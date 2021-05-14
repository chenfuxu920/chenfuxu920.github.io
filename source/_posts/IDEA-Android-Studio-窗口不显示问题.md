---
title: IDEA/Android Studio 窗口不显示问题
toc: true
date: 2020-06-08 17:36:50
tags: [Android Studio, IDEA]
categories: IDEA
---
今天在双屏使用`IDEA`时，拖动`IDEA`到另一块屏幕上的时候，窗口不显示了，网上搜这个问题时，说的方法大多为修改`workspace.xml`中的`ProjectFrameBounds`参数，但是我在项目的`.idea`目录下的`workspace.xml`中并没有发现`ProjectFrameBounds`参数，这其实是因为新版的`IDEA`配置管理是分散在不同的目录的，具体操作如下
1. 打开`idea.config.path`配置的文件夹，默认为C盘User目录下，`.IntelliJIdea`目录
2. 打开`options\window.state.xml`，找到`"WindowManager"`，修改`frame`元素中的x，y，建议改为0
3. 打开`ptions\recentProjects.xml`,找到你的项目，修改`frame`元素中的x，y，并且记下`projectWorkspaceId`
4. 打开`workspace`目录，找到`${projectWorkspaceId}.xml`，也就是前面`projectWorkspaceId`对应的文件，修改`ProjectFrameBounds`元素中的x，y