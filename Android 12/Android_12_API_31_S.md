<p align='center'>
<img src="https://files.mdnice.com/user/3257/93431579-5c91-4c6f-9bdc-9923676f869e.png" width = "900" />
</p>

## 前言

Android 12 是 2021 年 10 月发布的最新正式版本，然而很多同学表示还没有适配。针对开发者在进行版本适配过程中遇到的问题，我们建立了 [GitHub · AndroidPlatformWiki](https://github.com/pengxurui/AndroidPlatformWiki "GitHub · AndroidPlatformWiki")。我们希望站在开发者的视角，全面且深刻地解读每个 Android 版本更新，以此建立起一个体系化的 Android 系统适配手册。具体包括：

### 两个维度

根据内容相关度，我们将从 2 个维度解读：

- **基于时间线：** 现阶段官方每年会发布一个新的版本，因此有必要以一个 Android 版本为单位，解读该版本涉及的新功能与行为变更。这样可以帮助开发同学了解新版本的更新内容，例如我们会通过一个文档解读 Android 13 版本的更新内容与适配自查表；
- **基于内容线：** 通常一个系统功能模块会历经多个系统版本更新才会趋于稳定，因此有必要以一个功能为单位，解读该功能的主要能力以及不同版本的变更和差异。这样可以帮助开发同学了解该功能在不同版本上的差异，例如我们会通过一个文档单独解读系统通知。

### 三个等级

根据故障敏感性分级，我们将系统变更的兼容性划分为 3 个等级：

- **强制适配 ❗：** 所有应用必须适配，否则会出现编译不通过、功能不可用或者用户体验受损等问题；
- **推荐适配 ⭐：** 不强制要求适配，但适配的应用将获得更出色的用户体验或更安全的隐私保护等收益；
- **已适配：** 应用不需要任何改动就已经兼容。

### 两类行为变更

系统行为变更通常属于以下两种类别之一：

- **面对所有应用的行为变更：** 运行在该系统版本上的所有应用都会影响，而无论应用的 targetSDKVersion 为何。通常应该先针对这些变更进行适配和测试，这有助于用户在新版本系统上运行你的应用时，用户体验不会受损；
- **以特定 targetSDKVersion 为目标版本的行为变更：** 只有 targetSDKVersion 高于或等于系统版本的应用会影响，通常是影响较大或适配工作量较大的变更，我们可以理解为一个 Google 留给开发者的适配缓冲。

---

## Android 12 适配自查表

根据故障敏感性分级，我们将 Android 12 系统变更的兼容性划分为 3 个等级：

- **强制适配❗：** 涉及该功能的所有应用必须适配的变更，不适配的应用会出现编译不通过、功能不可用，或者用户体验出现一定受损等问题；
- **推荐适配⭐：** 不强制要求适配的变更，适配的应用具有更出色的用户体验或更安全的隐私保护等；
- **已适配：** 应用不需要任何改动就可以兼容的变更。

### 以 Android 12 为目标版本的应用

| 类别              | 变更                          | 兼容性  | 摘要                                                                                 |
| :---------------- | :---------------------------- | :------ | :----------------------------------------------------------------------------------- |
| 1. 用户体验       | 自定义通知外观模板统一        | 强制 ❗ | 自定义通知的内容区域缩小为自定义通知模板内的一块区域，不再完整覆盖通知区域           |
|                   | 画中画 (PiP) 交互改进         | 推荐 ⭐ | 优化画中画 (PiP) 模式的用户交互                                                      |
|                   | Toast 视图改进                | 已适配  | 系统 Toast 视图文本最多可以显示两行，并且始终在文本旁边显示应用图标                  |
| 2. 安全和隐私设置 | 新蓝牙运行时权限（新）        | 推荐 ⭐ | 引入一些新运行时权限，用于更好地管理应用于附近蓝牙设备的连接，而无需请求位置信息权限 |
|                   | 传感器采样率限制              | 已适配  | 系统会限制某些移动传感器和位置传感器的数据的刷新率                                   |
|                   | 应用休眠改进                  | 已适配  | 扩展应用休眠机制                                                                     |
|                   | 数据访问审核中的归因标记改进  | 强制 ❗ | 归因标记必须在 Manifest 文件中声明                                                   |
|                   | ADB 备份限制                  | 已适配  | adb backup  导出的数据不再默认包含应用数据                                           |
|                   | 显式指定组件 exported 属性    | 强制 ❗ | 声明了 <intent-filter> 过滤器的组件必须显式设置 android:exported 属性                |
|                   | 显式指定 PendingIntent 可变性 | 强制 ❗ | PendingIntent 必须显式声明一个可变性标志                                             |
|                   | 检测不安全的嵌套 Intent 启动  | 已适配  | StrictMode 会检测不安全的嵌套 Intent 启动                                            |
| 3. 性能和电池     | 精确的闹钟权限（新）          | 强制 ❗ | 设置 AlarmManager 精准闹钟的应用必须在 Manifest 中声明权限                           |
|                   | 前台服务启动限制              | 强制 ❗ | 除了少数情况外，禁止应用从后台启动前台服务                                           |
|                   | 通知 trampoline 限制          | 强制 ❗ | 禁止从通知 trampoline 间接启动目标 Activity                                          |

### 所有应用

| 类别              | 变更                                | 兼容性  | 摘要                                                                       |
| :---------------- | :---------------------------------- | :------ | :------------------------------------------------------------------------- |
| 4. 用户体验       | Material You 设计语言（新）         | 已适配  | 新的设计语言                                                               |
|                   | 富媒体内容插入（新）                | 推荐 ⭐ | 应用可以从统一的位置接受任何来源（剪贴板粘贴、键盘输入或拖放操作）的内容   |
|                   | 支持 AVIF 图片（新）                | 推荐 ⭐ | 支持 AVIF 格式图片                                                         |
|                   | 应用启动动画 API SplashScreen（新） | 强制 ❗ | 支持定制应用启动转场动画                                                   |
|                   | Widget 桌面小部件改进               | 推荐 ⭐ | 改进 Widgets 外观和行为                                                    |
|                   | 图形 API 改进                       | 推荐 ⭐ | 新增图形效果                                                               |
|                   | OverScroll 过度滑动动画改进         | 已适配  | 过度滑动动画改为拉伸和反弹效果                                             |
|                   | 通知改进                            | 推荐 ⭐ | 增加新的通知样式和安全保障                                                 |
|                   | HTTP 深度链接解析改进               | 已适配  | 调整了 HTTP Intent 的默认解析行为                                          |
|                   | 全屏模式的手势导航改进              | 推荐 ⭐ | 增加了一次交互即可执行手势导航的模式                                       |
|                   | 屏幕尺寸 API 变更                   | 强制 ❗ | 针对适配每种设配上获取屏幕尺寸的需求，系统引入了新 API                     |
|                   | 多窗口模式标准化                    | 强制 ❗ | 在大屏设备中，系统会为所有 Activity 启用多窗口模式                         |
|                   | 延迟展示前台服务通知                | 已适配  | 除了特殊情况外，前台服务通知会延迟 10 s 显示                               |
|                   | activity 生命周期改进               | 已适配  | 修改根 Activity 的返回行为                                                 |
|                   | Surface 帧率切换改进                | 推荐 ⭐ | 引入强制切换帧率的 API                                                     |
| 5. 安全和隐私设置 | 隐私信息中心（新功能）              | 推荐 ⭐ | 隐私信息中心以一个时间轴的方式显示过去时间内所有应用对于敏感信息的访问情况 |
|                   | 支持只授予粗略位置权限（新）        | 强制 ❗ | 用户可以只授予应用模糊位置权限                                             |
|                   | 麦克风和摄像头切换开关（新）        | 已适配  | 用户可以通过全局切换开关停用整台设备上的摄像头或麦克风权限                 |
|                   | 麦克风和摄像头指示标示（新）        | 已适配  | 应用使用麦克风或相机时，状态栏会有图标标记。                               |
|                   | 剪贴板访问提示（新）                | 已适配  | 应用首次从另一个应用访问剪辑数据时，会弹出一个消息框消息                   |
|                   | 隐藏应用叠加窗口（新）              | 推荐 ⭐ | 应用的窗口可见时可以隐藏所有可见的系统级悬浮窗口                           |
|                   | 应用无法关闭系统对话框              | 强制 ❗ | 除了特殊情况外，禁止应用尝试关闭系统对话框                                 |
|                   | 屏蔽不信任的触摸事件                | 强制 ❗ | 屏蔽从不同应用的窗口传递的事件                                             |
| 6. 性能和电池     | 应用待机分区改进                    | 已适配  | 引入了一个新的受限待机分区                                                 |

---

第 1~3 节介绍的是以 Android 12 为目标版本的应用行为变更和新功能更新，我将这部分更新总结为 3 部分：

- 1、用户体验（以 Android 12 为目标版本）

- 2、安全和隐私设置（以 Android 12 为目标版本）

- 3、性能和电池（以 Android 12 为目标版本）

---

## 1. 用户体验（以 Android 12 为目标版本）

### 1.1 自定义通知外观模板统一

Android 系统通知可以分为两类样式：标准通知 + 自定义通知

- **标准通知：** 标准通知是指基于 `NotificationCompat.Builder#setContentTitle()` 等模板 API 构建通知，最终会按照系统预置的视图模板展示。例如：

![](https://user-images.githubusercontent.com/25008934/164408756-088f6cae-ff97-4ca8-b432-e6fdcaacba45.png)

- **自定义通知：** 自定义通知是指基于 `NotificationCompat.Builder#setCustomContentView()` 等 API 构建的通知，最终会按照开发者自定义的的布局展示，而不会按照标准通知模板展示。

从 Android 12 系统开始，系统规范了自定义通知的外观和行为，自定义通知的内容区域缩小为自定义通知模板内的一块区域，不再完整覆盖通知区域。因此，如果你的应用使用了自定义通知，则需要进行必要的测试和调整：

- **布局调整：** 由于内容区域缩小了，需要调整并测试通知布局；
- **设置展开式通知：** 由于所有通知都是可展开的，所以需要调用 `NotificationCompat.Builder#setCustomBigContentView()` 设置展开后布局，确保展开和收起状态一致。

下图是统一的自定义通知模板：

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164408784-2798192f-9765-4331-b4f3-bb19f1bd1c4e.png" width = "600" />
</p>

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164408813-0eb4cda3-c4a6-4fc0-9d69-db4c3f50dd00.png" width = "600" />
</p>

**可以看出，这次改动是 Google 希望自定义通知能够呈现相对一致的感观体验，以及减少不同设备上产生的兼容性问题。**

### 1.2 画中画 (PiP) 交互改进

画中画模式是 Android 8.0 中引入的一种多窗口模式，最常用于视频播放 Activity，能够实现在视频播放过程中打开其他应用，而不退出中断当前视频。目前主流的音视频 App 都支持画中画模式，你可以在系统设置中搜索 “画中画” 查看。**这次改动是 Google 对画中画模式的用户交互进行优化**，具体参考资料：

- [对画中画的支持](https://developer.android.com/guide/topics/ui/picture-in-picture.html "对画中画的支持") —— 官方文档
- [Android 12 画中画改进](https://developer.android.google.cn/about/versions/12/features/pip-improvements "Android 12 画中画改进") —— 官方文档

### 1.3 Toast 视图改进

在 Android 12 中，系统 Toast 视图文本最多可以显示两行，并且始终在文本旁边显示应用图标。相关资料：[消息框概览](https://developer.android.google.cn/guide/topics/ui/notifiers/toasts "消息框概览")

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164408849-392d9164-6427-4f3e-a219-d4546f396775.png" width = "300" />
</p>

---

## 2. 安全和隐私设置（以 Android 12 为目标版本）

### 2.1 新蓝牙运行时权限（新功能）

Android 12 系统引入了新的运行时权限  [BLUETOOTH_SCAN](https://developer.android.google.cn/reference/android/Manifest.permission#BLUETOOTH_SCAN "BLUETOOTH_SCAN")、[BLUETOOTH_ADVERTISE](https://developer.android.google.cn/reference/android/Manifest.permission#BLUETOOTH_ADVERTISE "BLUETOOTH_ADVERTISE")  和  [BLUETOOTH_CONNECT](https://developer.android.google.cn/reference/android/Manifest.permission#BLUETOOTH_CONNECT "BLUETOOTH_CONNECT") 权限，用于更好地管理应用于附近蓝牙设备的连接。

在低版本中，应用与附近蓝牙设备连接需要用户授予 `ACCESS_FINE_LOCATION` 精确位置权限，这其实是不合理的设计，因为用户很难理解为什么蓝牙连接会跟位置信息有关。从 Android 12 系统开始，ACCESS_FINE_LOCATION 精确位置权限是可选项，只要应用不会通过蓝牙推导物理位置信息，就不再需要请求。如果不会，你需要在 Manifest 中显式做出 `usesPermissionFlags` 声明：

```kotlin
<manifest>
    <!-- Include "neverForLocation" only if you can strongly assert that
         your app never derives physical location from Bluetooth scan results. -->
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN"
                     android:usesPermissionFlags="neverForLocation" />
    ...
</manifest>
```

- **新蓝牙权限体系（以 Android 12 为目标版本）：**
  - BLUETOOTH_SCAN：允许搜索附近蓝牙设备；
  - BLUETOOTH_ADVERTISE：允许当前设备暴露给其他蓝牙设备；
  - BLUETOOTH_CONNECT：允许当前设备连接其他蓝牙设备；
  - ACCESS_FINE_LOCATION（可选）：允许由蓝牙信息推导设备位置信息。
- **旧蓝牙权限体系：**
  - BLUETOOTH：允许与蓝牙相关的交互；
  - ACCESS_FINE_LOCATION（必选）：允许由蓝牙信息推导设备位置信息，在 Android 9 或以下版本，可以用 ACCESS_COARSE_LOCATION 替代。

另外，BLUETOOTH_SCAN  等权限是 NEARBY_DEVICES 附近设备权限组的一部分。请求该权限组的权限，权限授予对话框会提示用户批准访问附近的设备。

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164408902-ac7df413-bed9-4183-8f52-fe08b2c3e7b2.png" width = "300" />
</p>

**可以看出，这次的改动 Google 是希望连接蓝牙设备的权限授予能够给用户更精准的权限功能描述。**

相关资料：

- [蓝牙概览](https://developer.android.google.cn/guide/topics/connectivity/bluetooth "蓝牙概览") —— 官方文档
- [蓝牙权限](https://developer.android.google.cn/guide/topics/connectivity/bluetooth/permissions#declare-android11-or-lower "蓝牙权限") —— 官方文档

### 2.2 传感器采样率限制

大多数 Android 设备都有内置传感器，用来测量运动、屏幕方向和各种环境条件，这些传感器能够提供高度精确的原始数据。为了保护有关用户的潜在敏感信息，Android 12 系统会限制某些移动传感器和位置传感器的数据的刷新率。

相关资料：[传感器概览](https://developer.android.google.cn/guide/topics/sensors/sensors_overview#sensors-rate-limiting "传感器概览") —— 官方文档

### 2.3 应用休眠改进

Android 11 引入了应用休眠机制，如果用户有几个月没有与应用交互，那么系统会将应用置于休眠 / 冬眠状态，Android 12 扩展了应用休眠机制：

- Android 11：重置已授予的运行时敏感权限；
- Android 12：重置已授予的运行时敏感权限；无法从后台运行任务；无法接收推送通知；应用缓存文件会被删除。

相关资料：[应用休眠](https://developer.android.google.cn/topic/performance/app-hibernation "应用休眠") —— 官方文档

### 2.4 数据访问审核中的归因标记改进

Android 11 引入了数据访问审核 API，开发者可以在应用访问用户隐私数据的代码位置增加归因标记，并通过注册 `AppOpsManager.OnOpNotedCallback` 监听。这个功能提供了对调用隐私数据的监听，无论是应用层还是依赖库中的代码，只要访问到私密数据（危险权限）都会回调。从 Android 12 系统开始，归因标记必须在 Manifest 文件中声明，例如：

```kotlin
<manifest ...>
    <!-- The value of "android:tag" must be a literal string, and the
         value of "android:label" must be a resource. The value of
         "android:label" should be user-readable. -->
    <attribution android:tag="sharePhotos"
                 android:label="@string/share_photos_attribution_label" />
    ...
</manifest>
```

相关资料：[数据访问审核](https://developer.android.google.cn/guide/topics/data/audit-access#declare-attribution-tags "数据访问审核") —— 官方文档

### 2.5 ADB 备份限制

为了保护私有应用数据，Android 12 变更了  `adb backup`  命令的默认行为，adb backup  导出的数据不再默认包含应用数据。如果开发阶段需要依赖于  adb backup  导出的应用数据，可以将 Manifest 文件中将  android:debuggable  设置为  true  来导出应用数据。

### 2.6 显式指定组件 exported 属性

组件属性 `android:exported` 用于设置该组件是否支持其他应用交互，exported 为 false 表示不允许该组件被其他应用启动。一般 exported 属性默认为 false，除非组件声明了 `<intent-filter>` 过滤器（即支持隐式启动），则 exported 属性默认为 true。从 Android 12 系统开始，声明了 <intent-filter> 过滤器的组件必须显式设置 android:exported 属性。例如：

```kotlin
<service android:name="com.example.app.backgroundService"
         android:exported="false">
    <intent-filter>
        <action android:name="com.example.app.START_BACKGROUND" />
    </intent-filter>
</service>
```

否则，在编译应用时就会有报错：

```kotlin
Manifest merger failed : Apps targeting Android 12 and higher are required \
to specify an explicit value for android:exported when the corresponding \
component has an intent filter defined.
```

如果使用低版本的 Android Gradle 插件虽然可以编译成功，但安装时会报错：

```bash
Installation did not succeed.
The application could not be installed: INSTALL_FAILED_VERIFICATION_FAILURE
```

**可以看出，这次改动背后的理念是 “不要相信默认值”，因为不符合预期的默认值会产生更严重的风险**。举个例子，由于开发者的疏忽，一个原本不允许外部应用启动的组件未显式声明 android:exported=“false”，而正好该组件声明了 <intent-filter> 过滤器，那么就因为默认值的影响的产生了一个安全风险。而强制开发者对声明 <intent-filter> 过滤器的组件显式声明 android:exported 的值，就可以避免了默认值的安全风险。同样的道理在对接外部系统时，也不要相信默认值，例如网络请求参数的默认值，能传的就传。

### 2.7 显式指定 PendingIntent 可变性

为了使 PendingIntent 的处理更加安全，Android 12 要求 PendingIntent 必须显式声明一个可变性标志即  [FLAG_MUTABLE](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.android.google.cn%2Freference%2Fandroid%2Fapp%2FPendingIntent%23FLAG_MUTABLE "FLAG_MUTABLE") 或  [FLAG_IMMUTABLE](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.android.google.cn%2Freference%2Fandroid%2Fapp%2FPendingIntent%23FLAG_IMMUTABLE "FLAG_IMMUTABLE")。在此之前，PendingIntent 默认是可变的。

### 2.8 检测不安全的嵌套 Intent 启动

Android 12 引入了一项 `StrictMode` 检查规则，用于检测不安全的嵌套 Intent 启动。StrictMode 模式大家很熟悉了，这里解释下为什么嵌套 Intent 启动是不安全的。

举个例子，开发者的预期效果是 Client App 请求 Provider App 的一个服务，并且希望在请求结束后回调到 Client App 的 ClientCallbackActivity。那么，最直接的方法是将启动 ClientCallbackActivity 的 Intent 当作参数嵌套到启动 ApiService 的 Intent 里。例如：

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164408971-6b0d97fe-9ba0-46b2-8c54-3df498b4c5ab.png" width = "900" />
</p>

乍看起来没有问题，但其实这种实现方式存在两个隐蔽的安全风险：

- **Client App：** 由于 ClientCallbackActivity 是从另一个应用 Provider App 启动的，因此它必须暴露为 exported。这意味着除了 Provider App 外，设备上其他恶意的应用也可以启动 ClientCallbackActivity；
- **Provider App：** 由于嵌套的 Intent 是在 Provider App 的上下文中启动的，因此恶意应用 Attacker App 可以将 Provider App 的任何一个 Activity 嵌套其中，即使启动的是私有的非 exported 的 Activity，这让 Provider App 防不胜防。

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409005-78b6bf35-ea1f-4f1a-b269-480231405bba.png" width = "900" />
</p>

解决方法是使用 PendingIntent 替代嵌套 Intent，PendingIntent 是 Intent 的包装容器，也类似于一个嵌套 Intent。但是，**很多小伙伴简单地认为 PendingIntent 只是延迟待处理的 Intent，两者只有时间维度的区别，这是片面的。**

**PendingIntent 的最主要的作用是授权外部应用以本应用的身份执行使用嵌套的 Intent**。有点拗口哈，在我们这个例子里，就是 Client App 将启动 ClientCallbackActivity 的 Intent 暴露给 Provider App 后，但 Provider App 在使用 PendingIntent 时，系统会以 Client App 的上下文身份来使用嵌套的 Intent。

```kotlin
PendingIntent pendingIntent = PendingIntent.getActivity(application, 0, resultIntent, 0);
```

现在，我们再回顾下还有没有风险：

- **Client App：** 由于 PendingIntent 使用 Client App 的身份使用嵌套的 Intent，那么 ClientCallbackActivity 不再需要暴露为 exported；
- **Provider App：** 由于 PendingIntent 使用 Client App / Attacker App 的身份使用嵌套的 Intent，而它们是没有权限访问 Provider App 非 exported 的 ApiSensitiveActivity 的。

相关资料：[Android 嵌套 Intent](https://medium.com/androiddevelopers/android-nesting-intents-e472fafc1933 "Android 嵌套 Intent") —— 官方博客文章

---

## 3. 性能和电池（以 Android 12 为目标版本）

### 3.1 精确的闹钟权限（新功能）

Android 12 系统引入了新的权限 `android.permission.SCHEDULE_EXACT_ALARM`，设置 AlarmManager 精准闹钟的应用必须在 Manifest 中请求 SCHEDULE_EXACT_ALARM 权限。此外，还新增了一个新的 API —— `canScheduleExactAlarms()`，用于检查应用的精准闹钟权限状态。

相关资料：[设置重复闹钟时间](https://developer.android.google.cn/training/scheduling/alarms#exact-acceptable-use-cases "设置重复闹钟时间")

### 3.2 前台服务启动限制

Android 12 对应用从后台启动前台服务的行为做出限制，除了 [后台启动限制的豁免](https://developer.android.google.cn/guide/components/foreground-services#background-start-restriction-exemptions "后台启动限制的豁免") 等少数情况外，如果应用尝试在后台运行时启动前台服务，系统会抛出 `ForegroundServiceStartNotAllowedException` 异常。应用可以使用 JobScheduler 中新引入的  [加急作业](<https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.android.google.cn%2Freference%2Fandroid%2Fapp%2Fjob%2FJobParameters.html%23isExpeditedJob( "加急作业")>) (expedited job) 来代替之前的做法。

> **提示：** 如果一个应用调用 `Context.startForegroundService()` 以启动另一个应用拥有的前台服务，则这些限制仅适用于两个应用都针对 Android 12 或更高版本的情况。

### 3.3 通知 trampoline 限制

通知 trampoline (蹦床) 是指利用广播接收器或服务间接启动目标 Activity（用户与通知交互后，应用先启动服务或广播接收器作为中介，再去启动 目标 Activity）。Android 12 系统对通知 trampoline 做出限制，当应用尝试从通知 trampoline 启动 Activity，系统会拦截该启动行为：

```kotlin
Indirect notification activity start (trampoline) from PACKAGE_NAME, \
this should be avoided for performance reasons.
```

如果你的应用使用了通知 trampoline，那么你需要切换为常规的 PendingIntent 方式。

---

第 4~6 节介绍的是针对所有应用的应用行为变更和新功能更新，我将这部分更新总结为 3 部分：

- 4、用户体验（所有应用）
- 5、安全和隐私设置（所有应用）
- 6、性能和电池（所有应用）

---

## 4. 用户体验（所有应用）

### 4.1 Material You 设计语言（新功能）

Android 12 引入了一种名为  [Material You](https://material.io/blog/announcing-material-you "Material You") 的新设计语言（对 Material Design 的再发展），可帮助构建更具个性化、更精美的应用。

![](https://user-images.githubusercontent.com/25008934/164409057-9633b65a-2d4f-4de2-aeef-4002fb0d3e28.png)

### 4.2 富媒体内容插入（新功能）

Android 12 系统引入了一个统一 API，使得应用可以从统一的位置接受任何来源（剪贴板粘贴、键盘输入或拖放操作）的内容。例如：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409087-d377388b-ef11-4cb3-b2e7-7b1e52491638.gif" width = "300" />
</p>

相关资料：[接收富媒体内容](https://developer.android.google.cn/guide/topics/input/receive-rich-content "接收富媒体内容") —— 官方文档

### 4.3 支持 AVIF 图片（新功能）

Android 12 引入了对使用 AV1 图片文件格式 (AVIF) 的图片的支持。AVIF 是一种使用 AV1 编码的图片和图片序列的容器格式。AVIF 利用了视频压缩的帧内编码内容。与以前的图片格式（例如 JPEG）相比，这种格式可显著提升相同文件大小下的图片质量。

![](https://user-images.githubusercontent.com/25008934/164409134-8316b0ad-eaf6-4024-beba-5f14fbea7b93.png)

相关资料：[AVIF has landed](https://jakearchibald.com/2020/avif-has-landed/ "AVIF has landed") —— Jake Archibald 著

### 4.4 应用启动动画 API SplashScreen（新功能）

从 Android 12 系统开始，所有应用的冷启动和温启动期间，系统会使用新的 SplashScreen API 来启动应用启动动画。除了平台 API 外，Google 还提供了兼容库 API：[androidx.core.splashscreen](https://developer.android.google.cn/reference/kotlin/androidx/core/splashscreen/SplashScreen "androidx.core.splashscreen")。

![](https://user-images.githubusercontent.com/25008934/164409181-9145b7ac-20f5-4feb-8145-cd1ca3716758.png)

在 SplashScreen API 之前，我们通常是利用 SplashActivity 的背景图 `android:windowBackground` 来实现应用启动转场效果，这个大家都很熟悉了。如果你不做任何适配，那么根据你配置的 windowBackground 资源值，在 Android 12 上会有不同的效果：

- windowBackground 采用 `@color/单色`，则系统会使用该单色和应用的启动图标来构成启动效果，这可能与预期效果不符；
- windowBackground 采用 `@drawable/图片`，则系统会继续使用该图片来构成启动效果，这个体验与低版本系统一致。

因此，如果你的应用采用的是 windowBackground 为图片资源的方式，那么你不适配也没有问题。需要升级启动效果的话，推荐参考以下资料：

- [启动画面](https://developer.android.google.cn/guide/topics/ui/splash-screen "启动画面") —— Android 官方文档
- [Jetpack 新成员 SplashScreen：打造全新的 App 启动画面](https://juejin.cn/post/6997217571208445965 "Jetpack 新成员 SplashScreen：打造全新的 App 启动画面") —— TechMerger 著

**可以看出，这次改动 Google 是希望提升下应用启动时的转场体验，同时也给予开发者更多自定义的想象空间。**

### 4.5 Widget 桌面小部件改进

Android 12 改进了现有的 Widgets API，让它们更实用、更美观，且更易于发现。相关改动详见以下资料：

![Untitled 11](https://user-images.githubusercontent.com/25008934/164409226-fca85d92-0a76-4e7a-9a95-f00a914659d5.png)

相关资料：

- [应用 Widget 概览](https://developer.android.google.cn/guide/topics/appwidgets/overview "应用 Widget 概览") —— 官方文档
- [Android 12 Widget 改进](https://developer.android.google.cn/about/versions/12/features/widgets "Android 12 Widget 改进") —— 官方文档
- [创造无限可能 | 在 Android 12 中使用 widget](https://juejin.cn/post/7012815676641378317 "创造无限可能 | 在 Android 12 中使用 widget") —— 官方博客文章
- [更新您的 widget 以适配 Android 12](https://juejin.cn/post/7004660915538755615 "更新您的 widget 以适配 Android 12") —— 官方博客文章

### 4.6 图形 API 改进

- 圆角：Android 12 引入了新的圆角 API [RoundedCorner](https://developer.android.google.cn/reference/android/view/RoundedCorner "RoundedCorner")
  和  [WindowInsets.getRoundedCorner(int position)](<https://developer.android.google.cn/reference/android/view/WindowInsets#getRoundedCorner(int "WindowInsets.getRoundedCorner(int position "WindowInsets.getRoundedCorner(int position)")")>)，可以提供给 View 实现圆角效果；
- 滤镜：Android 12 添加了新的滤镜 API [RenderEffect](https://developer.android.google.cn/reference/android/graphics/RenderEffect "RenderEffect") 可以给 View 实现常见的图片效果（如毛玻璃、颜色滤镜、Android 着色器效果及更多效果）。

  ```kotlin
  view.setRenderEffect(RenderEffect.createBlurEffect(radiusX, radiusY, SHADER_TILE_MODE))
  ```

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409256-00b4371f-5d2f-4b78-8842-b3cc404361d1.png" width = "600" />
</p>

### 4.7 OverScroll 过度滑动动画改进

Android 12 修改了可滚动控件在边缘过度滑动（OverScroll）的动画效果，从低版本的边缘发光效果修改为拉伸和反弹效果。这个边缘过度滑动效果是可以关闭的，有两种方法：

- 设置 `android:overScrollMode=”never”`
- 设置 `View#setOverScrollMode(View.OVER_SCROLL_NEVER)`

### 4.8 通知改进

- 电话通知：从 Android 12 系统开始，新增了新的电话通知样式 [Notification.CallStyle](https://developer.android.google.cn/reference/android/app/Notification.CallStyle "Notification.CallStyle")；
- 丰富图片支持：从 Android 12 系统开始，应用可以在  [MessagingStyle()](<https://developer.android.google.cn/reference/android/app/Notification.MessagingStyle#MessagingStyle(android.app.Person "MessagingStyle( "MessagingStyle()")")>)  和  [BigPictureStyle()](<https://developer.android.google.cn/reference/android/app/Notification.BigPictureStyle#BigPictureStyle( "BigPictureStyle( "BigPictureStyle()")")>) 通知中提供动画图片，来丰富应用的通知体验。此外，应用现在还可以让用户在从通知栏回复消息时发送图片消息；
- 设备解锁保障：从 Android 12 系统开始，应用可以通过 [setAuthenticationRequired(true)](<https://developer.android.google.cn/reference/android/app/Notification.Action.Builder#setAuthenticationRequired(boolean "setAuthenticationRequired(true "setAuthenticationRequired(true)")")>)，要求系统在执行通知的 PendingIntent 前先要求用户解锁设备，这对于敏感操作增加了一层安全保障。

### 4.9 HTTP 深度链接解析改进

Android 系统支持通过 Deep Link 或 Android App Link 将深度链接与应用行为关联，实践中采用的链接基于 URI 格式，例如：

![Untitled 13](https://user-images.githubusercontent.com/25008934/164409282-7155ac2c-8bfd-4fec-b15d-14d701715f2e.png)

从 Android 12 系统开始，系统调整了 HTTP Intent 的默认解析行为。在低版本中，如果 HTTP 链接未命中任何 Deep Link / App Link 的匹配规则，那么系统会打开应用选择对话框；而现在系统会直接通过默认浏览器打开链接（因为该链接本身是一个可访问的网址）

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409322-12c48e38-1d67-4edb-93cf-70aa3da8ac6e.gif" width = "600" />
</p>

相关资料：

- [Android Deep Link 深度链接，看看你在第几层？](https://juejin.cn/post/7073731278612201509 "Android Deep Link 深度链接，看看你在第几层？")
- [处理 Android 应用链接](https://developer.android.google.cn/training/app-links "处理 Android 应用链接") —— 官方文档

### 4.10 全屏模式下的手势导航改进

全屏模式是指应用最大限度地利用屏幕空间来展示内容，让用户获得最佳体验，常见场景例如视频、游戏、演示文稿等。

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409462-6ce2e5c4-cf9c-484e-893d-31f5cc070774.png" width = "600" />
</p>

全屏模式会隐藏状态栏、导航栏等系统栏，意味着用户无法轻松与系统栏交互，因此系统定义了以下全屏模式下的系统栏行为，使用 [WindowInsetsControllerCompat.setSystemBarsBehavior](https://developer.android.google.cn/reference/androidx/core/view/WindowInsetsControllerCompatt "WindowInsetsControllerCompat.setSystemBarsBehavior") 设置：

- [BEHAVIOR_SHOW_BARS_BY_TOUCH](https://developer.android.google.cn/reference/androidx/core/view/WindowInsetsControllerCompat#BEHAVIOR_SHOW_BARS_BY_TOUCH "BEHAVIOR_SHOW_BARS_BY_TOUCH") 模式，当用户点击屏幕任何位置后，会重新显示系统栏。这种模式适合于用户不会与屏幕进行大量互动的场景；
- [BEHAVIOR_SHOW_BARS_BY_SWIPE](https://developer.android.google.cn/reference/androidx/core/view/WindowInsetsControllerCompat#BEHAVIOR_SHOW_BARS_BY_SWIPE "BEHAVIOR_SHOW_BARS_BY_SWIPE") 模式，当用户从隐藏系统栏的边缘滑动时，会显示系统栏。例如从屏幕底部边缘向上滑动，会重新显示系统导航栏。这种模式适合于用户需要与屏幕进行大量交互的场景，例如游戏、阅读等，使用这种意图更强的手势能够避免系统栏交互与应用交互冲突；
- [BEHAVIOR_SHOW_TRANSIENT_BARS_BY_SWIPE](https://developer.android.google.cn/reference/androidx/core/view/WindowInsetsControllerCompat#BEHAVIOR_SHOW_TRANSIENT_BARS_BY_SWIPE "BEHAVIOR_SHOW_TRANSIENT_BARS_BY_SWIPE") 模式，当用户从隐藏系统栏的边缘滑动时，会暂时性地显示系统栏，并等待一小段时间后自动重新隐藏。系统栏会并不会挤压应用内容，而是以半透明的方式覆盖在应用上层。

Android 12 系统整合了现有模式，BEHAVIOR_SHOW_BARS_BY_TOUCH 和 BEHAVIOR_SHOW_BARS_BY_SWIPE 这两种行为现已弃用，新增了 BEHAVIOR_DEFAULT 行为。

- [BEHAVIOR_DEFAULT](https://developer.android.google.cn/reference/android/view/WindowInsetsController#BEHAVIOR_DEFAULT "BEHAVIOR_DEFAULT") 模式：当用户从隐藏系统栏的边缘滑动时，会显示系统栏，这一点与 BEHAVIOR_SHOW_BARS_BY_SWIPE 类似。最主要的是，全面屏导航手势可以直接生效，不管系统导航栏是否可见。换句话说，BEHAVIOR_DEFAULT 行为让用户只需滑动一次即可执行手势导航，而在 Android 11 上则需要滑动两次。

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409369-6d379468-b023-4c31-bcbd-1c606e37f308.gif" width = "300" />
</p>

相关资料：[启用全屏模式](https://developer.android.google.cn/training/system-ui/immersive#immersive-consolidated-behavior "启用全屏模式")

### 4.11 屏幕尺寸 API 变更

Android 11 系统废弃了 [Display.getSize](https://developer.android.google.cn/reference/android/view/Display#getSize "Display.getSize") & [Display.getMetrics](https://developer.android.google.cn/reference/android/view/Display#getMetrics "Display.getMetrics")，并在 Android 12 中继续废弃了 [Display.getRealSize](https://developer.android.google.cn/reference/android/view/Display#getRealSize "Display.getRealSize") & [Display.getRealMetrics](https://developer.android.google.cn/reference/android/view/Display#getRealMetrics "Display.getRealMetrics")。Android 设备有许多不同的外形（例如大屏设备、平板电脑和可折叠手机），为了针对适配每种设备上获取屏幕尺寸的需求，系统引入了 WindowMetrics API。

- 平台 API： [WindowMetrics](https://developer.android.google.cn/reference/android/view/WindowMetrics "WindowMetrics")
- 兼容库 API： [WindowManager](https://developer.android.google.cn/jetpack/androidx/releases/window "WindowManager")

### 4.12 多窗口模式标准化

Android 7 系统引入了多窗口模式，允许同时在屏幕上显示多个应用，目前一共有 3 种多窗口模式：

- **分屏模式：** 以左右并排或上下并排显示两个应用；
- **画中画模式：** 以叠加的小窗口显示应用；
- **自由窗口模式：** 以可移动且可调整显示尺寸的窗口显示应用；

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409504-d73aebea-cb2a-44ee-b5a1-9e7d27907c38.png" width = "600" />
</p>

从 Android 12 系统开始，多窗口模式将成为大屏设备上的标准行为，大屏设备下 Activity 的 `resizeableActivity` 配置将被忽略。具体如下：

- **Android 7：** 手机设备支持分屏模式，电视设备支持画中画模式，更大尺寸的设备制造商可以选择启用自由窗口模式。开发者可以设置 android:resizeableActivity=”false” 禁用多窗口模式，确保 Activity 始终以独占屏幕的方式显示；
- **Android 8：** 手机设备也支持画中画模式；
- **Android 12：** 在小屏设备（sw < 600dp）设备中，系统根据 resizeableActivity 配置确定该 Activity 是否启用多窗口模式，在大屏设备中，系统会忽略 resizeableActivity 配置，为所有 Activity 启用多窗口模式。在此之前，用户要操作的 Activity 不支持多窗口的话，用户只能先退出窗口，再回来，体验就中断了。

**可以看出，这次改动 Google 是希望大屏设备下的多窗口模式成为标准行为，实现多窗口模式下的体验闭环**。

### 4.13 延迟展示前台服务通知

前台服务（startForegroundService 启动的服务）会显示一个系统通知，以便让用户应用正在执行任务并且消耗系统资源，即使该应用已经退出到后台。从 Android 12 系统开始，前台服务通知会延迟 10 s 显示，除非一些需要立即显示通知的服务。

以下是立即显示通知的情况：

- 1、前台服务通知调用 `NotificationCompat.Builder#setForegroundServiceBehavior(FOREGROUND_SERVICE_IMMEDIATE)` 修改了显示行为；
- 2、前台服务通知调用 `NotificationCompat.Builder#addAction()` 配置了可操作按钮；
- 3、前台服务通知调用 `NotificationCompat.Builder#setCategory()` 配置为 CATEGORY_CALL、CATEGORY_NAVIGATION 或 CATEGORY_TRANSPORT；
- 4、前台服务的 [foregroundServiceType](https://developer.android.google.cn/guide/topics/manifest/service-element#foregroundservicetype "foregroundServiceType") 取值 mediaPlayback、mediaProjection 或 phoneCall。

**可以看出，这次改动 Google 是希望简化短期运行的前台服务的用户感知，既然服务很快就停止了，就没有必要用通知让用户注意到。**

相关资料：

- [前台服务](https://developer.android.google.cn/guide/components/foreground-services "前台服务") —— 官方文档

### 4.14 activity 生命周期改进

从 Android 12 开始，系统修改了 Activity Task 根 Activity 在处理 ”返回键“ 时的默认行为。在旧版本中，返回键会执行 finish Activity，而从 Android 12 开始会将 Task 任务栈切换到后台。此后，用户返回应用将执行热启动，应用的热启动简单得多，系统的工作只是将 Activity 恢复到前台。

`Activity.java`

```kotlin
public void onBackPressed() {
    if (mActionBar != null && mActionBar.collapseActionView()) {
        return;
    }

    FragmentManager fragmentManager = mFragments.getFragmentManager();

    if (!fragmentManager.isStateSaved() && fragmentManager.popBackStackImmediate()) {
        return;
    }
    if (!isTaskRoot()) {
        // If the activity is not the root of the task, allow finish to proceed normally.
        finishAfterTransition();
        return;
    }
    // 以上 Android 12 与旧版本相同，差别在下面这句：
    try {
        // Android 11
        // Inform activity task manager that the activity received a back press
        // while at the root of the task. This call allows ActivityTaskManager
        // to intercept or defer finishing.
        ActivityTaskManager.getService().onBackPressedOnTaskRoot(mToken,
                new IRequestFinishCallback.Stub() {
                    public void requestFinish() {
                        mHandler.post(() -> finishAfterTransition());
                    }
                });
        // Android 12
        // Inform activity task manager that the activity received a back press while at the
        // root of the task. This call allows ActivityTaskManager to intercept or move the task
        // to the back.
        ActivityClient.getInstance().onBackPressedOnTaskRoot(mToken,
                new RequestFinishCallback(new WeakReference<>(this)));
    } catch (RemoteException e) {
        finishAfterTransition();
    }
}
```

**可以看出，这次的改动 Google 希望通过 ”返回**键**“ 退出的应用，能够更快地从热启动恢复应用，而不是从冷启动或温启动重启应用。**

### 4.15 Surface 帧率切换改进

Android 11 系统引入了一个 Surface 帧率切换 API `setFrameRate()`，但这个 API 并不总是会生效。由于不支持无缝切换帧率的屏幕在切换帧率时会有一两秒的黑屏，所以系统会对这一行为做拦截。如果屏幕不支持无缝切换，即使应用调用 setFrameRate() 依然会继续使用原来的帧率。

Android 12 系统引入了强制切换帧率的 API，这对于长视频内容的帧率切换更有优势，因为合适帧率带来的体验提升已经超过了不支持无缝切换带来的体验损失。

```kotlin
// 检查是否支持无缝切换帧率
val refreshRates = this.display?.mode?.alternativeRefreshRates
val willBeSeamless = Arrays.asList<FloatArray>(refreshRates).contains(newRefreshRate)

// 切换帧率，即使屏幕不支持无缝过渡
surface.setFrameRate(newRefreshRate, FRAME_RATE_COMPATIBILITY_FIXED_SOURCE, CHANGE_FRAME_RATE_ALWAYS)
```

- **CHANGE_FRAME_RATE_ALWAYS：** 总是切换帧率，即使屏幕不支持无缝过渡；
- **CHANGE_FRAME_RATE_ONLY_IF_SEAMLESS：** 仅在屏幕支持无缝过渡时切换帧率（旧版本行为）。

---

## 5. 安全和隐私设置（所有应用）

### 5.1 隐私信息中心（新功能）

Android 12 系统在系统设置中引入了隐私信息中心功能，可以让用户更好地了解应用正在访问数据的行为。隐私信息中心以一个时间轴的方式显示过去时间内所有应用对于麦克风、摄像头或位置等敏感信息的访问情况。

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409545-f5d0208b-681a-4f84-a79d-d014671e02b6.png" width = "300" />
</p>

另外，系统定义了一个新的 Intent 操作 [ACTION_VIEW_PERMISSION_USAGE_FOR_PERIOD](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.android.google.cn%2Freference%2Fandroid%2Fcontent%2FIntent.html%23ACTION_VIEW_PERMISSION_USAGE_FOR_PERIOD "ACTION_VIEW_PERMISSION_USAGE_FOR_PERIOD")，可以从隐私信息中心中向用户解释为什么应用需要访问这些隐私信息。要支持此功能，应用需要定义一个 Activity 并声明 IntentFilter 关联到此 Action 上，例如：

```kotlin
<!-- android:exported required if you target Android 12. -->
<activity android:name=".DataAccessRationaleActivity"
          android:permission="android.permission.START_VIEW_PERMISSION_USAGE"
          android:exported="true">
       <!-- VIEW_PERMISSION_USAGE shows a selectable information icon on
            your app permission's page in system settings.
            VIEW_PERMISSION_USAGE_FOR_PERIOD shows a selectable information
            icon on the Privacy Dashboard screen. -->
    <intent-filter>
       <action android:name="android.intent.action.VIEW_PERMISSION_USAGE" />
       <action android:name="android.intent.action.VIEW_PERMISSION_USAGE_FOR_PERIOD" />
       <category android:name="android.intent.category.DEFAULT" />
       ...
    </intent-filter>
</activity>
```

相关资料：[解释对比较敏感信息的访问权限](https://developer.android.google.cn/training/permissions/explaining-access#privacy-dashboard-show-rationale "解释对比较敏感信息的访问权限")

### 5.2 支持只授予粗略位置权限（新功能）

Android 系统支持两个精度级别的位置信息，并且分别对应一个权限。虽然有两个精度级别的权限，但是因为它们处于同一个权限组中，所以应用只要请求授予其中一个权限，另一个权限就自动授予了。

- **粗略位置：** 精确到 3 平方公里的位置值，请求 ACCESS_COARSE_LOCATION 权限可以获得；
- **精确位置：** 精确到 50 米以内的位置值，请求 ACCESS_FINE_LOCATION 权限可以获得。

然而，从 Android 12 系统开始，这一规则不再成立了。从 Android 12 系统开始，用户可以只授予应用模糊位置 ACCESS_COARSE_LOCATION 权限，即使应用请求的是精确位置 ACCESS_FINE_LOCATION 权限。

举个例子，我们通过以下代码请求 ACCESS_FINE_LOCATION 权限，在 Android 12 系统上的权限请求弹窗会给用户两个选项：`Precise 精确位置` 和 `Approximate 粗略位置`。如果用户选择授予粗略位置，那么最终应用获得的权限反而是 ACCESS_COARSE_LOCATION 权限，而不是一开始请求的 ACCESS_FINE_LOCATION 权限，并且应用也只能获取粗略位置信息。

```kotlin
val locationPermissionRequest = registerForActivityResult(
    ActivityResultContracts.RequestPermission()
) { isGrant ->

    Log.i("权限","isGrant: $isGrant")
    if (PERMISSION_GRANTED == ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION)) {
        Log.i("权限", "ACCESS_COARSE_LOCATION is Grant")
    }
    if (PERMISSION_GRANTED == ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)) {
        Log.i("权限", "ACCESS_FINE_LOCATION is Grant")
    }
}

findViewById<View>(R.id.tv).setOnClickListener {
    locationPermissionRequest.launch( Manifest.permission.ACCESS_FINE_LOCATION)
}
```

输出日志：

```kotlin
I/权限: isGrant: false
I/权限: ACCESS_COARSE_LOCATION is Grant
```

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409579-bcd8d889-6c45-4e18-be85-72693bf3f931.png" width = "300" />
</p>

那么从 Android 12 开始，你在请求位置权限时就要注意以下问题，稍不注意就出现兼容问题了：

- 请求位置权限时要同时请求 ACCESS_FINE_LOCATION 权限和 ACCESS_COARSE_LOCATION 权限，如果应用只请求 ACCESS_FINE_LOCATION 权限，系统会直接忽略该请求。如果应用以 Android 12 或更高版本为目标版本，系统会在 logcat 中提示错误：

  ```kotlin
  ACCESS_FINE_LOCATION must be requested with ACCESS_COARSE_LOCATION.
  ```

  > 提示：我在 Pixel 模拟器上实测并没有出现文档描述的 ”忽略请求“ 和 ”报错提示“，不过最好还是按照官方文档处理吧。

- 即使用户已经授予了精确位置权限，用户依然可以进入系统设置中直接修改到粗略位置权限，修改后系统会自动杀死进程。
- 为了更好地尊重用户隐私，尽量只请求 ACCESS_COARSE_LOCATION 权限，因为粗略位置信息已经能满足大多数应用场景。仅请求 ACCESS_COARSE_LOCATION 权限时，授权弹窗只有一个选项：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409595-20a3940b-0c51-4753-ad9b-af85e6a0fede.png" width = "300" />
</p>

- 如果你的应用场景确实需要请求 ACCESS_FINE_LOCATION 权限，那么你可以再次同时请求 ACCESS_FINE_LOCATION  和  ACCESS_COARSE_LOCATION 权限。由于之前用户已经授予过粗略位置权限，这次的系统弹窗会变成询问是否升级到精确位置权限：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409624-ecce6438-a1c4-4727-9f9c-365205df3177.png" width = "300" />
</p>

- 最后一个问题，怎么确定应用场景时需要精确位置还是粗略位置呢？其实并不是依靠纯主观判断，这块是有行业标准的。在我们之前讨论的 [还在见招拆招？先看懂 APP 个人信息保护治理机制](https://juejin.cn/post/7078940840009302052 "还在见招拆招？先看懂 APP 个人信息保护治理机制") 中，提到过一个标准 [团体标准《APP 收集使用个人信息最小必要评估规范》共 17 项](https://link.juejin.cn/?target=https%3A%2F%2Fwww.miit.gov.cn%2Fjgsj%2Fxgj%2FAPPqhyhqyzxzzxd%2Fzcbz%2Fart%2F2021%2Fart_f438a606b92e40ada9c611139f297300.html "团体标准《APP 收集使用个人信息最小必要评估规范》共 17 项")。其中对于位置信息的获取有明确的标准和案例，例如：

<p align='center'>
<img src="https://user-images.githubusercontent.com/25008934/164409647-251f2b4b-8a0a-43dd-9ec8-ff07f55a2bd7.png" width = "900" />
</p>

> 有小伙伴发现部分 Android 12 系统的手机上，请求位置权限的行为跟低版本没有区别，这是因为有些厂商系统还没有完全实现这个功能。以小米 MIUI 13.0.5.0 为例，模糊定位功能需要打开 `系统设置 - 隐私保护实验室 - 模糊定位` 选项才能启用。而且我在该系统上实测后，发现即使用户只授予 ACCESS_COARSE_LOCATION 权限，另一个 ACCESS_FINE_LOCATION 权限也会同时授予，这个就离谱了，怪不得还在实验室。

**可以看出，这次的改动是 Google 希望引导开发者尽量使用低精度的位置信息，提高对用户隐私的保护度。**

相关资料：

- [请求位置权限](https://developer.android.google.cn/training/location/permissions "请求位置权限") —— 官方文档
- [请求应用权限](https://developer.android.google.cn/training/permissions/requesting#explain "请求应用权限") —— 官方文档
- [解释对比较敏感信息的访问权限](https://developer.android.google.cn/training/permissions/explaining-access#privacy-dashboard-show-rationale "解释对比较敏感信息的访问权限") —— 官方文档

### 5.3 麦克风和摄像头切换开关（新功能）

从 Android 12 系统开始，用户可以通过一个全局切换开关，停用整台设备上的摄像头或麦克风权限。当应用请求摄像头或麦克风权限时，系统有弹窗提示。这个切换开关在不同厂商系统上的体现不一样，比如小米系统是放在 `手机关机-隐身模式` 。

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164409680-7ef38e6a-9245-4873-b988-c1c6e58fa01c.png" width = "300" />
</p>

### 5.4 麦克风和摄像头指示标示（新功能）

从 Android 12 开始，当应用使用麦克风或相机时，在状态栏会有图标标记。

### 5.5 剪贴板访问提示（新功能）

在 Android 12 及更高版本中，当某个应用首次调用  [getPrimaryClip](https://developer.android.google.cn/reference/android/content/ClipboardManager#getPrimaryClip "getPrimaryClip")  以 [从另一个应用访问剪辑数据](https://developer.android.google.cn/guide/topics/text/copy-paste#Pasting "getPrimaryClip( "从另一个应用访问剪辑数据")") 时，会弹出一个消息框消息，提示用户应用存在访问剪贴板的行为。

### 5.6 隐藏应用叠加窗口（新功能）

Android 12 系统引入了隐藏 TYPE_APPLICATION_OVERLAY 窗口的功能。声明  [HIDE_OVERLAY_WINDOWS](https://developer.android.google.cn/reference/android/Manifest.permission#HIDE_OVERLAY_WINDOWS "HIDE_OVERLAY_WINDOWS")  权限后，应用可以调用  [setHideOverlayWindows](https://developer.android.google.cn/reference/android/view/Window "setHideOverlayWindows")  指明当应用的窗口可见时隐藏所有可见的 TYPE_APPLICATION_OVERLAY 窗口。例如在显示敏感页面（如交易）时，应用可以选择隐藏其他悬浮窗。

### 5.7 应用无法关闭系统对话框

为了加强用户与应用和系统互动时的控制，从 Android 12 系统开始，弃用了  [ACTION_CLOSE_SYSTEM_DIALOGS](https://developer.android.google.cn/reference/android/content/Intent#ACTION_CLOSE_SYSTEM_DIALOGS "ACTION_CLOSE_SYSTEM_DIALOGS") Intent 操作，当应用使用尝试关闭系统对话框时，除了 [一些特殊情况](https://developer.android.google.cn/about/versions/12/behavior-changes-all#close-system-dialogs-exceptions "一些特殊情况") 之外，系统会进行拦截：

- 以 Android 12 或更高版本为目标版本：系统会抛出 SecurityException；
- 以 Android 11 或更低版本为目标版本：系统不会执行 Intent，并在 logcat 提示：

  ```kotlin
  E ActivityTaskManager Permission Denial: \
  android.intent.action.CLOSE_SYSTEM_DIALOGS broadcast from \
  com.package.name requires android.permission.BROADCAST_CLOSE_SYSTEM_DIALOGS, \
  dropping broadcast.
  ```

### 5.8 屏蔽不信任的触摸事件

屏幕应用是用户与应用交互的主要方式，为了提高触摸交互的直观和安全性，Android 12 系统会屏蔽从不同应用的窗口传递的事件。因此，这次的改动主要影响 声明选择让触摸事件穿透窗口的应用，例如声明了 TYPE_APPLICATION_OVERLAY 和 FLAG_NOT_TOUCHABLE 的悬浮窗口。

详细分析见相关资料：[行为变更 | Android 12 中不受信任的触摸事件](https://juejin.cn/post/6998339177024602125 "行为变更 | Android 12 中不受信任的触摸事件") —— 官方博客文章

---

## 6. 性能和电池（所有应用）

### 6.1 应用待机分区改进

App Standby Buckets 应用待机分区是 Android 9 引入的电池管理功能，系统会对应用的使用新近度和使用频率对应用进行排序，分别放置在不同的分区中。更活跃的应用会被分配到更高优先级的分区中，而低优先级的分区中应用的作业、闹钟或 FCM 会有一定限制。Android 12 系统引入了一个新的分区 —— “受限” 待机分区：

- 活跃：目前正在使用，或者最近刚刚使用；
- 工作集：定期使用；
- 常用：经常使用，但不会每天使用；
- 极少使用：不经常使用；
- 受限：应用消耗大量资源，或表现出不良行为
- 从未使用：已安装但从未运行过。

旧版本的待机分区只是根据应用的活跃度排序，而现在还有引入资源占用率的维度。**可以看出，这次改动是 Google 希望提高对消耗大量系统资源的应用的限制。**

> 提示： 这些限制仅适用于设备使用电池供电的情况；在设备充电期间，系统不会对应用施加这些限制。

相关资料：[应用待机分区](https://developer.android.google.cn/topic/performance/appstandby#restricted-bucket "应用待机分区") —— 官方文档

---

## 7. 总结

关注我，带你了解更多，我们下次见。

**以下变更相对冷门，实用价值较低，暂且按住不表：**

- 行为变更 - Target 12 - 用户体验 - 大屏设备上的相机预览改进 & 富感反馈体验 & AppSearch & 游戏模式 & 近期网址共享（仅限 Pixel）
- 行为变更：Target 12 - WebView 中的现代 SameSite Cookie & 备份和恢复 & 连接性 & 供应商 & 更新后的非 SDK 接口限制
- 行为变更：所有应用 - 安全和隐私设置 - 权限软件包可见性 & 移除了 BouncyCastle 实现
- 行为变更：所有应用 - 安全和隐私设置 -
- 行为变更：所有应用 - 媒体 & 相机 & 图形和图片 & 连接性 & 更新后的非 SDK 接口限制

## 参考资料

- [新版本系统适配: Android 12 中的兼容性变更](https://mp.weixin.qq.com/s/yk3FCuaU9LtmdM1uOQjOMQ) —— 官方文档
- [Android 12 正式发布 | 开发者们的全新舞台](https://juejin.cn/post/7017745147345534983 "Android 12 正式发布 | 开发者们的全新舞台") —— 官方文档
- [行为变更：所有应用](https://developer.android.google.cn/about/versions/12/behavior-changes-all#immersive-mode-improvements "行为变更：所有应用") —— 官方文档
- [行为变更：以 Android 12 为目标平台的应用](https://developer.android.google.cn/about/versions/12/behavior-changes-12 "行为变更：以 Android 12 为目标平台的应用") —— 官方文档
- [功能和 API 概览](https://developer.android.google.cn/about/versions/12/features "功能和 API 概览") —— 官方文档

> 你的点赞对我意义重大！微信搜索公众号 [彭旭锐]，希望大家可以一起讨论技术，找到志同道合的朋友，我们下次见！

