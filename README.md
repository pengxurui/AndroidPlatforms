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

## 使用方法

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
## 1. Android 系统适配的现状

从 2008 年 9 月 23 日，Android 的第一个版本 Android 1.0 发布。到 2021 年 10 月 4 日，Android 12 正式版发布。不知不觉，Android 操作系统已经走过了十几个年头了，系统的发布周期也从最初的几年缩短到稳定的一年。

很多同学表示很难跟上系统的更新步伐：“我还没用上 Android 10，Android 12 已经更新了！”。对于版本适配也是抱着能拖就拖的态度。为什么会出现这个状况呢，我想原因是多方面的：

- **1、工作指标：** 目标对人生和团队都具有巨大的导向作用，大多数情况下，新版本适配并不被认为是重要紧急的工作目标。就算是拖到不能再拖了，有些同学也只是把工作标准定为 “测试没发现问题”，草草了事；

- **2、系统变更碎片化：** 新版本发布时，虽然官方的 Release 文档会列举出这个版本相关的新功能和行为变更，但是它不会告诉你哪些变更是兼容的，哪些变更又要求强制适配。有些频繁更新的功能模块甚至需要关联多个文档才能理解清楚；

- **3、官方文档不易理解：** 首先官方文档在翻译时信息传递就有损失，再加上官方文档大多是陈述行为变更本身，对变更背后的原理、安全考量或设计理念少有涉及，导致开发者看得一头雾水。这也是我们倾向于通过博客文章获取知识的主要原因；

- **4、浮躁焦虑的心态：** 浮躁和焦虑似乎成了一个普遍的现象，开发者宁愿把时间投入到音视频、Flutter、Framework 这些听起来更牛逼的事情上。新系统年年都有，这个性价比不高。

这样看来，新版本适配的前景看起来不明朗了。但是，对于想成为一名高级的 Android 开发工程师 / 移动开发工程师的你，我希望你明白这也是个机会：

- **1、行业愈发成熟规范：** 如今的移动开发正处在挤泡沫的阶段，初级的招聘量在减少，但中高级的招聘量在增加，这说明整个行业在区域成熟规范。只要你具备足够的能力，你的薪资也会水涨船高；

- **2、全面能力：** 移动开发已经不再是当年单兵作战的时代，只 “做好” 产品需求已经远远不够了。你不仅需要掌握 Android 本身的知识，还需要全链路的知识扩展，比如产品思维、设计规范、市场运营、数据分析、后端等。如果你已经不是刚工作的小白，但是你却很少了解到这些知识，建议你思考下自己是不是处于舒适区。在这个立场下，系统兼容性就是你全面能力中的一环，你的功能在不同系统下是否兼容，你的技术方案有没有考虑过不同系统的差异，你对 Android 系统有多了解，这些都是你可以体现差异化价值的地方（当然你钻研于技术层面的某一个点，把这个点做到极致也是市场的）；

- **3、别人在焦虑，你在行动：** 焦虑不能解决任何问题，与其把时间花在抱怨上，不如把时间花在解决问题上。

---

## 2. 我们在做的事

我们希望站在开发者的视角，全面且深刻地解读每个 Android 版本更新，以此建立起一个体系化的 Android 系统适配手册。具体包括：

### 2.1 两个维度

根据内容相关度，我们将从 2 个维度解读：

- **基于时间线：** 现阶段官方每年会发布一个新的版本，因此有必要以一个 Android 版本为单位，解读该版本涉及的新功能与行为变更。这样可以帮助开发同学了解新版本的更新内容，例如我们会通过一个文档解读 Android 13 版本的更新内容与适配自查表；

- **基于内容线：** 通常一个系统功能模块会历经多个系统版本更新才会趋于稳定，因此有必要以一个功能为单位，解读该功能的主要能力以及不同版本的变更和差异。这样可以帮助开发同学了解该功能在不同版本上的差异，例如我们会通过一个文档单独解读系统通知。

### 2.2 三个等级

根据故障敏感性分级，我们将系统变更的兼容性划分为 3 个等级：

- **强制适配❗：** 所有应用必须适配，否则会出现编译不通过、功能不可用或者用户体验受损等问题；

- **推荐适配⭐：** 不强制要求适配，但适配的应用将获得更出色的用户体验或更安全的隐私保护等收益；

- **已适配：** 应用不需要任何改动就已经兼容。

### 2.3 两类行为变更

系统行为变更通常属于以下两种类别之一：

- **面对所有应用的行为变更：** 运行在该系统版本上的所有应用都会影响，而无论应用的 targetSDKVersion 为何。通常应该先针对这些变更进行适配和测试，这有助于用户在新版本系统上运行你的应用时，用户体验不会受损；

- **以特定 targetSDKVersion 为目标版本的行为变更：** 只有 targetSDKVersion 高于或等于系统版本的应用会影响，通常是影响较大或适配工作量较大的变更，我们可以理解为一个 Google 留给开发者的适配缓冲。由于受 targetSDKVersion 控制的行为变更较多，你在适配新版系统时，可以利用下面提到的 “兼容性调试框架” 逐项启用，而不是修改 targetSDKVersion 一次性启用所有变更。

---

## 3. 系统适配手册

| 平台版本 | API Level | 官方文档 | 适配手册 |
| --- | --- | --- | --- |
| Android 13 | TIRAMISU | https://developer.android.google.cn/about/versions/13 <br> https://developer.android.com/about/versions/13 | [Android_13_DP2](https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2013/Android_13_DP2.md)|
| Android 12L | S_V2（32） | https://developer.android.google.cn/about/versions/12/12L <br> https://developer.android.com/about/versions/12/12L | / |
| Android 12 | S（31） | https://developer.android.google.cn/about/versions/12 <br> https://developer.android.com/about/versions/12 | [Android_12_API_31_S](https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2012/Android_12_API_31_S.md)|
| Android 11 | R（30） | https://developer.android.google.cn/about/versions/11 <br> https://developer.android.com/about/versions/11 | |
| Android 10 | Q（29） | https://developer.android.google.cn/about/versions/10 <br> https://developer.android.com/about/versions/10 | |
| Android 9 | P（28） | https://developer.android.google.cn/about/versions/pie <br> https://developer.android.com/about/versions/pie | |
| Android 8.1 | O_MR1（27） | https://developer.android.google.cn/about/versions/oreo/android-8.1 <br> https://developer.android.com/about/versions/oreo/android-8.1 | |
| Android 8.0 | O（26） | https://developer.android.google.cn/about/versions/oreo <br> https://developer.android.com/about/versions/oreo | |
| Android 7.1/7.1.1 | N_MR1（25） | https://developer.android.google.cn/about/versions/nougat/android-7.1 <br> https://developer.android.com/about/versions/nougat/android-7.1 | |
| Android 7.0 | N（24） | https://developer.android.google.cn/about/versions/nougat <br> https://developer.android.com/about/versions/nougat | |
| Android 6.0 | M（23） | https://developer.android.google.cn/about/versions/marshmallow <br> https://developer.android.com/about/versions/marshmallow | |
| Android 5.1 | LOLLIPOP_MR1（22） | https://developer.android.google.cn/about/versions/android-5.1 <br> https://developer.android.com/about/versions/lollipop/android-5.1 | |
| Android 5.0 | LOLLIPOP（21） | https://developer.android.google.cn/about/versions/lollipop <br> https://developer.android.com/about/versions/lollipop | |
| Android 4.4W | KITKAT_WATCH（20） | KitKat for Wearables Only | |
| Android 4.4 | KITKAT（19） | https://developer.android.google.cn/about/versions/kitkat <br> https://developer.android.com/about/versions/kitkat | |
| Android 4.3 | JELLY_BEAN_MR2（18） | / | |
| Android 4.2/4.2.2 | JELLY_BEAN_MR1（17） | / | |

---

## 4. 最新 Android 版本

### Android 13 开发者预览版 2

![Android13封面](https://user-images.githubusercontent.com/25008934/164061894-ecd36374-e783-45be-b504-9540e234b9a8.png)

Android 13 开发者预览版从 2022 年 2 月正式启动，3 月份 Google 已经发布了第 2 个开发者预览版。目前更新的内容主要还是围绕隐私和安全这个主题，我们会持续跟进官方的 [发布计划表](https://developer.android.google.cn/about/versions/13/overview)，最终版本预计在今年年底发布。

<p align='center'>
<img src="https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2013/images/Android%2013%20%E8%AE%A1%E5%88%92%E6%97%B6%E9%97%B4%E8%BD%B4.svg"/>
</p>

### 以 Android 13 为目标版本的应用

| 类别 | 变更 | 兼容性 | 摘要 |
| --- | --- | :--- | --- |
| 1. 用户体验 | 等待官方更新...... | / | / |
| 2. 安全和隐私设置 | 附近 Wi-Fi 设备运行时权限（新） | 推荐⭐ | 引入了新运行时权限，可使应用扫描附近的 Wi-Fi <br/>感知设备，而无需请求位置信息权限 |
|  | 后台访问身体传感器运行时权限（新） | 强制❗ | 引入了新的运行时权限，<br>用于更好地管理应用在后台时访问身体传感器的行为 |
|  | IntentFilter 会屏蔽不匹配的 Intent | 已适配 | 当该 Intent 与接收应用中的 <intent-filter> <br/>匹配时，系统才会传送该 Intent |
|  | 更安全地动态注册广播接收器 | 强制❗ | 应用必须明确指出动态注册的广播接收器<br>是否接收其他应用的广播 |
| 3. 性能和电池 | 等待更新... | / | / |

### 所有应用

| 类别 | 变更 | 兼容性 | 描述 |
| --- | --- | :--- | --- |
| 4. 用户体验 | 多语言支持改进（新） | 推荐⭐ | 引入了一系列新的语言特性优化，<br>用于改善多语言用户体验 |
|  | 自适应主题的应用图标（新） | 推荐⭐ | 应用图标颜色可以自适应 Launcher 主题色调调整配色。 |
| 5. 安全和隐私设置 | 通知运行时权限（新） | 强制❗ | 引入了新的运行时权限，<br>用于管理应用发送系统通知的能力 |
|  | 可降级权限（新） | 推荐⭐ | 应用可以主动撤销用户已授予的运行时权限 |
|  | 照片选择器（新） | 推荐⭐ | 用户可以只向应用提供特定选择的图片或视频，<br>而不是直接授予整个媒体库的访问权限 |
| 6. 性能和电池 | 前台服务 FGS 管理器（新） | 已适配 | 引入了前台服务 FGS 管理器功能，<br>可以直接关闭服务和应用 |
|  |  JobScheduler 预提取作业优化 | 已适配 | 系统会更智能地基于机器学习预测应用下次启动<br/>的时间，并根据该估算值执行预提取作业 |
|  | 省电措施改进 | 已适配 | 引入了新的电池省电措施，<br>以便为系统提供更多方法来管理电池续航时间 |
  
完整文档：[Android_13_API_33_T](https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2013/Android_13_API_33_T.md)

---

## 5. 兼容性调试框架

Android 11 引入了一个新的开发者调试工具，能够帮助开发者更加灵活可控地适配新版系统。在此之前，当我们需要适配以特定 Android 系统版本为目标版本的行为变更时，我们需要先做以下操作：

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

相关文档：[兼容性框架工具](https://developer.android.google.cn/guide/app-compatibility/test-debug)

---

# Donate

如果本仓库对你有帮助，可以请小彭喝杯速溶咖啡

<p align='center'>
<img src="https://github.com/pengxurui/Android-NoteBook/raw/master/images/微信收款码.jpeg" width = "200" />
</p>

---

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
