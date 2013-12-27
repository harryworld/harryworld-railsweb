---
layout: post
title: "如何制作成第三方Android Libary"
description: ""
category: ""
tags: ["Android"]
comments: true
---
####1.把项目设置成Libary Project

![Screen Shot 2013-12-15 at 9.28.03 AM.png](http://user-image.logdown.io/user/5050/blog/5043/post/166764/y4pBgFgRQAmvXTyIEJPn_Screen%20Shot%202013-12-15%20at%209.28.03%20AM.png)


####2.建议引用到得包用源代码链接的形式加入

![Screen Shot 2013-12-15 at 9.30.20 AM.png](http://user-image.logdown.io/user/5050/blog/5043/post/166764/ALDA9zmqSmKnKhJwQCdE_Screen%20Shot%202013-12-15%20at%209.30.20%20AM.png)

> Properties > Java Build Path > Source > Link Source

![Screen Shot 2013-12-15 at 9.33.54 AM.png](http://user-image.logdown.io/user/5050/blog/5043/post/166764/tHVaQKhxShSBjfIvqt8l_Screen%20Shot%202013-12-15%20at%209.33.54%20AM.png)

![Screen Shot 2013-12-15 at 9.30.33 AM.png](http://user-image.logdown.io/user/5050/blog/5043/post/166764/3QgsuAxeTpmM0yuVnRDu_Screen%20Shot%202013-12-15%20at%209.30.33%20AM.png)

####3.资源文件改成按名称查找

```
//ResourceUtils.java
public class ResourceUtils {

   public static int getResourseIdByName(String packageName, String className, String name) {
         Class r = null;
         int id = 0;
      try {
          r = Class.forName(packageName + ".R");

          Class[] classes = r.getClasses();
          Class desireClass = null;

          for (int i = 0; i < classes.length; i++) {
              if(classes[i].getName().split("\\$")[1].equals(className)) {
                  desireClass = classes[i];

                  break;
              }
          }

          if(desireClass != null)
              id = desireClass.getField(name).getInt(desireClass);
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      } catch (IllegalArgumentException e) {
          e.printStackTrace();
      } catch (SecurityException e) {
          e.printStackTrace();
      } catch (IllegalAccessException e) {
          e.printStackTrace();
      } catch (NoSuchFieldException e) {
          e.printStackTrace();
      }

      return id;

      }
}

```

你可以改成这样去使用你的资源文件

```
findViewById(ResourceUtils.getResourseIdByName(this.getPackageName(), "id", "activity_chat_message_edittext"));
```

####4.包名规范
因为涉及到打包跟代码混淆，所以建议包名
com.xx.android.*  这里的代码是进行混淆的，使用者无法看到真实代码
com.xx.*  这里代码不进行混淆，使用可以可以反编译跟调用代码

####5.打包

- 1.使用ant
参考，创建你的build.xml <http://developer.android.com/tools/projects/projects-cmdline.html>
- 2.混淆

```
//proguard-project.txt

-keep public class * extends android.app.Activity
-keep public class * extends android.app.Application
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class com.android.vending.licensing.ILicensingService
-libraryjars libs/xx.jar

-dontwarn com.xx.**
-keep class com.xx.**  { *; }
-keep interface com.xx.**  { *; }
-keep public class * extends com.xx.**

```

>其中xx.jar是你的libary project的bin下产生的，新建多一个专门打包的项目，然后对整个jar进行混淆，这样最方便跟安全，具体的混淆语法可以移步<http://developer.android.com/tools/help/proguard.html>
> 打包完成后, 在 **bin/proguard/obfuscated.jar** 就是已经混淆过的jar包了
> 把你res资源也拷贝给别人，那么别人就能使用你的libary进行第三方libary引用了