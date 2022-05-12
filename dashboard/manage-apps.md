# Manage Apps

After the project has been successfully created, you can add apps to it.

Here, we'll detail the process of adding apps to your project.

### Adding an Android App

1. Select the project you'd like to add an app to and enter its project overview page
2. Select **"Add Android App"**&#x20;
3. Input relevant information to the Android app
   * App name: this is only displayed within the dashboard, used to distinguish between different apps for easy management.
   * Package Name: often referred to as an _application ID,_ it uniquely identifies your app on the device and in the Google Play Store. Find your app's **package name** in your module (app-level) Gradle file, usually `app/build.gradle` (e.g. `com.yourcompany.yourproject`). The package name value is case-sensitive.
4. Save your app information

### Adding an iOS App

1. Select the project you'd like to add an app to and enter its project overview page
2. Select **"Add iOS App"**&#x20;
3. Input relevant information to the iOS app
   * App Name: this is only displayed within the dashboard, used to distinguish between different apps for easy management.
   * Apple Bundle ID: this uniquely identifies an app in Apple's ecosystem. Open your project in Xcode, select the top-level app in the project navigator, then select the **General** tab. The value of the **Bundle Identifier** is the **Bundle ID** (e.g. `com.yourcompany.yourproject`). The Bundle ID value is case-sensitive.
4. Save your app information



Deleting an app in a project is not currently supported.
