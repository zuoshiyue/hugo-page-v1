+++
title = '飞牛 DDNS网络'
date = 2024-11-29T11:38:40+08:00
menu = ['posts']
tags = ['nas', 'ddns']
+++

## 飞牛 DDNS网络

飞牛nas的基本应用搞完了，看了一下应用中心，应用也不多，好在有docker。docker极大丰富了nas的可玩性（主打一个折腾，哈哈）。这样就涉及到几个问题了：

1、官网提供的FN ID中继转发有限速

2、docker应用大部分要使用不同的端口号，就算使用heimdall，Dashy，homepage，外网访问要[路由器](https://www.smzdm.com/fenlei/luyouqi/)要设置很多端口转发

3、考虑外网访问安全性，希望所有访问内部的都走https

4、内网穿透、VPN都涉及稳定性和客户端软件的问题

思来想去，还是考虑用公网IP+DDNS+反向代理来解决

接下来涉及的内容有点多，我也讲主要的，有些细节内容大家可以网上搜索一下

一、首先是要弄个公网IP（当然是动态的，后面用DDNS解决，固定的费用个人可玩不起）

还好家中网络是电信，以前也是想到有这个好处。不过电话打到10000，客服非说必须要办理3位数（X99）的套餐才可以开通。没办法，联系到电信在小区的技术老师，私底下给开通了（嘘，小声点...）。有些地区移动、联通的二级网络，也可以开通一个加端口转发服务的套餐

二、将光猫设置为桥接

电脑浏览器登录家中光猫，将光猫设置为桥接。光猫背面可看登录账号/密码，如果不会操作可以联系到小区网络技术老师提供帮助

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671df493deba34122.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_2/)

三、设置为路由器拨号

电脑浏览器登录家中主路由器，在网络设置（外网）中，设置为PPPoE拨号，以我的ASUS路由器为例：

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671df52143ed64351.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_3/)

四、检查路由器是否正确获得公网IP

[百度](https://pinpai.smzdm.com/3357/)搜：IP地址查询，从结果网页中查外网IP

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671df660f66bf9119.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_4/)

再看路由器上的外网IP地址：

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671df6dae57c06911.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_5/)

如果两个IP一致，就是正常获取到公网IP了

当然飞牛nas中也提供看外网IP

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671df749d46053506.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_6/)

五、注册一个自己的域名

[华硕](https://pinpai.smzdm.com/1681/)路由器是可以使用asuscomm.com、花生壳等DDNS，但这个DDNS本身就是二级域名，你只有这1个二级域名（如myasus.asuscomm.com），无法实现不同的二级域名访问不同的nas内部应用或内部不同的电脑。另外华硕的DDNS不知为啥前段时间一直IP更新不及时。还有就是自动证书的问题，所以还是自己申请一个吧。正好前几天看到[腾讯](https://pinpai.smzdm.com/19011/)云的国际顶级域名.cn才8.8一年，先申请个玩玩吧。

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671dfa65e3e5c1386.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_7/)

选个.cn域名吧

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671dfa8df402c3561.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_8/)

域名申请需要注册、提交实名资料并审批，等待审批完成

六、设置域名DNS解析

这里主要就是为你的域名添加A记录，为后续访问飞牛nas方便，可以添加泛解析

先进入腾讯云控制台

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671dfc8d8dde38399.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_9/)

点开域名，点解析

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671dfd1237bf68666.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_10/)

进入域名解析控制台，没有记录，点添加记录

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671dfd772245f9266.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_11/)

添加一个泛解析。主机记录填入\*，记录值随意录个IPV4如：8.8.8.8（后面会从飞牛nas更新正确的公网IP回来）

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671dfe1ae25352190.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_12/)

要让外部系统把路由器的公网IP更新到腾讯云该域名的记录值中，必须要在腾讯云创建API密钥，外部系统（如飞牛nas）填入正确的SecretId、SecretKey，才能有权限更新到腾讯云中。进入腾讯云的访问管理->API密钥管理

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671e00a3e621c1580.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_13/)

创建密钥，注意创建时保存好密钥，后续将无法查询密钥

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671e00f18a0f74993.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_14/)

吐槽一下，腾讯的服务实在是太多了，这个腾讯云的菜单和网站路径挺复杂的，转来转去，官方的帮助也没有更新，找个配置菜单配置项实在费劲，也可能是我平时弄得比较少，哈哈

七、设置飞牛DDNS

到此就可以在飞牛nas上设置DDNS，以便飞牛nas及时将变动后的公网IP传回到腾讯云你的域名解析记录中去

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671e02868c2e19051.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_15/)

选择DDNS服务商为腾讯云，录入DDNS为对应腾讯云上的泛域名解析域名，如：\*.xxxxxx.cn，填上腾讯云中的API密钥，点测试连接，如显示成功就大功告成。可转到腾讯云看一下记录管理中的公网IP是否正确了（跟上图的外部IPV4地址相同，也应该跟路由器上的外网IP地址相同）

[![飞牛nas（三、ddns网络篇）](https://am.zdmimg.com/202410/27/671e03716ce205394.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_16/)

八、路由器设置端口转发

现在，你已经可以通过泛域名访问到你的路由器了，如asus.xxxxxx.cn、nas.xxxxxx.cn，都会指向你的主路由器的外网IP。但如何访问路由器后面的飞牛nas呢？这就需要路由器上设置端口转发了。例如飞牛nas的内网访问是：http://192.168.0.250:8000，则路由器这样设置：

[![飞牛nas（三、ddns网络篇）](https://qnam.smzdm.com/202410/27/671e056d86bc15141.png_e1080.jpg)](https://post.smzdm.com/p/axodle6d/pic_17/)

现在，你就可以在外网，浏览器地址录入：http://nas.xxxxxx.cn:10080，访问到飞牛nas了

同理，飞牛移动端APP，也可以录入：nas.xxxxxx.cn:10080这个地址来连接飞牛nas

当然，因为是设置的泛域名方式，你录a.xxxxxx.cn:10080，b.xxxxxx.cn:10080，都是可以访问到的

好，现在余下的问题就是：

1、怎么保证通过https访问保证安全

2、怎么让路由器只开放一个端口转发，就能访问不同的[服务器](https://www.smzdm.com/fenlei/fuwuqi/)不同的应用

上面问题涉及到证书、自动证书、反向代理等内容，下回再讲

作者声明本文无利益相关，欢迎值友理性交流，和谐讨论～

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)
