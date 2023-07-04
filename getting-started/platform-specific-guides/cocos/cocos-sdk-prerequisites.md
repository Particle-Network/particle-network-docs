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
  * **For iOS** — targets iOS 14 or higher
  * **For Android** — Minimum API Level 23 or higher, Targets API level 31 or higher, Pack apk must be with exporting project to Android Studio, [change Java SDK version to 11](https://stackoverflow.com/questions/66449161/how-to-upgrade-an-android-project-to-java-11)

### Create a Particle Project and App

Before you can add our Auth Service to your Cocos game, you need to create a Particle project to connect to your iOS and Android app. Visit [Particle Dashboard](../../dashboard/) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)



### Import Cocos package

1. Copy [assets/Mobile/Core](https://github.com/Particle-Network/particle-cocos/tree/mobile/assets/Mobile/Core) into your project, put it in the assets directory.
2. Copy [native/engine](https://github.com/Particle-Network/particle-cocos/tree/mobile/native/engine) folder into your project, put it in the native directory.

### Android, Native Service Config <a href="#add-sdks" id="add-sdks"></a>

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

Make sure Base58.swift, Model.swift and ParticleAuthPlugin.swift TargetMemberShip is selected your app, If Xcode asks whether to create a Swift bridging file, click "Yes".

![](<../../../.gitbook/assets/image (11).png>)

![](<../../../.gitbook/assets/image (10).png>)

5.In AppDelegate, add these code in `(BOOL)application:openURL:options:`method.

```objectivec
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
    return [ParticleAuthSchemeManager handleUrl:url];
}
```

and add import file in top of file&#x20;

```objectivec
// for example, our demo project name is ParticleCocosDemo_mobile
// so we import like this.
#import "ParticleCocosDemo_mobile-Swift.h"

// if your project name is MyCocosGame, your need import your project name like this
#import "MyCocosGame-Swift.h"
```

6.JsbBridgeTest.m and JsbBridgeTest.h files are very important as they are the core of bridging JS and native code. Please ensure that the code inside them is consistent with the demo or contains the code from the demo.

7\. Configure your app scheme URL, select your app from `TARGETS`,  under `Info` section, click + to add the `URL types`, and paste your scheme in `URL Schemes`

Your scheme URL should be "pn" + your project app uuid.

For example, if your project app id is "63bfa427-cf5f-4742-9ff1-e8f5a1b9828f", your scheme URL is "pn63bfa427-cf5f-4742-9ff1-e8f5a1b9828f".

![Config scheme url](<../../../.gitbook/assets/image (1) (2) (1).png>)

8 Edit Podfile, you should follow [Podfile required](../../../developers/auth-service/sdks/ios.md#edit-podfile) to edit Podfile.



### FAQ

### Cocos Creator

### iOS

### Android
