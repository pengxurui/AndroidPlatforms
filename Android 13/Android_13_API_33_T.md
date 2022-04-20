
![Untitled](https://user-images.githubusercontent.com/25008934/164178589-29ffff69-9325-40ab-8a3e-40a59b27071d.png)

Android 13 开发者预览版从 2022 年 2 月正式启动，3 月份 Google 已经发布了第 2 个开发者预览版。目前更新的内容主要还是围绕隐私和安全这个主题，我们会持续跟进官方的 [发布计划表](https://developer.android.google.cn/about/versions/13/overview)，最终版本预计在今年年底发布。

<p align='center'>
<img src="https://github.com/pengxurui/AndroidPlatformWiki/blob/main/Android%2013/images/Android%2013%20%E8%AE%A1%E5%88%92%E6%97%B6%E9%97%B4%E8%BD%B4.svg"/>
</p>

## Android 13 适配自查表（持续更新）

根据故障敏感性分级，我们将系统变更的兼容性划分为 3 个等级：

- **强制适配❗：** 所有应用必须适配，否则会出现编译不通过、功能不可用或者用户体验受损等问题；
- **推荐适配⭐：** 不强制要求适配，但适配的应用将获得更出色的用户体验或更安全的隐私保护等收益；
- **已适配：** 应用不需要任何改动就已经兼容。

### **以 Android 13 为目标版本的应用**

| 类别 | 变更 | 兼容性 | 摘要 |
| --- | --- | :--- | --- |
| 1. 用户体验 | 等待官方更新...... | / | / |
| 2. 安全和隐私设置 | 附近 Wi-Fi 设备运行时权限（新） | 推荐⭐ | 引入了新运行时权限，可使应用扫描附近的 Wi-Fi <br/>感知设备，而无需请求位置信息权限 |
|  | 后台访问身体传感器运行时权限（新） | 强制❗ | 引入了新的运行时权限，<br>用于更好地管理应用在后台时访问身体传感器的行为 |
|  | IntentFilter 会屏蔽不匹配的 Intent | 已适配 | 当该 Intent 与接收应用中的 <intent-filter> <br/>匹配时，系统才会传送该 Intent |
|  | 更安全地动态注册广播接收器 | 强制❗ | 应用必须明确指出动态注册的广播接收器<br>是否接收其他应用的广播 |
| 3. 性能和电池 | 等待更新... | / | / |

### **所有应用**

| 类别 | 变更 | 兼容性 | 摘要 |
| --- | --- | :--- | --- |
| 4. 用户体验 | 多语言支持改进（新） | 推荐⭐ | 引入了一系列新的语言特性优化，<br>用于改善多语言用户体验 |
|  | 自适应主题的应用图标（新） | 推荐⭐ | 应用图标颜色可以自适应 Launcher 主题色调调整配色。 |
| 5. 安全和隐私设置 | 通知运行时权限（新） | 强制❗ | 引入了新的运行时权限，<br>用于管理应用发送系统通知的能力 |
|  | 可降级权限（新） | 推荐⭐ | 应用可以主动撤销用户已授予的运行时权限 |
|  | 照片选择器（新） | 推荐⭐ | 用户可以只向应用提供特定选择的图片或视频，<br>而不是直接授予整个媒体库的访问权限 |
| 6. 性能和电池 | 前台服务 FGS 管理器（新） | 已适配 | 引入了前台服务 FGS 管理器功能，<br>可以直接关闭服务和应用 |
|  |  JobScheduler 预提取作业优化 | 已适配 | 系统会更智能地基于机器学习预测应用下次启动<br/>的时间，并根据该估算值执行预提取作业 |
|  | 省电措施改进 | 已适配 | 引入了新的电池省电措施，<br>以便为系统提供更多方法来管理电池续航时间 |

---

第 1~3 节介绍的是以 Android 13 为目标版本的应用行为变更和新功能更新，我将这部分更新总结为 3 部分：

- 1.用户体验（以 Android 13 为目标版本）
- 2.安全和隐私设置（以 Android 13 为目标版本）
- 3.性能和电池（以 Android 13 为目标版本）

## 1. 用户体验（以 Android 13 为目标版本）

等待官方更新......

## 2. 安全和隐私设置（以 Android 13 为目标版本）

### 2.1 附近 Wi-Fi 设备运行时权限（新功能）

Android 13 系统引入了新的运行时权限 `android.permission.NEARBY_WIFI_DEVICES` 附近 Wi-Fi 设备权限，用于管理应用与附近 Wi-Fi 感知设备的连接。

```kotlin
<manifest ...>
    <uses-permission android:name="android.permission.NEARBY_WIFI_DEVICES"/>
    <application ...>
        ...
    </application>
</manifest>
```

在低版本中，应用与附近 Wi-Fi 设备连接需要用户授予 ACCESS_FINE_LOCATION 精确位置权限，这其实是不合理的设计，因为用户很难理解为什么 Wi-Fi 连接会跟位置信息有关。从 Android 13 系统开始，ACCESS_FINE_LOCATION 精确位置权限是可选项，只要应用不会通过 Wi-Fi 推导物理位置信息，就不需要再请求。如果不会，你需要在 Manifest 中显式做出 `usesPermissionFlags` 声明（这与声明蓝牙设备信息不会用于获取位置信息类似）：

```kotlin
<manifest ...>
    <uses-permission android:name="android.permission.NEARBY_WIFI_DEVICES"
                     android:usesPermissionFlags="neverForLocation" />
    <application ...>
        ...
    </application>
</manifest>
```

另外，NEARBY_WIFI_DEVICES 权限是 NEARBY_DEVICES 附近设备权限组的一部分。此权限组在 Android 12 中引入，还包含与蓝牙相关的权限。请求该权限组的权限，权限授予对话框会提示用户批准访问附近的设备。**可以看出，这次的改动 Google 是希望连接 Wi-Fi 设备的权限授予能够给用户更精准的权限功能描述。**

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164178724-1a5e842e-7925-452c-b5ec-48b4d0c94ac1.png" width = "300" />
</p>

相关资料：[附近的 Wi-Fi 设备权限](https://developer.android.google.cn/about/versions/13/features/nearby-wifi-devices-permission)

### 2.2 后台访问身体传感器运行时权限（新功能）

Android 13 系统引入了新的运行时权限 `android.permission.BODY_SENSORS_BACKGROUND` 后台身体传感器权限，用于更好地管理应用在后台时访问身体传感器（例如心率、体温和血氧饱和度等）的行为。现在应用在后台使用身体传感器，除了要请求现有的 BODY_SENSORS 权限外，还需要请求 BODY_SENSORS_BACKGROUND 权限（这与 ACCESS_FINE_LOCATION 和 ACCESS_BACKGROUND_LOCATION 的关系类似）。

### 2.3 IntentFilter 会屏蔽不匹配的 Intent

当您的应用向以 Android 13 或更高版本为目标平台的其他应用的导出组件发送 Intent 时，仅当该 Intent 与接收应用中的 `<intent-filter>` 元素匹配时，系统才会传送该 intent。

> **提示：** 因为我不理解这个特性的真正含义，所以这里直接复制粘贴了官方文档原话。你理解的话在评论里分享下。
> 

### 2.4 动态注册广播接收器改进

在旧版本中，应用动态注册的 BroadcastReceiver 广播接收器会接收到任何应用发送的广播（除非该接收器使用了应用签名权限保护），这会让动态注册的广播接收器存在安全风险。从 Android 13 系统开始，应用动态注册的广播接收器必须显式指出是否允许其他应用访问，即其他应用是否可以向其发送广播。否则，在动态注册时系统会抛出 SecurityException。

```kotlin
// 这相当于静态注册 android:exported="true"
context.registerReceiver(sharedBroadcastReceiver, intentFilter, RECEIVER_EXPORTED)

// 这相当于静态注册 android:exported="false"
context.registerReceiver(privateBroadcastReceiver, intentFilter, RECEIVER_NOT_EXPORTED)
```

## 3. 性能和电池（以 Android 13 为目标版本）

期待官方更新......

---

第 4~6 节介绍的是针对所有应用的应用行为变更和新功能更新，我将这部分更新总结为 3 部分：

- 4.用户体验（所有应用）
- 5.安全和隐私设置（所有应用）
- 6.性能和电池（所有应用）

## 4. 用户体验（所有应用）

### 4.1 多语言支持改进（新功能）

Android 13 系统引入了一系列新的语言特性优化，用以改进多语言用户的应用体验：

- **应用级别语言偏好设置：** 在旧版本中，用户可以通过系统设置来全局切换语言。从 Android 13 开始，系统开始支持应用级别的语言偏好设置，可以在系统设置中针对每个应用设置，也可以在运行时使用以下 API 设置：
    - 平台 API：[LocaleManager#setApplicationLocales](https://developer.android.google.cn/reference/android/app/LocaleManager#setApplicationLocales(android.os.LocaleList))
    - 兼容库 API：[AppCompatDelegate#setApplicationLocales](https://developer.android.google.cn/reference/androidx/appcompat/app/AppCompatDelegate#setApplicationLocales(androidx.core.os.LocaleListCompat))
- **改进日语文本换行：** 从 Android 13 系统开始，可以通过为 TextView 设置 [android:lineBreakWordStyle="phrase"](https://developer.android.google.cn/reference/android/R.attr#lineBreakWordStyle) 来改进日语文本的换行效果。TextView 会按照 Bunsetsu（最小自然语素单元）或短语，而不是单个字符来进行文本换行。例如，下图是启用了短语样式的日语文本换行（下方）和未启用短语样式的日语文本换行（上方）。
  
    <p align='left'>
    <img src="https://user-images.githubusercontent.com/25008934/164178782-000edddd-557d-44b2-b243-28e85fd9a48d.png" width = "300" />
    </p>
    
- **改进非拉丁字母行高：** Android 13 通过使用适合每种语言的行高，改进了非拉丁文字（例如泰米尔语、缅甸语、泰卢固语和藏语）的显示，可防止字符被裁剪，这个改动需要以 Android 13 为目标版本。例如：
  
    <p align='left'>
    <img src="https://user-images.githubusercontent.com/25008934/164178822-a49bfd10-0539-45fe-8321-fe230cdc0f2a.png" width = "300" />
    </p>
    
### 4.2 自适应主题的应用图标（新功能）

Android 8 系统中引入了自适应图标，可以在不同厂商设备的 Launcher 上显示不同形状的应用图标。**如果说 Android 8 的图标是自适应形状的应用图标，那么 Android 13 就是在此基础上再推出了自适应主题的应用图标**。用户可以调节 Launcher 中的主题色调，应用图标颜色会自适应调整配色。要支持此功能，应用必须在原有的自适应图标 <adaptive-icon> 上增加一张单色图标，例如：

`res/mipmap-anydpi-v26/ic_launcher.xml`

```kotlin
<adaptive-icon >
    <background android:drawable="..." />
    <foreground android:drawable="..." />
    <monochrome android:drawable="@drawable/myicon" />
</adaptive-icon>
```

在 Manifest 文件中引用该图标，例如：

```kotlin
<application
		...
    android:icon="@mipmap/ic_launcher"
    ...>
</application>
```

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164178914-4c2373fb-6e47-4720-979e-eddf138e0122.gif" width = "300" />
</p>

## 5. 安全和隐私设置（所有应用）

### 5.1 通知运行时权限（新功能）

Android 13 系统引入了新的运行时权限 —— `android.permission.POST_NOTIFICATION` 通知权限，用于管理应用发送系统通知的能力。如果用户拒绝授予权限，则应用的所有通知渠道（Channel）都会别屏蔽，这类似于用户在系统设置中手动关闭应用通知后发生的行为。**可以看出，这次的改动 Google 是希望加强对应用通知行为的管理**，现在每个新安装的应用都会发一大堆通知，造成通知栏总是被一堆不重要的消息占满，用户只能一个个去关闭通知开关。

```kotlin
<manifest ...>
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
    <application ...>
        ...
    </application>
</manifest>
```

与其他运行时权限请求类似，请求通知权限也应该遵循最小必要原则。建议是在合适的业务流程节点或者用户体验峰值再请求，以便用户明确了解接收通知能带来的好处。例如：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164179064-80858255-4737-410e-8a14-acdacf4f2f1f.png" width = "950" />
</p>

> **提示：** 通过调用 [areNotificationsEnabled()](https://developer.android.google.cn/reference/android/app/NotificationManager#areNotificationsEnabled())，可以判断用户是否已打开应用的通知开关。
> 

为了降低新权限的影响，从低版本升级到 Android 13 的设备上已安装的应用，系统会临时授予通知权限，前提是该应用本身是有通知的资格的：应用具有通知渠道，并且用户在低版本时并未关闭该应用的通知开关。

- 以 Android 12 或更低版本为目标版本的应用：临时授权会一致有效，直到用户在通知权限授权对话框中明确关闭权限；
- 以 Android 13 或更高版本为目标版本的应用：临时授权会持续到首次启动应用为止。

相关资料：

- [通知运行时权限](https://developer.android.google.cn/about/versions/13/changes/notification-permission) —— 官方文档
- [请求应用权限](https://developer.android.google.cn/training/permissions/requesting#handle-denial) —— 官方文档

### 5.2 **可降级权限（新功能）**

从 Android 13 系统开始，应用可以主动撤销用户已授予的运行时权限，这能够在不再需要权限后更好地保护用户隐私。通过调用 [revokeOwnPermissionsOnKill()](https://developer.android.google.cn/reference/android/content/Context#revokeOwnPermissionsonKill(java.util.Collection%3Cjava.lang.String%3E)) 可以撤销特定的权限或权限组。另外，撤销前台权限时，其对应的后台权限也会被撤销（例如 BODY_SENSORS  & BODY_SENSORS_BACKGROUND）。

### 5.3 照片选择器（新功能）

Android 13 系统引入了新的 [照片选择器](https://developer.android.google.cn/about/versions/13/features/photopicker) 功能，允许用户只向应用提供特定选择的图片或视频，而不是像旧版本那样直接授予整个媒体库的访问权限，这个功能与 IOS 14 “仅允许应用访问部分照片” 是类似的。图片选择器可以更好地保护用户隐私，并且应用不再需要请求媒体库运行时访问权限。

```kotlin
请求：
val maxNumPhotosAndVideos = 10
val intent = Intent(MediaStore.ACTION_PICK_IMAGES)
// 设置获取的图片或视频最大数量
intent.putExtra(MediaStore.EXTRA_PICK_IMAGES_MAX, maxNumPhotosAndVideos)
startActivityForResult(intent, PHOTO_PICKER_MULTI_SELECT_REQUEST_CODE)
```

> 提示： 文件数量上限的最大数字存在平台限制 `MediaStore#getPickImagesMaxLimit()`。
> 

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164179132-855b1008-fe54-4225-84e3-ee062b12110b.gif" width = "300" />
</p>

相关资料：[照片选择器](https://developer.android.google.cn/about/versions/13/features/photopicker)

## 6. 性能和电池（所有应用）

### 6.1 前台服务 FGS 管理器（新功能）

Android 13 系统引入了前台服务 FGS 管理器功能，它会显示当前正在运行前台服务的应用列表，并且每个应用旁边都有一个 “停止” 按钮。当用户点击 “停止” 按钮时，系统不仅会关闭该前台服务，还会停止整个应用。例如：

<p align='left'>
<img src="https://user-images.githubusercontent.com/25008934/164179219-7db0abdf-2808-4386-9808-52753da2ad98.png" width = "650" />
</p>

**可以看出，这次改动 Google 是希望提高用户对前台服务的控制性**。在旧版本的前台服务并没有直接的停止按钮，只有一些些友好的应用会在前台服务通知中使用可操作性的关闭按钮。

相关资料：[前台服务 (FGS) 任务管理器](https://developer.android.google.cn/about/versions/13/changes/fgs-manager)

### 6.2 JobScheduler 预提取作业优化

JobScheduler 预提取作业是 Android 9 引入的机制，通过调用 [JobInfo.Builder.setPrefetch(true)](https://developer.android.google.cn/reference/android/app/job/JobInfo.Builder#setPrefetch(boolean)) 能够将该作业标记为 “预提取” 作业，理想情况下，开发者的预期是该作业应该在应用下一次启动前一点运行，以提升用户体验。

在旧版本中，系统只会在有充足的过剩资源时，才会允许预提取作业运行。从 Android 13 开始，系统会更智能地基于机器学习预测应用下次启动的时间，并根据该估算值执行预提取作业。此外，预提取任务也不再允许设置最后执行期限 [setOverrideDeadline(long)](https://developer.android.google.cn/reference/android/app/job/JobInfo.Builder#setOverrideDeadline(long))。

### 6.3 省电措施改进

Android 13 系统引入了一些新的电池省电措施，以便为系统提供更多方法来管理电池续航时间。

- **更新了应用进入 “受限” 待机分区的规则：** App Standby Buckets 应用待机分区是 Android 9 引入的电池管理功能，“受限” 待机分区是 Android 12 新增的待机分区，主要面向消耗大量系统资源的应用（目前有 “活跃、工作集、常用、极少使用、受限、从未使用” 等待机分区）；
- **更新了 “受限” 后台电池电量的新限制：** 后台电量限制是 Android 9 引入的电池管理功能，能够让用户调整应用处于后台运行时可以执行的工作量（目前有 “无限制、优化（默认）、受限” 等选项）；
- **新增一个电量提醒系统通知：** Android 13 引入了一个新的系统通知，当应用在 24 小时内消耗了大量电池电量时会显示；
- **新增一个前台服务提醒系统通知：** Android 13 引入了一个新的系统通知，当应用的某项前台服务在 24 小时内至少运行了 20 小时时会显示；

相关资料：[电池资源利用率](https://developer.android.google.cn/about/versions/13/changes/battery#system-notification-long-running-fgs)

**以下变更相对冷门，实用价值较低，本文暂且按住不表：**

新功能 - 用户体验 - 快捷磁贴放置 API

新功能 - 用户体验 - 多语言用户 - 文本转换 API & Unicode 库更新

新功能 - 用户体验 - 更快断字 & 彩色矢量字体 & 蓝牙 LE 音频 & MIDI 2.0

新功能 - 隐私安全 - APK 签名 v3.1

新功能 - 图形 - 可编程的着色器

新功能 - 核心功能 - OpenJDK 11 更新
