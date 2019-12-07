# gradle 测试/正式包同时安装,BuildConfig 配置小技巧


在开发的时候，你会需要经常切换正式/测试服务器地址开发,或者根据不同环境进行不同的配置,这时该怎么办?有人说，我手里头有两台测试机，一台装正式包，另一台装测试包。我想说，陈独秀同学，你先坐下，咱大多数人都只有一台测试机呀。
那么，问题来了，怎么实现一台手机上同时安装正式包和测试包呢。这就是本文要解决的问题。

<!--more-->

## 1. 前言

---

在开发的时候，你会需要经常切换正式/测试服务器地址开发,或者根据不同环境进行不同的配置,这时该怎么办?有人说，我手里头有两台测试机，一台装正式包，另一台装测试包。我想说，陈独秀同学，你先坐下，咱大多数人都只有一台测试机呀。
那么，问题来了，怎么实现一台手机上同时安装正式包和测试包呢。这就是本文要解决的问题。

## 2. 实现一台手机上同时安装正式包和测试包并进行区别配置

---

我们知道，Android 应用的唯一标识是包名，也就是 build.gradle 里的 applicationId。在一台手机上不允许安装的两个包的唯一标识重复。因此，只需要把测试包的 applicationId 亦即包名改一下就好了~

### 2.1 修改测试包包名

查阅文档之后发现，Android 官方对这种场景早有支持，只需要在 `app/build.gradle` 的 `android->buildTypes->debug` 节点下面设置 `applicationIdSuffix` 即可，示例如下：

```gradle
buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release //方便调试不要学
            buildConfigField "String", "HOST", "\"https:www.google.com\""
            buildConfigField "boolean", "SHOW_LOG", "false"
            applicationIdSuffix ".release"  //包名
            resValue "string", "app_name", "release"  //修改程序名
        }

        debug {
            signingConfig signingConfigs.release //方便调试不要学
            buildConfigField "String", "HOST",  "\"https:www.baidu.com\""
            buildConfigField "boolean", "SHOW_LOG", "true"
            applicationIdSuffix ".debug"  // 包名
            resValue "string", "app_name", "debug"  //修改程序名
        }

    }
```

### 2.2 区别配置

眼尖的小伙伴肯定发现了,为什么除了`applicationIdSuffix`这个以外,还有其它不懂的东西鸭! 问的好,这就是 gradle 另一个方便的类`BuildConfig`.
![BuildConfig_release][3]
![BuildConfig_debug][2]

根据图片我们可以看到,我们设置后 App 的`APP名`,`包名`,`HOST`,`SHOW_LOG`都发生了变化,这样大家可以根据自己的业务需求进行定制化.

## 3.实际效果演示

github Demo 地址: https://github.com/yikwing/BaseAndroidModel

![实际效果][1]

[1]: https://i.loli.net/2019/02/19/5c6bbf9b2f282.gif
[2]: https://i.loli.net/2019/02/19/5c6bc4c3e1002.jpg
[3]: https://i.loli.net/2019/02/19/5c6bc4c3df0ea.jpg
