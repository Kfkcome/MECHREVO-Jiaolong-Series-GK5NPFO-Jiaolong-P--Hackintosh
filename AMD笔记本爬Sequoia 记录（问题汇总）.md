# AMD笔记本爬Sequoia 记录（问题汇总）还有问题需要求助

## 前情提要

暑假又闲着想折腾一下，所以想把amd笔记本升级下（虽然已经是14.5，但是还是想折腾一下升个15），原本的14.5已经接近完美了。升级之前先办所有驱动都升到了最新，并且oc是1.01。

最新驱动下载：

https://github.com/dortania/build-repo/releases

https://bbs.pcbeta.com/viewthread-2007546-1-6.html

https://bbs.pcbeta.com/viewthread-2006198-1-3.html

https://bbs.pcbeta.com/viewthread-2009312-1-2.html

还有好多，自己搜吧，感谢这些大佬。

## 升级过程

### 问题一：升级卡EB

![image-20240718103553984](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/image-20240718103553984.png)

这个问题卡了好久没有解决，思前先后，原来是内核补丁没有升级到sequoia的，以前只小版本升过，所以没有注意过。

解决方法：

1. 获取最新的支持sequoia的amd内核补丁

   https://github.com/AMD-OSX/AMD_Vanilla/blob/beta/patches.plist

2. 更新config.plist

   把这个上面的patch代码替换到你的config.plist对应kernel patch的位置上

3. 修改补丁中的cpu核心数

   这里我是用的是ocat进行编辑，其他的应该差不多

   ![QQ_1721270649223](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721270649223.png)

   把这个replace 的三、四位改成你的cpu核心数,这里我是8核的所以填08

   | Core Count | Hexadecimal |
   | ---------- | ----------- |
   | 4 Core     | `04`        |
   | 6 Core     | `06`        |
   | 8 Core     | `08`        |
   | 12 Core    | `0C`        |
   | 16 Core    | `10`        |
   | 24 Core    | `18`        |
   | 32 Core    | `20`        |



### 问题二：将之前disable的驱动补全后发现开机卡住

这个卡住的代码忘记拍了，后来逐步排查发现是nootedred.kext没有更新到最新

我是用的这个帖子的

https://bbs.pcbeta.com/forum.php?mod=viewthread&tid=2008132&extra=page%3D1%26filter%3Dtypeid%26typeid%3D1521%26typeid%3D1521

![](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721270925757.png)

更新了之后就好了

### 问题三：BFixup.kext失效了，qq非常卡，浏览器也是

这个问题真是困扰我好久，心里就像要是这个问题解决不了，真是没法用，所以熬夜搞定了这个问题。

这个BFixup.kext还是没有仓库里，github上找不到，是在nootedred里面提的issue中，有个第三方的大佬写的，暂时解决了硬解的问题，于是乎我追根溯源，找啊找，终于找到了， 原理github上的发这个的不是原作者，也是搬运的，原作者只在一个外国论坛里发。

并且幸运的发现作者已经跟进到了sequoia了嘻嘻。

然后我就发现了，一开始还下载不了，提示404资源失效，后来发现是登录后才能下载。

![QQ_1721271309859](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721271309859.png)

然后更新了这个驱动，qq什么的也不卡了也不花屏了（amd就是事多）

### 问题四：无线网卡不行了

https://bbs.pcbeta.com/forum.php?mod=viewthread&tid=2008953&extra=page%3D1%26filter%3Dtypeid%26typeid%3D1525%26typeid%3D1525

跟这个帖子的第一个评论来，就好了

### 问题五：触摸板不行了

按这个帖子的楼主的做法就行

https://bbs.pcbeta.com/viewthread-2005670-1-2.html

![QQ_1721271801429](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721271801429.png)

### 问题六：蓝牙不行了

https://bbs.pcbeta.com/viewthread-2006995-1-6.html

按这个来，一开始不知道，用来最新版的三件套，直接开机无线重启，后来才知道是

这个BTPatcher会引发崩溃，不能用，不用也能启动蓝牙。

![QQ_1721271936595](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721271936595.png)

### 问题七：无法启用任何来源的应用

`sudo spctl --allow-disable`

跟这位大佬说就好了

![image-20240718105933846](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/image-20240718105933846.png)

### 问题八：声卡不行了（希望有大佬能帮帮忙）

原本在14.5下声卡没啥问题，但是15就不行了。

我已经尝试了的：

1. 使用最新版appleALC，并且添加过了-alcbata参数
2. 尝试了所有对应的layout—id
3. 查看layout-id生效了

![QQ_1721272081439](./AMD%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%88%ACSequoia%20%E8%AE%B0%E5%BD%95%EF%BC%88%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB%EF%BC%89.assets/QQ_1721272081439.png)

4. 万能声卡更是不能用。



