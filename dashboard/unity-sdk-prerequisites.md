---
description: Integrate powerful Web3.0 Unity SDK in minutes
---

# Unity SDK Prerequisites

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install Unity 2020.3.26f1 or later. Earlier versions may also be compatible but will not be actively supported.&#x20;
* _(iOS only)_ Install the following:
  * Xcode 13.3.1 or higher
  * CocoaPods 1.10.0 or higher
* Make sure that your Unity project meets these requirements:
  * **For iOS** — targets iOS 13 or higher
  * **For Android** — Minimum API Level 23 or higher, Targets API level 31 or higher, Pack apk must be with exporting project to Android Studio, [change Java SDK version to 11](https://stackoverflow.com/questions/66449161/how-to-upgrade-an-android-project-to-java-11)

### Create a Particle Project and App

Before you can add our Auth Service to your Unity game, you need to create a Particle project to connect to your iOS and Android app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Import Unity package

#### Download and import the Unity package

[https://github.com/Particle-Network/particle-unity/releases](https://github.com/Particle-Network/particle-unity/releases)

### **iOS**

* **Configure scheme url in Unity Editor**

1. Open the [iOS Player Settings](https://docs.unity3d.com/Manual/class-PlayerSettingsiOS.html) window (menu: **Edit** > **Project Settings** > **Player Settings**, then select **iOS**).
2. Select **Other**, then scroll down to **Configuration**.
3. Expand the **Supported URL schemes** section and, in the **Element 0** field, enter the URL scheme to associate with your application. For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f"

* **Remove other service code if you don't need them.**

1. In ParticleNetworkIOSBridge.cs, there are 5 part ParticleNetworkBase, ParticleAuthService, ParticleWalletAPI, ParticleWalletGUI, ParticleConnect, works for interact with iOS native. ParticleNetworkBase is required,&#x20;
2. ParticleAuthService if required for Auth Service,&#x20;
3. ParticleConnect is required for Connect Service.
4. You can remove other codes if you don't need them.

* **Configure Xcode project after iOS build.**

1.  Create a Podfile if you don't already have one. From the root of your project directory, run the following command:

    ```ruby
    pod init
    ```
2.  To your Podfile, add select these pods that you want to use in your app:

    <pre class="language-ruby"><code class="lang-ruby"><strong>
    </strong><strong>// ParticleWalletGUI contains all other services.
    </strong>pod 'ParticleWalletGUI'
    // ParticleAuthService provide auth service.
    <strong>pod 'ParticleAuthService'
    </strong>// PaticleConnectService privide wallet connect and auth service.
    pod 'ParticleConnect'
    pod 'CommonConnect'</code></pre>
3.  &#x20;Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

    ```ruby
    pod install --repo-update
    ```

    ```ruby
    open your-project.xcworkspace
    ```

{% hint style="info" %}
### Xcode 14

#### We have released new version for Xcode 14, if you want to develop with Xcode 14, you should specify version, for more versions information, please explore our github [connect](https://github.com/Particle-Network/particle-connect-ios) page and [wallet](https://github.com/Particle-Network/particle-ios) page.
{% endhint %}

* **Configure Project information.**

1. Create a **ParticleNetwork-Info.plist** into the root of your Xcode project
2. Copy the following text into this file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>PROJECT_UUID</key>
	<string>YOUR_PROJECT_UUID</string>
	<key>PROJECT_CLIENT_KEY</key>
	<string>YOUR_PROJECT_CLIENT_KEY</string>
	<key>PROJECT_APP_UUID</key>
	<string>YOUR_PROJECT_APP_UUID</string>
</dict>
</plist>

```

3\. Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard

### **Android**&#x20;

#### Configuration file path is  Assets/Plugins/Android/gradleTemplate.properties

```
particle.network.project_client_key=Your Project Client Key
particle.network.project_id=Your Project Client ID
particle.network.app_id=Your App ID

//like this
particle.network.project_client_key=cKtHdeHis0ghNom64w7mdNkGYX5rmRr0jLlIKatY
particle.network.project_id=fa0fe0e8-f1bb-47ff-88bb-4fa31711e7b3
particle.network.app_id=859fada8-48ad-441b-b8e6-cde15ae1b48f

```



### Android, iOS Native Service Config <a href="#add-sdks" id="add-sdks"></a>

**Android** — Configuration file path is  Assets/Plugins/Android/launcherTemplate.gradle

```
dependencies {
    implementation project(':unityLibrary')
    def sdkVersion = "0.4.1" //find the latest version of the sdk here:https://search.maven.org/search?q=g:network.particle
    //particle auth service (Required)  
    implementation "network.particle:auth-service:${sdkVersion}"
    //particle api service (Optional.) If you don't want to use the API service, you can remove this dependency.
    implementation "network.particle:api-service:${sdkVersion}"
    //particle wallet service (Optional.) If you don't want to use the wallet service, you can remove this dependency.
    implementation "network.particle:wallet-service:${sdkVersion}"
    //particle unity bridge (Required) 
    implementation "network.particle:unity-bridge:${sdkVersion}"
}
```

**iOS**&#x20;

1. Download `UnityManger.swift`, `Unity-iPhone-Bridging-Header.h` and `AppDelegate.swift` from under github `/Assets/Plugins/iOS/.Swift` , Copy files into the root of your Xcode project. Xcode will ask you if auto create Bridging file, click yes.

![](<../.gitbook/assets/image (2).png>)

2\. Remove `main.mm` under MainApp folder.

3\. Under `/Assets/Plugins/iOS` is NativeCallProxy files, they are requested by Unity to interact with iOS code. Remove code under Particle Wallet API and Particle Wallet GUI if you don't need wallet service.

4\. In `UnityManger.swift`, it has implemented methods defined in `NativeCallProxy.h`

5\. Select NativeCallProxy.h, in the file inspector, check public in Target Membership.

![](<../.gitbook/assets/image (3) (2).png>)

6\. If you are skilled in iOS, you can modify these files as you like. For example, add other services.
