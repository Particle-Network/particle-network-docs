---
description: Integrate powerful Web3.0 Cocos SDK in minutes
---

# Cocos SDK Prerequisites

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install Cocos Creator 3.7.2 or later. Earlier versions may also be compatible but will not be actively supported.&#x20;
* _(iOS only)_ Install the following:
  * Xcode 14.1 or higher
  * CocoaPods 1.11.0 or higher
* Make sure that your Unity project meets these requirements:
  * **For iOS** — targets iOS 13 or higher
  * **For Android** — Minimum API Level 23 or higher, Targets API level 31 or higher, Pack apk must be with exporting project to Android Studio, [change Java SDK version to 11](https://stackoverflow.com/questions/66449161/how-to-upgrade-an-android-project-to-java-11)

### Create a Particle Project and App

Before you can add our Auth Service to your Cocos game, you need to create a Particle project to connect to your iOS and Android app. Visit [Particle Dashboard](../../dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)



### Import Cocos package

1. Copy assets/Mobile/Core into your project, put it in the assets directory.
2. Copy native/engine folder into your project, put it in the native directory.
3.

### **iOS**

* **Configure Xcode project after iOS build.**

1.  Create a Podfile if you don't already have one. From the root of your Xcode project directory, run the following command:

    ```ruby
    pod init
    ```
2.  Clear the contents of the Podfile, and paste the following code into it. If you have integrated other native SDKs through CocoaPods, you only need to update the original Podfile and add our SDK declaration under the main app.,&#x20;

    ```ruby
    ENV['SWIFT_VERSION'] = '5'
    platform :ios, '13.0'
    source 'https://github.com/CocoaPods/Specs.git'

    target 'boost_container' do
      use_frameworks!
    end

    target 'cocos_engine' do
      use_frameworks!
    end

    target 'ParticleCocosDemo-mobile' do
      use_frameworks!
      # Auth SDK
      pod 'ParticleAuthService'
      pod 'ParticleNetworkBase'
      
    end

    post_install do |installer|
      installer.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
        config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
          end
        end
    end

    ```
3.  &#x20;Install the pods, then open your `.xcworkspace` file to see the project in Xcode:

    ```ruby
    pod install --repo-update
    open your-project.xcworkspace
    ```

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

3\.  Replace `YOUR_PROJECT_UUID`, `YOUR_PROJECT_CLIENT_KEY`, and `YOUR_PROJECT_APP_UUID` with the new values created in your Dashboard.

4\.  Drag the following files into your project and set the Target as the main app.



5\. Configure your app scheme URL, select your app from `TARGETS`,  under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)









### Android, iOS Native Service Config <a href="#add-sdks" id="add-sdks"></a>

**Android** — Configuration file path is  $exportDir/proj/gradle.properties

```properties
android.enableJetifier=true
# replace your project id
PN_PROJECT_ID = 77eeb8ec-dd64-48e9-b786-aa340ce42a40  
#replace your client key
PN_PROJECT_CLIENT_KEY = cpahKexYg891D1BQ3IwMv9fd6E5JKdizMZcFRZ6x 
#replace your app id
PN_APP_ID = 96ad2ff1-4a57-4069-90da-9306bf1492e1 
```



**iOS**&#x20;

1. Download `UnityManger.swift`, `Unity-iPhone-Bridging-Header.h` and `AppDelegate.swift` from under github `/Assets/Plugins/iOS/.Swift` , Copy files into the root of your Xcode project. Xcode will ask you if auto create Bridging file, click yes.

![](<../../../.gitbook/assets/image (2) (1) (1).png>)

2.Make sure Build Settings, Swift Compiler - General, has Objective-C Bridging Header, its connect is Unity-iPhone-Bridging-Header.h 's local path.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3\. Remove `main.mm` under MainApp folder.

4\. Under `/Assets/Plugins/iOS` is NativeCallProxy files, they are requested by Unity to interact with iOS code. Remove code under Particle Wallet API and Particle Wallet GUI if you don't need wallet service.

5\. In `UnityManger.swift`, it has implemented methods defined in `NativeCallProxy.h`

6\. Select NativeCallProxy.h, in the file inspector, check public in Target Membership.

![](<../../../.gitbook/assets/image (3) (1) (2).png>)

7\. If you want to use ParticleConnect, you should add LSApplicationQueriesSchemes to info.plist.

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>metamask</string>
    <string>phantom</string>
    <string>bitkeep</string>
    <string>imtokenv2</string>
    <string>rainbow</string>
    <string>trust</string>
    <string>gnosissafe</string>
    <string>zerion</string>
    <string>mathwallet</string>
    <string>1inch</string>
    <string>awallet</string>
    <string>bitpie</string>
</array>
```

8\. If you are skilled in iOS, you can modify these files as you like. For example, add other services.

### FAQ

### Cocos Creator

### iOS

### Android
