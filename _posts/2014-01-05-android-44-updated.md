---
layout: post
title: "浅谈Android4.4.2的更新"
description: ""
category: ""
tags: []
comments: true
---

我的Nexus 7 一直是使用的4.3，因为懒得在折腾Root的事情，就一直放着4.4没更新

在Android里面，如果没Root会很困扰，国内很多应用都没完没了的跑在后台，推送，所以没有绿色守护的话，就很麻烦了。


4.4更新了许多东西，但更多是针对手机进行优化。

###4.4部分改进

 - 新的电话拨号应用，可快速搜索人名，甚至是地名
 - Hangouts直接整合了原来的短信应用，现在用Hangouts可以解决短信、网络文字、视频、彩信所有事情，还可分享当前的位置（Hello iMessage），发送的照片不限本地，也包括在Google Drive云端或任何其他云存储服务里的图片
 - 摄像机加入新的HDR+模式，可一次拍摄多张照片选出各张里最好的部分拼接成一个最佳的照片（目前只支持Nexus 5，未来可能会逐渐支持老手机）
 - 无线打印框架支持，任何文件都可被支持
 - 直接说“OK, Google”即可开始语音搜索，语音识别率提升
Google Now现在改成从左向右滑动屏幕开启了，可解决用户的更多问题和提问，也利用了众包来获取更好的推荐答案。比如Google发现斯坦福的学生会在秋季经常查询课表，那它一旦判断出你也是斯坦福的学生，就会自动给你推送课程表
在进行Google搜索的时候，也许会看到“Open in App 某某”的按钮，说明Google可直接将搜索结果转变成应用里的相应页面呈现给你
 - 支持安全的NFC传输协议Host Card Emulation来做支付
 - 支持批量处理硬件感应器，而非一个一个调用，适合低功耗的长期需要感应器的健康应用，直接支持计算出步伐数
 - 应用支持全屏模式，下面的虚拟按钮和上面的状态栏全部不见，给你完全的全屏享受
 - 可直接录制屏幕视频，保存为MP4文件
可在回放视频时自动切换分辨率
 - 支持HTTP Live Streaming V7
 - [更多对开发者的改进支持请看Android博客](http://developer.android.com/about/versions/kitkat.html)

##新的虚拟机ART

Google非常低调的在4.4推出了新的虚拟机***ART***,这里普及一下。

####Dalvik虚拟机
在4.4以前，在过去，安卓的应用程序由 Dalvik Java 虚拟机运行，Dalvik 依靠一个 Just-In-Time(JIT) 编译器去向硬件“解释” App 字节码，代码和硬件打交道时平白无故多出一个解释过程，显而易见，这种方式并不能直接调用底层的硬件，而是通过了一个中间介绍人来让 App 运行，这就是为什么搭载 Android 系统的手机相比 iPhone 来说耗电快，软件占内存大，卡顿严重。从而 Dalvik 被看作安卓运行效率低下的“毒瘤”。当然，Dalvik 虚拟机让应用能更容易在不同硬件和架构上运行，是安卓系统普及的功臣。


####ART虚拟机
Android 操作系统已逐渐成熟，谷歌开始将注意力转向一些底层组件，谷歌已经花了很长时间开发更快执行效率更高、更省电的 ART 运行时。自 Android 4.4 开始，谷歌将逐渐用 ART 运行时替代 Dalvik。而新的 ART 则完全改变了 Dalvik 这套做法，其处理应用程序执行的方式完全不同于 Dalvik，在应用安装时，ART 就直接把代码预编译成机器语言，这一机制叫 Ahead-Of-Time (AOT）编译。和 Dalvik 相比，经过 ART 编译后的应用从根本上省略了解释字节码这个过程，运行起来更有效率、耗电更少、占的内存也更低。当然，预编译也带来了两个问题，一个是应用占用的存储空间将会更大，另一个是这个过程也会让应用安装耗时更长。预编译的 App 体积会大一些，安装时间则要看 App 本身的复杂程度。不过，App 的安装过程只有一次，相信大部分人是能忍受这个时间的。

而art会利用LLVM，真正意义上把APK中的dex编译为本地代码镜像，就像Windows Phone的使用的.Net本地代码镜像那样。

了解更多:[BREAKING: New Runtime Compiler in Android 4.4 to Possibly Bring Better Performance in Future Releases](http://www.xda-developers.com/android/new-runtime-compiler-in-android-4-4/)

##Advertising ID
使用新追踪技术替代Cookie

网易：[Google将用新的追踪技术取代Cookies](http://tech.163.com/13/0922/15/99CSP6SM000915BF.html)

Android Developer: [Advertising ID](https://developer.android.com/google/play-services/id.html)

##其他相关
 - kitkat 的出现代表Android开始进入封闭的生态圈了，Google正式完全接管Android生态系统
 - 要调用地图api得用google play service
 - 要调用用户追踪的识别符只能用google play service生成的ad id
 - Hangouts正式代替原有的短信
 - 通话开始调用Google自己的云端黄页
 - 启动器变成了Google Now的一部分(仅限于Nexus 5)


##部分看法

笔者使用了自己Nexus 7进行测试，更换虚拟机后，会重新把所有应用更新一遍，粗略估计每个应用安装的时间是之前的2倍，而且体积增加了10-20%左右，兼容性看起来大部分应用都没问题，至少笔者使用的应用都没有出现奔溃的情况，在速度并没有特别大的感觉。

在目前来说，Android所以市场占有率上远远超过IOS，但是碎片化问题依旧无法解决。目前只有Nexus系列的终端能第一时间接受到新版本的推送。其他终端需要等到厂商完成定制才能进行更新。而且很多旧的机型因为厂商无法支持而无法使用最新的版本，这次4.4的版本优化了底层，使得部分512M的机器都能顺利升级去，目的就是为了解决碎片。相信这次下次Android5.0发布时，很多厂商会为了兼容ART一并把目前老的UI进行修改，以便能更好的兼容。

