---
description: Power up your Unity games with Particle Network SDKs.
---

# Unity

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Install Unity 2020.3.26f1 or later. Earlier versions may also be compatible but will not be actively supported.&#x20;
* _(iOS only)_ Install the following:
  * Xcode 13.3.1 or higher
  * CocoaPods 1.10.0 or higher
* Make sure that your Unity project meets these requirements:
  * **For iOS** — targets iOS 12 or higher
  *   **For Android** — Minimum API Level 23 or higher,Targets API level 31 or higher

      **Note:** If you encounter an "Error while dexing" error when performing Android builds using Unity 2017 or 2018, see the [FAQ](https://firebase.google.com/docs/unity/troubleshooting-faq#desugaring) for possible workarounds.
* Set up a physical device or use an emulator to run your app.
  * **For iOS** — Set up a physical iOS device or use the iOS simulator.
  * **For Android** — [Emulators](https://developer.android.com/studio/run/managing-avds) must use an emulator image with Google Play.
* ###

### Create a Particle Project and App

Before you can add our Auth Service to your Unity game, you need to create a Particle project to connect to your iOS and Android app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

### Import Unity package

#### Download and import Unity package

[https://github.com/Particle-Network/particle-unity/releases](https://github.com/Particle-Network/particle-unity/releases)

### Replace Particle Network configuration  <a href="#add-config-file" id="add-config-file"></a>

* **For iOS** — Click **Download GoogleService-Info.plist**.
*   **For Android** —  Configuration file path is  Assets/Plugins/Android/gradleTemplate.properties



    ```
    particle.network.project_client_key=Your Project Client Key
    particle.network.project_id=Your Project Client ID
    particle.network.app_id=Your App ID

    //like this
    particle.network.project_client_key=cKtHdeHis0ghNom64w7mdNkGYX5rmRr0jLlIKatY
    particle.network.project_id=fa0fe0e8-f1bb-47ff-88bb-4fa31711e7b3
    particle.network.app_id=859fada8-48ad-441b-b8e6-cde15ae1b48f

    ```

###

###



### Android、iOS Native Service Config <a href="#add-sdks" id="add-sdks"></a>

**For Android** — Configuration file path is  Assets/Plugins/Android/launcherTemplate.gradle

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



**For iOS** — [Emulators](https://developer.android.com/studio/run/managing-avds) must use an emulator image with Google Play.

### &#x20;Initialize the SDK





\
