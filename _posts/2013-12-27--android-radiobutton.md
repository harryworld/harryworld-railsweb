---
layout: post
title: "如何自定义 Android RadioButton"
description: ""
category: "Android"
tags: ["Android"]
comments: true
---
我们经常使用RadioButton来做单选，但是Android的Radio Button在不同版本下显示的不一样（4.0后Holo 风格很大程度上改善了这一点）

以1为基准，以下均使用320×480分辨率

首先我们来使用默认的按钮看看

```
<RadioGroup android:id="@+id/act_main_repayment_grp"
   android:layout_width="match_parent" android:layout_height="wrap_content"
 android:layout_toRightOf="@id/act_main_repayment_tv" android:layout_marginLeft="29dp"android:background="@color/light"  android:layout_marginTop="10dp"
android:orientation="horizontal" >

  <RadioButton android:id="@+id/act_main_repayment_benjin_btn"
    android:layout_marginLeft="10dp"   android:text="测试1" />

  <RadioButton android:id="@+id/act_main_repayment_benjin_btn"
    android:layout_marginLeft="10dp"   android:text="测试2" />

</RadioGroup>
```

这是2.3显示的风格
![Screen Shot 2013-07-30 at 12.13.13 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/cQY2qkDkSEijsvkDOicL_Screen%20Shot%202013-07-30%20at%2012.13.13%20PM.png)

我们来看看4.0显示的风格
![Screen Shot 2013-07-30 at 12.13.04 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/K3QnFYJ7TPOvF0nXA8cf_Screen%20Shot%202013-07-30%20at%2012.13.04%20PM.png)



你会发现2.3.3按钮跟4.0完全不一样，现在还只是在原生的Android机器上，还有各家定制的UI，你快疯了吧，毫无疑问，你得要用自己的图片来做

```建立 res/drawable/ic_radio.xml

<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="true" android:drawable="@drawable/ic_radio_on" />
 <item android:state_focused="true" android:drawable="@drawable/ic_radio_off" />
<item android:state_checked="true" android:drawable="@drawable/ic_radio_on"/>
  <item android:drawable="@drawable/ic_radio_off" />
</selector>
```
好啦，我们给我们的第一个raido button加上样式吧

```
<RadioButton android:id="@+id/act_main_repayment_benxi_btn"  android:checked="true" android:button="@drawable/ic_radio"
  android:text="测试1" />
```

为了更好的显示出来，我在他们前面加多了一个textview，上面为2.3.3，下面是4.0

![Screen Shot 2013-07-30 at 12.26.42 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/65iIwfymQeCILpsU5uG9_Screen%20Shot%202013-07-30%20at%2012.26.42%20PM.png)

![Screen Shot 2013-07-30 at 12.26.35 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/xkkvhDQNQqeWsIgkTL4R_Screen%20Shot%202013-07-30%20at%2012.26.35%20PM.png)



图片变了，可是，为毛他还占用那么多位置？

你会发现2.3的文字很友好的帮你做了padding，而4.0很愚蠢的贴边，还没奔溃吧，button这个属性压根不能用！

我们使用android:drawableLeft来放我们的图片吧，然后加个简单的padding

```
<RadioButton   android:id="@+id/act_main_repayment_benxi_btn"
   android:checked="true"              android:drawablePadding="5dp" android:drawableLeft="@drawable/ic_radio"  android:button="@null" android:text="测试1" />
```

照旧，上面是2.3.3，下面是4.0

![Screen Shot 2013-07-30 at 12.34.21 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/o9UTPsrVQ0q8iu5EtxLZ_Screen%20Shot%202013-07-30%20at%2012.34.21%20PM.png)
![Screen Shot 2013-07-30 at 12.34.12 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/eZPDyjzRRKe4FZUD04ot_Screen%20Shot%202013-07-30%20at%2012.34.12%20PM.png)




2.3.3的测试2的Radio Button要离家出走了，FML~ 还有他为什么还要占用那么多地方，喝多了？

经过一轮的google和不断的尝试，po主终于找到问题了，是2.3的background在作怪！

我们修改成如下代码

```
<RadioButton  android:id="@+id/act_main_repayment_benxi_btn"
  android:checked="true" android:background="@null"
  android:drawablePadding="5dp" android:drawableLeft="@drawable/ic_radio"              android:button="@null" android:text="测试1" />
```

上面是2.3，下面4.0，看这个长的一样了吧
![Screen Shot 2013-07-30 at 12.40.25 PM.png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/HkjBDGxSPGiWtGoWAyI6_Screen%20Shot%202013-07-30%20at%2012.40.25%20PM.png)
![Screen Shot 2013-07-30 at 12.40.25 PM (1).png](http://user-image.logdown.io/user/5050/blog/5043/post/161018/Q1rU9wedTiosi5AvVnRf_Screen%20Shot%202013-07-30%20at%2012.40.25%20PM%20(1).png)





