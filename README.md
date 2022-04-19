# AndroidPlatformWiki

<p align='center'>
<a href="https://github.com/pengxurui/AndroidPlatformWiki" target="_blank"><img alt="GitHub" src="https://img.shields.io/github/stars/pengxurui/AndroidPlatformWiki?label=Stars&style=flat-square&logo=GitHub"></a>
</p>

<p align='center'>
<a href="https://www.github.com/pengxurui" target="_blank"><img src="https://img.shields.io/badge/作者-@彭旭锐-000000.svg?style=flat&logo=GitHub"></a>
<a href="https://github.com/pengxurui/Android-NoteBook/raw/master/images/搜一搜公众号.png" target="_blank"><img src="https://img.shields.io/badge/公众号-@彭旭锐-000000.svg?style=flat&logo=WeChat"></a>
<a href="https://juejin.cn/user/1063982987230392" target="_blank"><img src="https://img.shields.io/badge/掘金-@彭旭锐-000000.svg?style=flat&logo=JueJin"></a>
<a href="https://www.zhihu.com/people/pengxurui" target="_blank"><img src="https://img.shields.io/badge/知乎-@彭旭锐-000000.svg?style=flat&logo=Zhihu"></a>
</p>

![公众号](https://github.com/pengxurui/Android-NoteBook/raw/master/images/搜一搜公众号-文字-白色版.png)

### 使用方法

**1、先给一个 Star，你的支持对我非常重要**。我的内容质量绝对对得起你的 Star，给我一点创作的动力，感谢。

**2、加我微信，进小彭的 Android 交流群**。我们对群质量有要求，你可以在这里找到志同道合的朋友。群里可以讨论技术、分享文章、聊天、吐槽，允许适当发招聘广告，不受欢迎的行为是严格禁止的：

<p align='center'>
<img src="https://github.com/pengxurui/Android-NoteBook/raw/master/images/个人微信.jpeg" width = "200" />
</p>

**3、关注我的公众号 [彭旭锐]，坚持高质量原创内容，不人云亦云**，公众号后续是我主要的内容更新平台：

<p align='center'>
<img src="https://github.com/pengxurui/Android-NoteBook/raw/master/images/公众号.jpg" width = "230" />
</p>

**4、关注我的 [掘金](https://juejin.cn/user/1063982987230392)、[知乎](https://www.zhihu.com/people/pengxurui) 和 [Android 知识体系](https://github.com/pengxurui/Android-NoteBook)**，掘金上有我历史发布过的所有文章，Android-NoteBook 是我参考杜威十进制模型搭建的 Android 知识体系，你可以参考我的模型定制专属的知识体系。

---
## 1. Android 系统的今天和明天

从 2008 年 9 月 23 日，Android 的第一个版本 Android 1.0 发布。到 2021 年 10 月 4 日，Android 12 正式版发布。不知不觉，Android 操作系统系统走过了十几个年头了。

![Android 1.0](https://user-images.githubusercontent.com/25008934/164010501-8ece2954-6be6-4293-aa67-a9035063f167.png)



## 2. Android 系统适配常见问题

#### 2.1 两类行为变更

- **面对所有应用的行为变更：** 对于运行在该系统版本上的所有应用都会影响，而无论应用的 targetSDKVersion 为何。通常应该先针对这些变更进行适配和测试，这有助于用户在新版本系统上运行你的应用时，用户体验不会受损；

- **以特定 targetSDKVersion 为目标版本的行为表更：** 只有 targetSDKVersion 高于或等于系统版本的应用会影响，Google 通常会把影响较大或适配工作量较大的行为变更化为这一类，可以理解为一个适配缓冲区。

由于受 targetSDKVersion 控制的行为变更较多，你在适配新版系统时，可以利用下面提到的 “兼容性调试框架” 逐项启用，而不是修改 targetSDKVersion 一次性启用所有变更。

#### 2.2 兼容性调试框架

Android 11 引入了一个新的开发者调试工具，能够帮助开发者更加灵活可控地适配新版系统，在系统设置中搜索  可以找到这个功能。

在此之前，当我们需要适配以特定 Android 系统版本为目标版本的行为变更时，我们需要先做以下操作：

- 1、修改项目 targetSDKVersion，指向特定的 Android 系统版本；
- 2、适配所有行为变更，或者至少适配影响编译运行的行为变更；
- 3、重新编译构建应用，并安装到调试设备上。

这意味着你不仅需要重新编译应用，而且至少需要适配多个会影响编译运行的行为变更，这对适配问题比较多的应用就不太友好了。而使用应用兼容性调试框架，你可以在不升级 targetSDKVersion 的情况下，单独针对某一个行为变更进行适配。

兼容性调试框架支持通过开发者选项或 adb 命令启用或停用各项变更：

- **使用开发者选项启用或停用变更：** 先启用开发者选项后，在系统设置中搜索 `应用兼容性变更`，从列表中选择调试的应用，再从变更列表中找到各项变更。例如：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164006462-9ce60f4f-0b00-41ad-bd49-2f693681966c.png" width="400"/>
</p>

- **使用 adb 命令启用或停用变更：** 直接执行 adb 命令，模板如下：
  ```
  adb shell am compat enable (CHANGE_ID|CHANGE_NAME) PACKAGE_NAME
  adb shell am compat disable (CHANGE_ID|CHANGE_NAME) PACKAGE_NAME
  ```
  其中 PACKAGE_NAME 是应用包名，CHANGE_ID 或 CHANGE_NAME 是兼容性框架为每个变更定义的唯一标识，你可以在各个版本的官方文档中找到完整的变更列表：
  
  - [Android 11（API 级别 30）](https://developer.android.google.cn/about/versions/11/reference/compat-framework-changes)
  - [Android 12（API 级别 31 和 32）](https://developer.android.google.cn/about/versions/12/reference/compat-framework-changes)
  - [Android 13（开发者预览版）](https://developer.android.google.cn/about/versions/13/reference/compat-framework-changes)

---


| 平台版本 | API Level | Build.VERSON_CODES | 官方文档 |
| --- | --- | --- | --- |
| 13 | 10000（预览） | TIRAMISU（预览） | https://developer.android.google.cn/about/versions/13 <br> https://developer.android.com/about/versions/13 |
| 12L | 32 | S_V2 | https://developer.android.google.cn/about/versions/12/12L <br> https://developer.android.com/about/versions/12/12L |
| 12 | 31 | S | https://developer.android.google.cn/about/versions/12 <br> https://developer.android.com/about/versions/12 |
| 11 | 30 | R | https://developer.android.google.cn/about/versions/11 <br> https://developer.android.com/about/versions/11 |
| 10 | 29 | Q | https://developer.android.google.cn/about/versions/10 <br> https://developer.android.com/about/versions/10 |
| 9 | 28 | P | https://developer.android.google.cn/about/versions/pie <br> https://developer.android.com/about/versions/pie |
| 8.1 | 27 | O_MR1 | https://developer.android.google.cn/about/versions/oreo/android-8.1 <br> https://developer.android.com/about/versions/oreo/android-8.1 |
| 8.0 | 26 | O | https://developer.android.google.cn/about/versions/oreo <br> https://developer.android.com/about/versions/oreo |
| 7.1/7.1.1 | 25 | N_MR1 | https://developer.android.google.cn/about/versions/nougat/android-7.1 <br> https://developer.android.com/about/versions/nougat/android-7.1 |
| 7.0 | 24 | N | https://developer.android.google.cn/about/versions/nougat <br> https://developer.android.com/about/versions/nougat |
| 6.0 | 23 | M | https://developer.android.google.cn/about/versions/marshmallow <br> https://developer.android.com/about/versions/marshmallow |
| 5.1 | 22 | LOLLIPOP_MR1 | https://developer.android.google.cn/about/versions/android-5.1 <br> https://developer.android.com/about/versions/lollipop/android-5.1 |
| 5.0 | 21 | LOLLIPOP | https://developer.android.google.cn/about/versions/lollipop <br> https://developer.android.com/about/versions/lollipop |
| 4.4W | 20 | KITKAT_WATCH | KitKat for Wearables Only |
| 4.4 | 19 | KITKAT | https://developer.android.google.cn/about/versions/kitkat <br> https://developer.android.com/about/versions/kitkat |
| 4.3 | 18 | JELLY_BEAN_MR2 | / |
| 4.2/4.2.2 | 17 | JELLY_BEAN_MR1 | / |

## Android 13 进行时


<p align='center'>
<img src="https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2013/images/Android%2013%20%E8%AE%A1%E5%88%92%E6%97%B6%E9%97%B4%E8%BD%B4.svg"/>
</p>




# Donate

如果本仓库对你有帮助，可以请小彭喝杯速溶咖啡

<p align='center'>
<img src="https://github.com/pengxurui/Android-NoteBook/raw/master/images/微信收款码.jpeg" width = "200" />
</p>



#### License
Copyright [2022] [Peng Xurui]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
