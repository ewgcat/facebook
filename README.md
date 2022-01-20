# facebook 广告SDK接入
## 一、 Android接入FacebookSDK
### 1、将以下代码添加至模块级 `/app/build.gradle`，并放在 `dependencies`之前：

```
repositories {
    mavenCentral() 
}
```
### 2、将配置最新版 Facebook SDK 的实现依赖项添加到 `build.gradle` 文件中
```
dependencies {
```
    implementation 'androidx.annotation:annotation:1.0.0'
    implementation 'com.facebook.android:audience-network-sdk:6.+'
```
}
```
### 3.初始化 Audience Network SDK
#### 3.1、 5.3.0以上显式初始化。
```
AudienceNetworkAds.initialize(context);
```
#### 3.2、 Audience Network SDK 初始化帮手类示例

以下帮手类示例将展示调用 Android 版 Audience Network SDK 初始化方法的具体操作。

在应用内定义此类别后，您便可在 `Application.onCreate()` 或 `Activity` 所有包含广告的 `Activity.onCreate()` 方法中用 `AudienceNetworkInitializeHelper.initialize(context)`，以对 SDK 进行初始化。
```
/**
 * Sample class that shows how to call initialize() method of Audience Network SDK.
 */
public class AudienceNetworkInitializeHelper
    implements AudienceNetworkAds.InitListener {

    /**
     * It's recommended to call this method from Application.onCreate().
     * Otherwise you can call it from all Activity.onCreate()
     * methods for Activities that contain ads.
     *
     * @param context Application or Activity.
     */
    static void initialize(Context context) {
        if (!AudienceNetworkAds.isInitialized(context)) {
            if (DEBUG) {
                AdSettings.turnOnSDKDebugger(context);
            }

            AudienceNetworkAds
                .buildInitSettings(context)
                .withInitListener(new AudienceNetworkInitializeHelper())
                .initialize();
        }
    }

    @Override
    public void onInitialized(AudienceNetworkAds.InitResult result) {
        Log.d(AudienceNetworkAds.TAG, result.getMessage());
    }
}
```
#### 3.3、API
https://developers.facebook.com/docs/audience-network/reference/android/?version=v6.4.0

## 二、 iOS
###  1. CocoaPods 

#### 1.1、 Add the following line to your project's Podfile.
```
pod 'FBAudienceNetwork'
```
#### 1.2、  运行 `pod install` 命令。
```
pod install
```
### 2.初始化 Audience Network SDK
#### 2.1、  Open your project file (or workspace if you are using CocoaPods),  Add the following code snippet in your App Delegate
```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

  FBAudienceNetworkAds.initialize(with: nil, completionHandler: nil)

  // Pass user's consent after acquiring it. For sample app purposes, this is set to YES.
  FBAdSettings.setAdvertiserTrackingEnabled(true)

  // Your app initialization logic goes here...
  return true
}
```
#### 2.2、 [启用广告追踪功能](https://developers.facebook.com/docs/audience-network/setting-up/platform-setup/ios/advertising-tracking-enabled)
If you are using mediation, then you need to implement the `setAdvertiserTrackingEnabled` flag before initializing the mediation SDK in order for us to receive it in the bidding request.

This flag also applies with [test mode enabled](https://developers.facebook.com/docs/audience-network/guides/test)

```
// Set the flag as true 
[FBAdSettings setAdvertiserTrackingEnabled:YES];
// Set the flag as false 
[FBAdSettings setAdvertiserTrackingEnabled:NO];
```
#### 2.3、配置
All publishers will need to use Audience Network SDK 6.2.1 or later in order to monetize with iOS 14.5 users. (We also recommend Audience Network SDK 6.2.1 for iOS 14 users.)

Requirements:

1.  Build with Xcode 12+ (iOS SDK 14+).
1.  Use AN SDK v6.2.1+
1.  Add IDs to `Info.plist`.
Add the following SKAdNetwork IDs to your Xcode project’s `Info.plist` as shown in the following example. For more information, see the [Configuring a Source App](https://developer.apple.com/documentation/storekit/skadnetwork/configuring_a_source_app) topic in the Apple StoreKit documentation.
```
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>v9wttpbfk9.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>n38lu8286q.skadnetwork</string>
    </dict>
</array>
```
#### 2.4 、API 
https://developers.facebook.com/docs/audience-network/reference/ios/?version=v6.3.0
