---
title: SqlSever
date: 2020-06-17
tags:
 - Yu
categories:
 -  category2
---

# SQLServer数据库备份与还原

## 问题描述

​       最近需要给程序新增功能，用于将旧格式的数据转换为新格式，同时删除旧格式的数据（新旧格式的数据库表有部分重叠，同一份数据无法同时存在新旧格式的数据），由于测试环境中的测试数据不多，功能调试几次之后就没有旧格式的数据做测试了，因此想到在功能调试前先将测试数据库备份，然后功能调试之后再将测试数据库还原，这样就可以重复的进行功能调试。
  数据库备份过程比较顺利，但是还原过程中出现错误，无论是还原数据库还是还原数据库文件都报错：
  还原数据库时报下面错误：
[![2xDorT.png](https://z3.ax1x.com/2021/06/17/2xDorT.png)](https://imgtu.com/i/2xDorT)

​     还原数据库文件时报下面错误：

[![2xDIMV.png](https://z3.ax1x.com/2021/06/17/2xDIMV.png)](https://imgtu.com/i/2xDIMV)

​       通过百度资料，最终解决了还原数据库出错的问题，现将数据库备份和还原的步骤列在下面，以备后用。

## SqlServer数据库备份步骤

1.首先在本地磁盘上建一个备份文件夹，如果不想单独建个文件夹的话，使用SqlServer默认的备份文件夹也可以。本例中在本地K盘建立一个数据库备份文件夹。

[![2xsA7F.png](https://z3.ax1x.com/2021/06/17/2xsA7F.png)](https://imgtu.com/i/2xsA7F)



2）打开SqlServer客户端，在需要备份的数据库上点右键，选择任务->备份，弹出备份数据库窗口。

[![2xymVS.png](https://z3.ax1x.com/2021/06/17/2xymVS.png)](https://imgtu.com/i/2xymVS)

 3）在备份数据库窗口下方删除默认的备份文件，然后点击添加按钮，选择步骤1中建立的文件夹作为备份文件夹，接着给一个备份文件的名称。点击确定按钮返回备份数据库窗口。

[![2xyEKP.png](https://z3.ax1x.com/2021/06/17/2xyEKP.png)](https://imgtu.com/i/2xyEKP)

[![2xyZb8.png](https://z3.ax1x.com/2021/06/17/2xyZb8.png)](https://imgtu.com/i/2xyZb8)

4）在备份数据库窗口中点击确定按钮进行备份，弹出备份成功的提示。然后到步骤1中建立的文件夹中查看，这时已经存在备份文件了。

[![2xykvt.png](https://z3.ax1x.com/2021/06/17/2xykvt.png)](https://imgtu.com/i/2xykvt)

[![2x6wTS.png](https://z3.ax1x.com/2021/06/17/2x6wTS.png)](https://imgtu.com/i/2x6wTS)

## **SqlServer数据库还原步骤**

 1）如果数据库是多个客户端在连接，在还原之前，首先要把数据库的连接方式设置为单一连接。打开SqlServer客户端，在需要还原的数据库上点右键，选择属性，弹出数据库属性窗口。

[![2xyVDf.png](https://z3.ax1x.com/2021/06/17/2xyVDf.png)](https://imgtu.com/i/2xyVDf)

 2）在数据库属性窗口右侧的其它选项中，在状态分组中将限制访问属性的值从MULTI_USER变成SINGLE_USER,然后点击确定按钮返回。

[![2xcC6I.md.png](https://z3.ax1x.com/2021/06/17/2xcC6I.md.png)](https://imgtu.com/i/2xcC6I)

3）在需要还原的数据库上点右键，选择任务->还原->文件和文件组，弹出还原文件和文件组窗口。

[![2xymVS.png](https://z3.ax1x.com/2021/06/17/2xymVS.png)](https://imgtu.com/i/2xymVS)

 4）在还原文件和文件组窗口中，将还原的源设置为源设备，然后点击右边的浏览按钮，选择数据库备份文件夹中的备份文件，然后点击确定按钮返回还原文件和文件组窗口。

[![2xyMCj.png](https://z3.ax1x.com/2021/06/17/2xyMCj.png)](https://imgtu.com/i/2xyMCj)

[![2xyQ8s.png](https://z3.ax1x.com/2021/06/17/2xyQ8s.png)](https://imgtu.com/i/2xyQ8s)

5）在还原文件和文件组窗口下方的选择用于还原的备份集中勾选刚才选中的备份文件

[![2xyl2n.md.png](https://z3.ax1x.com/2021/06/17/2xyl2n.md.png)](https://imgtu.com/i/2xyl2n)

 6）点击还原文件和文件组窗口左上角的选项，然后勾选覆盖现有数据库选项，最后点击确定按钮进行还原。还原成功后，会弹出数据库还原成功提示窗口。

[![2xy1vq.png](https://z3.ax1x.com/2021/06/17/2xy1vq.png)](https://imgtu.com/i/2xy1vq)

[![2xy8K0.png](https://z3.ax1x.com/2021/06/17/2xy8K0.png)](https://imgtu.com/i/2xy8K0)

## 其它



  照着上述方式可以多次还原数据库，最终也完成了功能调试。但是每次还原的时候都要手动操作，太费事儿，如果能将上述操作编成数据库脚本，然后一键还原就好了！