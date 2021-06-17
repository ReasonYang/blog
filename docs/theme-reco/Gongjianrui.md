---
title: Typora
date: 2020-06-17
tags:
 - Gong
---

### 一、背景

在用Typora写博客文章时，首先是将文章写好，为了让文章更直观，一般情况下会在文章中插入图片（毕竟人是视觉动物，眼睛是十分强大的图片处理器）。但是在将编辑好的文章复制粘贴至博客时，会发现图片无法显示，此时最笨的办法就是一张张的图片上传至博客，可想效率之低下，于是在思考如何高效的将文章复制到博客。虽然，网络上已经有了很多的办法，但一直在尝试是否能寻找到更便捷高效的方法，最后站在各路互联网站巨人的肩膀写下此博文。

### 二、解决方案

解决Typora图片复制问题，其实本质就是各博客的API并非互联互通，所以需要先要将本地的图片上传至互联网上，然后博客才能访问。所以思路已经很清楚了。

- 方案：云平台（腾讯云等）+上传图片插件（typora-plugins-win-img）

### 三、注册云账号

##### 1、注册云账号

腾讯云地址：https://cloud.tencent.com/

##### 2、创建子用户

进入“访问管理”，然后点击“用户”，再点击“用户列表”，根据提示创建子用户

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225121110-59542.png)

然后，设置账户的访问权限，搜索“QcloudCOSFullAccess”，然后勾选了“QcloudCOSFullAccess”即可。

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225121344-155748.png)

访问方式选择“编程访问”，最后点击完成。

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225121839-795481.png)

##### 3、创建存储桶

进入“对象管理”，然后点击“存储桶列表”，再点击“创建存储桶”，然后点击“公有读私有写”，接着点击“不加密”，确定。

创建桶，并给桶取名字

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225122002-951908.png)

“创建存储桶”完毕后，再在“桶列表”里选择刚创建的桶，然后点击“创建文件夹”，此处保存Typora插件上传的图片。

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225122141-904081.png)

### 四、下载插件并配置

##### 1、下载插件

下载地址：https://github.com/Thobian/typora-plugins-win-img

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225124309-991742.png)

##### 2、拷贝插件文件

- 将插件文件夹复制到Typora安装目录

  将plugins文件夹复制至Typora安装目录下的app文件夹下，如图所示：

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225124440-290551.png)

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225124733-886939.png)

##### 3、配置window.html文件

来到Typora安装目录下的app文件夹下（比如：D:\Program Files\Typora\resources\app），修改window.html文件。

![技术图片](http://www.mamicode.com/Typora%E6%96%87%E7%AB%A0%E4%B8%80%E9%94%AE%E5%A4%8D%E5%88%B6%E8%87%B3%E5%8D%9A%E5%AE%A2.assets/20200225124958-966183.png)

用编辑器打开window.html文件，搜索（Ctrl+F可调出搜索功能）代码：

```
<script src="./app/window/frame.js" defer="defer"></script>

注意：搜索上述全部代码可能搜索不到，此时局部搜索即可，比如搜索src="./app/window/frame.js"
```

搜到到后，在其后面追加：

```
<script src="./plugins/image/upload.js" defer="defer"></script>
```

如图所示：

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225125516-548936.png)

##### 4、配置upload.js文件

同样，来到Typora安装目录下的app文件夹下的plugins（比如：D:\Program Files\Typora\resources\app\plugins\image），plugins文件夹是从插件typora-plugins-win-img复制过来的。

配置upload.js文件，在刚复制的plugins下，进入到plugins/image，配置upload.js文件。

![技术图片](http://www.mamicode.com/Typora%E6%96%87%E7%AB%A0%E4%B8%80%E9%94%AE%E5%A4%8D%E5%88%B6%E8%87%B3%E5%8D%9A%E5%AE%A2.assets/20200225125713-717342.png)

用文本编辑器打开upload.js文件，直接到达底部的$.image.init();一行，用以下部分替换之。

```
//为了你腾讯云的安全，强烈建议你为这个操作添加一个单独的子账号，并只开启API访问权限
//添加子账号：https://console.cloud.tencent.com/cam
//更多关于腾讯云子账号（CAM）说明：https://cloud.tencent.com/document/product/598/13665
$.image.init({
    target:'tencent',
    tencent : {
        Bucket: 'bucket-name',  // 对象存储->存储桶列表(存储桶名称就是Bucket)
        SecretId: 'SecretId',   // 访问控制->用户->用户列表->用户详情->API密钥 下查看
        SecretKey: 'SecretKey', // 访问控制->用户->用户列表->用户详情->API密钥 下查看
        Region: 'Region',       // 对象存储->存储桶列表(所属地域中的英文就是Region)
        folder: 'typora',       // 可以把上传的图片都放到这个指定的文件夹下
    },
});
```

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225130045-525204.png)

##### 5、配置说明

- Bucket: ‘bucket-name‘, 对象存储->存储桶列表(存储桶名称就是Bucket)

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225131107-809860.png)

- SecretId: ‘SecretId‘, 访问控制->用户->用户列表->用户详情->API密钥下查看

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225130744-334685.png)

- SecretKey: ‘SecretKey‘, // 访问控制->用户->用户列表->用户详情->API密钥 下查看

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225130749-402405.png)

- Region: ‘Region‘, 对象存储->存储桶列表(所属地域中的英文就是Region)

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225130505-845879.png)

- folder: ‘typora‘, 可以把上传的图片都放到这个指定的文件夹下

##### 6、防盗链配置

为了避免恶意程序使用资源 URL 盗刷公网流量或使用恶意手法盗用资源，带来不必要的损失。腾讯云对象存储支持防盗链配置，通过控制台的防盗链设置配置黑/白名单，来进行安全防护。

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225131759-654105.png)

##### 7、重启Typora

保存好修改的配置，然后重启Typora上传图片，显示成功。

![技术图片](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20200225132525-541790.png)