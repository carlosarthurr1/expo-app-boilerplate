# .cursorrules

```
// React Native Expo .cursorrules

// React Native Expo best practices

const reactNativeExpoBestPractices = [
  "Use functional components with hooks",
  "Utilize Expo SDK features and APIs",
  "Implement proper navigation using React Navigation",
  "Use Expo's asset system for images and fonts",
  "Implement proper error handling and crash reporting",
  "Utilize Expo's push notification system",
];

// Folder structure

const folderStructure = `
assets/
src/
  components/
  screens/
  navigation/
  hooks/
  utils/
App.js
app.json
`;

// Additional instructions

const additionalInstructions = `
1. Use TypeScript for type safety
2. Implement proper styling using StyleSheet
3. Utilize Expo's vector icons
4. Use Expo's secure store for sensitive data
5. Implement proper offline support
6. Follow React Native best practices for performance
7. Use Expo's OTA updates for quick deployments
`;

```

# .gitignore

```
# Learn more https://docs.github.com/en/get-started/getting-started-with-git/ignoring-files

# dependencies
node_modules/

# Expo
.expo/
dist/
web-build/
expo-env.d.ts

# Native
*.orig.*
*.jks
*.p8
*.p12
*.key
*.mobileprovision

# Metro
.metro-health-check*

# debug
npm-debug.*
yarn-debug.*
yarn-error.*

# macOS
.DS_Store
*.pem

# local env files
.env*.local

# typescript
*.tsbuildinfo

app-example

```

# android/.gitignore

```
# OSX
#
.DS_Store

# Android/IntelliJ
#
build/
.idea
.gradle
local.properties
*.iml
*.hprof
.cxx/

# Bundle artifacts
*.jsbundle

```

# android/app/build.gradle

```gradle
apply plugin: "com.android.application"
apply plugin: "org.jetbrains.kotlin.android"
apply plugin: "com.facebook.react"

def projectRoot = rootDir.getAbsoluteFile().getParentFile().getAbsolutePath()

/**
 * This is the configuration block to customize your React Native Android app.
 * By default you don't need to apply any configuration, just uncomment the lines you need.
 */
react {
    entryFile = file(["node", "-e", "require('expo/scripts/resolveAppEntry')", projectRoot, "android", "absolute"].execute(null, rootDir).text.trim())
    reactNativeDir = new File(["node", "--print", "require.resolve('react-native/package.json')"].execute(null, rootDir).text.trim()).getParentFile().getAbsoluteFile()
    hermesCommand = new File(["node", "--print", "require.resolve('react-native/package.json')"].execute(null, rootDir).text.trim()).getParentFile().getAbsolutePath() + "/sdks/hermesc/%OS-BIN%/hermesc"
    codegenDir = new File(["node", "--print", "require.resolve('@react-native/codegen/package.json', { paths: [require.resolve('react-native/package.json')] })"].execute(null, rootDir).text.trim()).getParentFile().getAbsoluteFile()

    // Use Expo CLI to bundle the app, this ensures the Metro config
    // works correctly with Expo projects.
    cliFile = new File(["node", "--print", "require.resolve('@expo/cli', { paths: [require.resolve('expo/package.json')] })"].execute(null, rootDir).text.trim())
    bundleCommand = "export:embed"

    /* Folders */
     //   The root of your project, i.e. where "package.json" lives. Default is '../..'
    // root = file("../../")
    //   The folder where the react-native NPM package is. Default is ../../node_modules/react-native
    // reactNativeDir = file("../../node_modules/react-native")
    //   The folder where the react-native Codegen package is. Default is ../../node_modules/@react-native/codegen
    // codegenDir = file("../../node_modules/@react-native/codegen")

    /* Variants */
    //   The list of variants to that are debuggable. For those we're going to
    //   skip the bundling of the JS bundle and the assets. By default is just 'debug'.
    //   If you add flavors like lite, prod, etc. you'll have to list your debuggableVariants.
    // debuggableVariants = ["liteDebug", "prodDebug"]

    /* Bundling */
    //   A list containing the node command and its flags. Default is just 'node'.
    // nodeExecutableAndArgs = ["node"]

    //
    //   The path to the CLI configuration file. Default is empty.
    // bundleConfig = file(../rn-cli.config.js)
    //
    //   The name of the generated asset file containing your JS bundle
    // bundleAssetName = "MyApplication.android.bundle"
    //
    //   The entry file for bundle generation. Default is 'index.android.js' or 'index.js'
    // entryFile = file("../js/MyApplication.android.js")
    //
    //   A list of extra flags to pass to the 'bundle' commands.
    //   See https://github.com/react-native-community/cli/blob/main/docs/commands.md#bundle
    // extraPackagerArgs = []

    /* Hermes Commands */
    //   The hermes compiler command to run. By default it is 'hermesc'
    // hermesCommand = "$rootDir/my-custom-hermesc/bin/hermesc"
    //
    //   The list of flags to pass to the Hermes compiler. By default is "-O", "-output-source-map"
    // hermesFlags = ["-O", "-output-source-map"]

    /* Autolinking */
    autolinkLibrariesWithApp()
}

/**
 * Set this to true to Run Proguard on Release builds to minify the Java bytecode.
 */
def enableProguardInReleaseBuilds = (findProperty('android.enableProguardInReleaseBuilds') ?: false).toBoolean()

/**
 * The preferred build flavor of JavaScriptCore (JSC)
 *
 * For example, to use the international variant, you can use:
 * `def jscFlavor = 'org.webkit:android-jsc-intl:+'`
 *
 * The international variant includes ICU i18n library and necessary data
 * allowing to use e.g. `Date.toLocaleString` and `String.localeCompare` that
 * give correct results when using with locales other than en-US. Note that
 * this variant is about 6MiB larger per architecture than default.
 */
def jscFlavor = 'org.webkit:android-jsc:+'

android {
    ndkVersion rootProject.ext.ndkVersion

    buildToolsVersion rootProject.ext.buildToolsVersion
    compileSdk rootProject.ext.compileSdkVersion

    namespace 'com.cqjack.expoappboilerplate'
    defaultConfig {
        applicationId 'com.cqjack.expoappboilerplate'
        minSdkVersion rootProject.ext.minSdkVersion
        targetSdkVersion rootProject.ext.targetSdkVersion
        versionCode 1
        versionName "1.0.0"
    }
    signingConfigs {
        debug {
            storeFile file('debug.keystore')
            storePassword 'android'
            keyAlias 'androiddebugkey'
            keyPassword 'android'
        }
    }
    buildTypes {
        debug {
            signingConfig signingConfigs.debug
        }
        release {
            // Caution! In production, you need to generate your own keystore file.
            // see https://reactnative.dev/docs/signed-apk-android.
            signingConfig signingConfigs.debug
            shrinkResources (findProperty('android.enableShrinkResourcesInReleaseBuilds')?.toBoolean() ?: false)
            minifyEnabled enableProguardInReleaseBuilds
            proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
            crunchPngs (findProperty('android.enablePngCrunchInReleaseBuilds')?.toBoolean() ?: true)
        }
    }
    packagingOptions {
        jniLibs {
            useLegacyPackaging (findProperty('expo.useLegacyPackaging')?.toBoolean() ?: false)
        }
    }
    androidResources {
        ignoreAssetsPattern '!.svn:!.git:!.ds_store:!*.scc:!CVS:!thumbs.db:!picasa.ini:!*~'
    }
}

// Apply static values from `gradle.properties` to the `android.packagingOptions`
// Accepts values in comma delimited lists, example:
// android.packagingOptions.pickFirsts=/LICENSE,**/picasa.ini
["pickFirsts", "excludes", "merges", "doNotStrip"].each { prop ->
    // Split option: 'foo,bar' -> ['foo', 'bar']
    def options = (findProperty("android.packagingOptions.$prop") ?: "").split(",");
    // Trim all elements in place.
    for (i in 0..<options.size()) options[i] = options[i].trim();
    // `[] - ""` is essentially `[""].filter(Boolean)` removing all empty strings.
    options -= ""

    if (options.length > 0) {
        println "android.packagingOptions.$prop += $options ($options.length)"
        // Ex: android.packagingOptions.pickFirsts += '**/SCCS/**'
        options.each {
            android.packagingOptions[prop] += it
        }
    }
}

dependencies {
    // The version of react-native is set by the React Native Gradle Plugin
    implementation("com.facebook.react:react-android")

    def isGifEnabled = (findProperty('expo.gif.enabled') ?: "") == "true";
    def isWebpEnabled = (findProperty('expo.webp.enabled') ?: "") == "true";
    def isWebpAnimatedEnabled = (findProperty('expo.webp.animated') ?: "") == "true";

    if (isGifEnabled) {
        // For animated gif support
        implementation("com.facebook.fresco:animated-gif:${reactAndroidLibs.versions.fresco.get()}")
    }

    if (isWebpEnabled) {
        // For webp support
        implementation("com.facebook.fresco:webpsupport:${reactAndroidLibs.versions.fresco.get()}")
        if (isWebpAnimatedEnabled) {
            // Animated webp support
            implementation("com.facebook.fresco:animated-webp:${reactAndroidLibs.versions.fresco.get()}")
        }
    }

    if (hermesEnabled.toBoolean()) {
        implementation("com.facebook.react:hermes-android")
    } else {
        implementation jscFlavor
    }
}

```

# android/app/debug.keystore

This is a binary file of the type: Binary

# android/app/proguard-rules.pro

```pro
# Add project specific ProGuard rules here.
# By default, the flags in this file are appended to flags specified
# in /usr/local/Cellar/android-sdk/24.3.3/tools/proguard/proguard-android.txt
# You can edit the include path and order by changing the proguardFiles
# directive in build.gradle.
#
# For more details, see
#   http://developer.android.com/guide/developing/tools/proguard.html

# react-native-reanimated
-keep class com.swmansion.reanimated.** { *; }
-keep class com.facebook.react.turbomodule.** { *; }

# Add any project specific keep options here:

```

# android/app/src/debug/AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

    <application android:usesCleartextTraffic="true" tools:targetApi="28" tools:ignore="GoogleAppIndexingWarning" tools:replace="android:usesCleartextTraffic" />
</manifest>

```

# android/app/src/main/AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  <uses-permission android:name="android.permission.INTERNET"/>
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
  <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
  <uses-permission android:name="android.permission.VIBRATE"/>
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
  <queries>
    <intent>
      <action android:name="android.intent.action.VIEW"/>
      <category android:name="android.intent.category.BROWSABLE"/>
      <data android:scheme="https"/>
    </intent>
  </queries>
  <application android:name=".MainApplication" android:label="@string/app_name" android:icon="@mipmap/ic_launcher" android:roundIcon="@mipmap/ic_launcher_round" android:allowBackup="true" android:theme="@style/AppTheme" android:supportsRtl="true">
    <meta-data android:name="expo.modules.updates.ENABLED" android:value="false"/>
    <meta-data android:name="expo.modules.updates.EXPO_UPDATES_CHECK_ON_LAUNCH" android:value="ALWAYS"/>
    <meta-data android:name="expo.modules.updates.EXPO_UPDATES_LAUNCH_WAIT_MS" android:value="0"/>
    <activity android:name=".MainActivity" android:configChanges="keyboard|keyboardHidden|orientation|screenSize|screenLayout|uiMode" android:launchMode="singleTask" android:windowSoftInputMode="adjustResize" android:theme="@style/Theme.App.SplashScreen" android:exported="true" android:screenOrientation="portrait">
      <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
      </intent-filter>
      <intent-filter>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="android.intent.category.BROWSABLE"/>
        <data android:scheme="myapp"/>
        <data android:scheme="xyz.bentopop.bell"/>
        <data android:scheme="exp+expo-app-boilerplate"/>
      </intent-filter>
    </activity>
  </application>
</manifest>
```

# android/app/src/main/java/com/cqjack/expoappboilerplate/MainActivity.kt

```kt
package com.cqjack.expoappboilerplate
import expo.modules.splashscreen.SplashScreenManager

import android.os.Build
import android.os.Bundle

import com.facebook.react.ReactActivity
import com.facebook.react.ReactActivityDelegate
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.fabricEnabled
import com.facebook.react.defaults.DefaultReactActivityDelegate

import expo.modules.ReactActivityDelegateWrapper

class MainActivity : ReactActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    // Set the theme to AppTheme BEFORE onCreate to support
    // coloring the background, status bar, and navigation bar.
    // This is required for expo-splash-screen.
    // setTheme(R.style.AppTheme);
    // @generated begin expo-splashscreen - expo prebuild (DO NOT MODIFY) sync-f3ff59a738c56c9a6119210cb55f0b613eb8b6af
    SplashScreenManager.registerOnActivity(this)
    // @generated end expo-splashscreen
    super.onCreate(null)
  }

  /**
   * Returns the name of the main component registered from JavaScript. This is used to schedule
   * rendering of the component.
   */
  override fun getMainComponentName(): String = "main"

  /**
   * Returns the instance of the [ReactActivityDelegate]. We use [DefaultReactActivityDelegate]
   * which allows you to enable New Architecture with a single boolean flags [fabricEnabled]
   */
  override fun createReactActivityDelegate(): ReactActivityDelegate {
    return ReactActivityDelegateWrapper(
          this,
          BuildConfig.IS_NEW_ARCHITECTURE_ENABLED,
          object : DefaultReactActivityDelegate(
              this,
              mainComponentName,
              fabricEnabled
          ){})
  }

  /**
    * Align the back button behavior with Android S
    * where moving root activities to background instead of finishing activities.
    * @see <a href="https://developer.android.com/reference/android/app/Activity#onBackPressed()">onBackPressed</a>
    */
  override fun invokeDefaultOnBackPressed() {
      if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.R) {
          if (!moveTaskToBack(false)) {
              // For non-root activities, use the default implementation to finish them.
              super.invokeDefaultOnBackPressed()
          }
          return
      }

      // Use the default back button implementation on Android S
      // because it's doing more than [Activity.moveTaskToBack] in fact.
      super.invokeDefaultOnBackPressed()
  }
}

```

# android/app/src/main/java/com/cqjack/expoappboilerplate/MainApplication.kt

```kt
package com.cqjack.expoappboilerplate

import android.app.Application
import android.content.res.Configuration

import com.facebook.react.PackageList
import com.facebook.react.ReactApplication
import com.facebook.react.ReactNativeHost
import com.facebook.react.ReactPackage
import com.facebook.react.ReactHost
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.load
import com.facebook.react.defaults.DefaultReactNativeHost
import com.facebook.react.soloader.OpenSourceMergedSoMapping
import com.facebook.soloader.SoLoader

import expo.modules.ApplicationLifecycleDispatcher
import expo.modules.ReactNativeHostWrapper

class MainApplication : Application(), ReactApplication {

  override val reactNativeHost: ReactNativeHost = ReactNativeHostWrapper(
        this,
        object : DefaultReactNativeHost(this) {
          override fun getPackages(): List<ReactPackage> {
            val packages = PackageList(this).packages
            // Packages that cannot be autolinked yet can be added manually here, for example:
            // packages.add(new MyReactNativePackage());
            return packages
          }

          override fun getJSMainModuleName(): String = ".expo/.virtual-metro-entry"

          override fun getUseDeveloperSupport(): Boolean = BuildConfig.DEBUG

          override val isNewArchEnabled: Boolean = BuildConfig.IS_NEW_ARCHITECTURE_ENABLED
          override val isHermesEnabled: Boolean = BuildConfig.IS_HERMES_ENABLED
      }
  )

  override val reactHost: ReactHost
    get() = ReactNativeHostWrapper.createReactHost(applicationContext, reactNativeHost)

  override fun onCreate() {
    super.onCreate()
    SoLoader.init(this, OpenSourceMergedSoMapping)
    if (BuildConfig.IS_NEW_ARCHITECTURE_ENABLED) {
      // If you opted-in for the New Architecture, we load the native entry point for this app.
      load()
    }
    ApplicationLifecycleDispatcher.onApplicationCreate(this)
  }

  override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
    ApplicationLifecycleDispatcher.onConfigurationChanged(this, newConfig)
  }
}

```

# android/app/src/main/res/drawable-hdpi/splashscreen_logo.png

This is a binary file of the type: Image

# android/app/src/main/res/drawable-mdpi/splashscreen_logo.png

This is a binary file of the type: Image

# android/app/src/main/res/drawable-xhdpi/splashscreen_logo.png

This is a binary file of the type: Image

# android/app/src/main/res/drawable-xxhdpi/splashscreen_logo.png

This is a binary file of the type: Image

# android/app/src/main/res/drawable-xxxhdpi/splashscreen_logo.png

This is a binary file of the type: Image

# android/app/src/main/res/drawable/ic_launcher_background.xml

```xml
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@color/splashscreen_background"/>
  <item>
    <bitmap android:gravity="center" android:src="@drawable/splashscreen_logo"/>
  </item>
</layer-list>
```

# android/app/src/main/res/drawable/rn_edit_text_material.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Copyright (C) 2014 The Android Open Source Project

     Licensed under the Apache License, Version 2.0 (the "License");
     you may not use this file except in compliance with the License.
     You may obtain a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

     Unless required by applicable law or agreed to in writing, software
     distributed under the License is distributed on an "AS IS" BASIS,
     WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     See the License for the specific language governing permissions and
     limitations under the License.
-->
<inset xmlns:android="http://schemas.android.com/apk/res/android"
       android:insetLeft="@dimen/abc_edit_text_inset_horizontal_material"
       android:insetRight="@dimen/abc_edit_text_inset_horizontal_material"
       android:insetTop="@dimen/abc_edit_text_inset_top_material"
       android:insetBottom="@dimen/abc_edit_text_inset_bottom_material"
       >

    <selector>
        <!--
          This file is a copy of abc_edit_text_material (https://bit.ly/3k8fX7I).
          The item below with state_pressed="false" and state_focused="false" causes a NullPointerException.
          NullPointerException:tempt to invoke virtual method 'android.graphics.drawable.Drawable android.graphics.drawable.Drawable$ConstantState.newDrawable(android.content.res.Resources)'

          <item android:state_pressed="false" android:state_focused="false" android:drawable="@drawable/abc_textfield_default_mtrl_alpha"/>

          For more info, see https://bit.ly/3CdLStv (react-native/pull/29452) and https://bit.ly/3nxOMoR.
        -->
        <item android:state_enabled="false" android:drawable="@drawable/abc_textfield_default_mtrl_alpha"/>
        <item android:drawable="@drawable/abc_textfield_activated_mtrl_alpha"/>
    </selector>

</inset>

```

# android/app/src/main/res/mipmap-anydpi-v26/ic_launcher_round.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@color/iconBackground"/>
    <foreground android:drawable="@mipmap/ic_launcher_foreground"/>
</adaptive-icon>
```

# android/app/src/main/res/mipmap-anydpi-v26/ic_launcher.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@color/iconBackground"/>
    <foreground android:drawable="@mipmap/ic_launcher_foreground"/>
</adaptive-icon>
```

# android/app/src/main/res/mipmap-hdpi/ic_launcher_foreground.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-hdpi/ic_launcher_round.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-hdpi/ic_launcher.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-mdpi/ic_launcher_foreground.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-mdpi/ic_launcher_round.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-mdpi/ic_launcher.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xhdpi/ic_launcher_foreground.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xhdpi/ic_launcher_round.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xhdpi/ic_launcher.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxhdpi/ic_launcher_foreground.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxhdpi/ic_launcher_round.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxhdpi/ic_launcher.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxxhdpi/ic_launcher_foreground.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxxhdpi/ic_launcher_round.webp

This is a binary file of the type: Image

# android/app/src/main/res/mipmap-xxxhdpi/ic_launcher.webp

This is a binary file of the type: Image

# android/app/src/main/res/values-night/colors.xml

```xml
<resources/>
```

# android/app/src/main/res/values/colors.xml

```xml
<resources>
  <color name="splashscreen_background">#ffffff</color>
  <color name="iconBackground">#ffffff</color>
  <color name="colorPrimary">#023c69</color>
  <color name="colorPrimaryDark">#ffffff</color>
</resources>
```

# android/app/src/main/res/values/strings.xml

```xml
<resources>
  <string name="app_name">expo-app-boilerplate</string>
  <string name="expo_system_ui_user_interface_style" translatable="false">automatic</string>
  <string name="expo_splash_screen_resize_mode" translatable="false">contain</string>
  <string name="expo_splash_screen_status_bar_translucent" translatable="false">false</string>
</resources>
```

# android/app/src/main/res/values/styles.xml

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
  <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="android:textColor">@android:color/black</item>
    <item name="android:editTextStyle">@style/ResetEditText</item>
    <item name="android:editTextBackground">@drawable/rn_edit_text_material</item>
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="android:statusBarColor">#ffffff</item>
  </style>
  <style name="ResetEditText" parent="@android:style/Widget.EditText">
    <item name="android:padding">0dp</item>
    <item name="android:textColorHint">#c8c8c8</item>
    <item name="android:textColor">@android:color/black</item>
  </style>
  <style name="Theme.App.SplashScreen" parent="Theme.SplashScreen">
    <item name="windowSplashScreenBackground">@color/splashscreen_background</item>
    <item name="windowSplashScreenAnimatedIcon">@drawable/splashscreen_logo</item>
    <item name="postSplashScreenTheme">@style/AppTheme</item>
  </style>
</resources>
```

# android/build.gradle

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    ext {
        buildToolsVersion = findProperty('android.buildToolsVersion') ?: '35.0.0'
        minSdkVersion = Integer.parseInt(findProperty('android.minSdkVersion') ?: '24')
        compileSdkVersion = Integer.parseInt(findProperty('android.compileSdkVersion') ?: '35')
        targetSdkVersion = Integer.parseInt(findProperty('android.targetSdkVersion') ?: '34')
        kotlinVersion = findProperty('android.kotlinVersion') ?: '1.9.25'

        ndkVersion = "26.1.10909125"
    }
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath('com.android.tools.build:gradle')
        classpath('com.facebook.react:react-native-gradle-plugin')
        classpath('org.jetbrains.kotlin:kotlin-gradle-plugin')
    }
}

apply plugin: "com.facebook.react.rootproject"

allprojects {
    repositories {
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url(new File(['node', '--print', "require.resolve('react-native/package.json')"].execute(null, rootDir).text.trim(), '../android'))
        }
        maven {
            // Android JSC is installed from npm
            url(new File(['node', '--print', "require.resolve('jsc-android/package.json', { paths: [require.resolve('react-native/package.json')] })"].execute(null, rootDir).text.trim(), '../dist'))
        }

        google()
        mavenCentral()
        maven { url 'https://www.jitpack.io' }
    }
}

```

# android/gradle.properties

```properties
# Project-wide Gradle settings.

# IDE (e.g. Android Studio) users:
# Gradle settings configured through the IDE *will override*
# any settings specified in this file.

# For more details on how to configure your build environment visit
# http://www.gradle.org/docs/current/userguide/build_environment.html

# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx512m -XX:MaxMetaspaceSize=256m
org.gradle.jvmargs=-Xmx2048m -XX:MaxMetaspaceSize=512m

# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
# org.gradle.parallel=true

# AndroidX package structure to make it clearer which packages are bundled with the
# Android operating system, and which are packaged with your app's APK
# https://developer.android.com/topic/libraries/support-library/androidx-rn
android.useAndroidX=true

# Enable AAPT2 PNG crunching
android.enablePngCrunchInReleaseBuilds=true

# Use this property to specify which architecture you want to build.
# You can also override it from the CLI using
# ./gradlew <task> -PreactNativeArchitectures=x86_64
reactNativeArchitectures=armeabi-v7a,arm64-v8a,x86,x86_64

# Use this property to enable support to the new architecture.
# This will allow you to use TurboModules and the Fabric render in
# your application. You should enable this flag either if you want
# to write custom TurboModules/Fabric components OR use libraries that
# are providing them.
newArchEnabled=true

# Use this property to enable or disable the Hermes JS engine.
# If set to false, you will be using JSC instead.
hermesEnabled=true

# Enable GIF support in React Native images (~200 B increase)
expo.gif.enabled=true
# Enable webp support in React Native images (~85 KB increase)
expo.webp.enabled=true
# Enable animated webp support (~3.4 MB increase)
# Disabled by default because iOS doesn't support animated webp
expo.webp.animated=false

# Enable network inspector
EX_DEV_CLIENT_NETWORK_INSPECTOR=true

# Use legacy packaging to compress native libraries in the resulting APK.
expo.useLegacyPackaging=false

```

# android/gradle/wrapper/gradle-wrapper.jar

This is a binary file of the type: Binary

# android/gradle/wrapper/gradle-wrapper.properties

```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.10.2-all.zip
networkTimeout=10000
validateDistributionUrl=true
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists

```

# android/gradlew

```
#!/bin/sh

#
# Copyright © 2015-2021 the original authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# SPDX-License-Identifier: Apache-2.0
#

##############################################################################
#
#   Gradle start up script for POSIX generated by Gradle.
#
#   Important for running:
#
#   (1) You need a POSIX-compliant shell to run this script. If your /bin/sh is
#       noncompliant, but you have some other compliant shell such as ksh or
#       bash, then to run this script, type that shell name before the whole
#       command line, like:
#
#           ksh Gradle
#
#       Busybox and similar reduced shells will NOT work, because this script
#       requires all of these POSIX shell features:
#         * functions;
#         * expansions «$var», «${var}», «${var:-default}», «${var+SET}»,
#           «${var#prefix}», «${var%suffix}», and «$( cmd )»;
#         * compound commands having a testable exit status, especially «case»;
#         * various built-in commands including «command», «set», and «ulimit».
#
#   Important for patching:
#
#   (2) This script targets any POSIX shell, so it avoids extensions provided
#       by Bash, Ksh, etc; in particular arrays are avoided.
#
#       The "traditional" practice of packing multiple parameters into a
#       space-separated string is a well documented source of bugs and security
#       problems, so this is (mostly) avoided, by progressively accumulating
#       options in "$@", and eventually passing that to Java.
#
#       Where the inherited environment variables (DEFAULT_JVM_OPTS, JAVA_OPTS,
#       and GRADLE_OPTS) rely on word-splitting, this is performed explicitly;
#       see the in-line comments for details.
#
#       There are tweaks for specific operating systems such as AIX, CygWin,
#       Darwin, MinGW, and NonStop.
#
#   (3) This script is generated from the Groovy template
#       https://github.com/gradle/gradle/blob/HEAD/platforms/jvm/plugins-application/src/main/resources/org/gradle/api/internal/plugins/unixStartScript.txt
#       within the Gradle project.
#
#       You can find Gradle at https://github.com/gradle/gradle/.
#
##############################################################################

# Attempt to set APP_HOME

# Resolve links: $0 may be a link
app_path=$0

# Need this for daisy-chained symlinks.
while
    APP_HOME=${app_path%"${app_path##*/}"}  # leaves a trailing /; empty if no leading path
    [ -h "$app_path" ]
do
    ls=$( ls -ld "$app_path" )
    link=${ls#*' -> '}
    case $link in             #(
      /*)   app_path=$link ;; #(
      *)    app_path=$APP_HOME$link ;;
    esac
done

# This is normally unused
# shellcheck disable=SC2034
APP_BASE_NAME=${0##*/}
# Discard cd standard output in case $CDPATH is set (https://github.com/gradle/gradle/issues/25036)
APP_HOME=$( cd -P "${APP_HOME:-./}" > /dev/null && printf '%s
' "$PWD" ) || exit

# Use the maximum available, or set MAX_FD != -1 to use that value.
MAX_FD=maximum

warn () {
    echo "$*"
} >&2

die () {
    echo
    echo "$*"
    echo
    exit 1
} >&2

# OS specific support (must be 'true' or 'false').
cygwin=false
msys=false
darwin=false
nonstop=false
case "$( uname )" in                #(
  CYGWIN* )         cygwin=true  ;; #(
  Darwin* )         darwin=true  ;; #(
  MSYS* | MINGW* )  msys=true    ;; #(
  NONSTOP* )        nonstop=true ;;
esac

CLASSPATH=$APP_HOME/gradle/wrapper/gradle-wrapper.jar


# Determine the Java command to use to start the JVM.
if [ -n "$JAVA_HOME" ] ; then
    if [ -x "$JAVA_HOME/jre/sh/java" ] ; then
        # IBM's JDK on AIX uses strange locations for the executables
        JAVACMD=$JAVA_HOME/jre/sh/java
    else
        JAVACMD=$JAVA_HOME/bin/java
    fi
    if [ ! -x "$JAVACMD" ] ; then
        die "ERROR: JAVA_HOME is set to an invalid directory: $JAVA_HOME

Please set the JAVA_HOME variable in your environment to match the
location of your Java installation."
    fi
else
    JAVACMD=java
    if ! command -v java >/dev/null 2>&1
    then
        die "ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH.

Please set the JAVA_HOME variable in your environment to match the
location of your Java installation."
    fi
fi

# Increase the maximum file descriptors if we can.
if ! "$cygwin" && ! "$darwin" && ! "$nonstop" ; then
    case $MAX_FD in #(
      max*)
        # In POSIX sh, ulimit -H is undefined. That's why the result is checked to see if it worked.
        # shellcheck disable=SC2039,SC3045
        MAX_FD=$( ulimit -H -n ) ||
            warn "Could not query maximum file descriptor limit"
    esac
    case $MAX_FD in  #(
      '' | soft) :;; #(
      *)
        # In POSIX sh, ulimit -n is undefined. That's why the result is checked to see if it worked.
        # shellcheck disable=SC2039,SC3045
        ulimit -n "$MAX_FD" ||
            warn "Could not set maximum file descriptor limit to $MAX_FD"
    esac
fi

# Collect all arguments for the java command, stacking in reverse order:
#   * args from the command line
#   * the main class name
#   * -classpath
#   * -D...appname settings
#   * --module-path (only if needed)
#   * DEFAULT_JVM_OPTS, JAVA_OPTS, and GRADLE_OPTS environment variables.

# For Cygwin or MSYS, switch paths to Windows format before running java
if "$cygwin" || "$msys" ; then
    APP_HOME=$( cygpath --path --mixed "$APP_HOME" )
    CLASSPATH=$( cygpath --path --mixed "$CLASSPATH" )

    JAVACMD=$( cygpath --unix "$JAVACMD" )

    # Now convert the arguments - kludge to limit ourselves to /bin/sh
    for arg do
        if
            case $arg in                                #(
              -*)   false ;;                            # don't mess with options #(
              /?*)  t=${arg#/} t=/${t%%/*}              # looks like a POSIX filepath
                    [ -e "$t" ] ;;                      #(
              *)    false ;;
            esac
        then
            arg=$( cygpath --path --ignore --mixed "$arg" )
        fi
        # Roll the args list around exactly as many times as the number of
        # args, so each arg winds up back in the position where it started, but
        # possibly modified.
        #
        # NB: a `for` loop captures its iteration list before it begins, so
        # changing the positional parameters here affects neither the number of
        # iterations, nor the values presented in `arg`.
        shift                   # remove old arg
        set -- "$@" "$arg"      # push replacement arg
    done
fi


# Add default JVM options here. You can also use JAVA_OPTS and GRADLE_OPTS to pass JVM options to this script.
DEFAULT_JVM_OPTS='"-Xmx64m" "-Xms64m"'

# Collect all arguments for the java command:
#   * DEFAULT_JVM_OPTS, JAVA_OPTS, JAVA_OPTS, and optsEnvironmentVar are not allowed to contain shell fragments,
#     and any embedded shellness will be escaped.
#   * For example: A user cannot expect ${Hostname} to be expanded, as it is an environment variable and will be
#     treated as '${Hostname}' itself on the command line.

set -- \
        "-Dorg.gradle.appname=$APP_BASE_NAME" \
        -classpath "$CLASSPATH" \
        org.gradle.wrapper.GradleWrapperMain \
        "$@"

# Stop when "xargs" is not available.
if ! command -v xargs >/dev/null 2>&1
then
    die "xargs is not available"
fi

# Use "xargs" to parse quoted args.
#
# With -n1 it outputs one arg per line, with the quotes and backslashes removed.
#
# In Bash we could simply go:
#
#   readarray ARGS < <( xargs -n1 <<<"$var" ) &&
#   set -- "${ARGS[@]}" "$@"
#
# but POSIX shell has neither arrays nor command substitution, so instead we
# post-process each arg (as a line of input to sed) to backslash-escape any
# character that might be a shell metacharacter, then use eval to reverse
# that process (while maintaining the separation between arguments), and wrap
# the whole thing up as a single "set" statement.
#
# This will of course break if any of these variables contains a newline or
# an unmatched quote.
#

eval "set -- $(
        printf '%s\n' "$DEFAULT_JVM_OPTS $JAVA_OPTS $GRADLE_OPTS" |
        xargs -n1 |
        sed ' s~[^-[:alnum:]+,./:=@_]~\\&~g; ' |
        tr '\n' ' '
    )" '"$@"'

exec "$JAVACMD" "$@"

```

# android/gradlew.bat

```bat
@rem
@rem Copyright 2015 the original author or authors.
@rem
@rem Licensed under the Apache License, Version 2.0 (the "License");
@rem you may not use this file except in compliance with the License.
@rem You may obtain a copy of the License at
@rem
@rem      https://www.apache.org/licenses/LICENSE-2.0
@rem
@rem Unless required by applicable law or agreed to in writing, software
@rem distributed under the License is distributed on an "AS IS" BASIS,
@rem WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
@rem See the License for the specific language governing permissions and
@rem limitations under the License.
@rem
@rem SPDX-License-Identifier: Apache-2.0
@rem

@if "%DEBUG%"=="" @echo off
@rem ##########################################################################
@rem
@rem  Gradle startup script for Windows
@rem
@rem ##########################################################################

@rem Set local scope for the variables with windows NT shell
if "%OS%"=="Windows_NT" setlocal

set DIRNAME=%~dp0
if "%DIRNAME%"=="" set DIRNAME=.
@rem This is normally unused
set APP_BASE_NAME=%~n0
set APP_HOME=%DIRNAME%

@rem Resolve any "." and ".." in APP_HOME to make it shorter.
for %%i in ("%APP_HOME%") do set APP_HOME=%%~fi

@rem Add default JVM options here. You can also use JAVA_OPTS and GRADLE_OPTS to pass JVM options to this script.
set DEFAULT_JVM_OPTS="-Xmx64m" "-Xms64m"

@rem Find java.exe
if defined JAVA_HOME goto findJavaFromJavaHome

set JAVA_EXE=java.exe
%JAVA_EXE% -version >NUL 2>&1
if %ERRORLEVEL% equ 0 goto execute

echo. 1>&2
echo ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH. 1>&2
echo. 1>&2
echo Please set the JAVA_HOME variable in your environment to match the 1>&2
echo location of your Java installation. 1>&2

goto fail

:findJavaFromJavaHome
set JAVA_HOME=%JAVA_HOME:"=%
set JAVA_EXE=%JAVA_HOME%/bin/java.exe

if exist "%JAVA_EXE%" goto execute

echo. 1>&2
echo ERROR: JAVA_HOME is set to an invalid directory: %JAVA_HOME% 1>&2
echo. 1>&2
echo Please set the JAVA_HOME variable in your environment to match the 1>&2
echo location of your Java installation. 1>&2

goto fail

:execute
@rem Setup the command line

set CLASSPATH=%APP_HOME%\gradle\wrapper\gradle-wrapper.jar


@rem Execute Gradle
"%JAVA_EXE%" %DEFAULT_JVM_OPTS% %JAVA_OPTS% %GRADLE_OPTS% "-Dorg.gradle.appname=%APP_BASE_NAME%" -classpath "%CLASSPATH%" org.gradle.wrapper.GradleWrapperMain %*

:end
@rem End local scope for the variables with windows NT shell
if %ERRORLEVEL% equ 0 goto mainEnd

:fail
rem Set variable GRADLE_EXIT_CONSOLE if you need the _script_ return code instead of
rem the _cmd.exe /c_ return code!
set EXIT_CODE=%ERRORLEVEL%
if %EXIT_CODE% equ 0 set EXIT_CODE=1
if not ""=="%GRADLE_EXIT_CONSOLE%" exit %EXIT_CODE%
exit /b %EXIT_CODE%

:mainEnd
if "%OS%"=="Windows_NT" endlocal

:omega

```

# android/settings.gradle

```gradle
pluginManagement {
    includeBuild(new File(["node", "--print", "require.resolve('@react-native/gradle-plugin/package.json', { paths: [require.resolve('react-native/package.json')] })"].execute(null, rootDir).text.trim()).getParentFile().toString())
}
plugins { id("com.facebook.react.settings") }

extensions.configure(com.facebook.react.ReactSettingsExtension) { ex ->
  if (System.getenv('EXPO_USE_COMMUNITY_AUTOLINKING') == '1') {
    ex.autolinkLibrariesFromCommand()
  } else {
    def command = [
      'node',
      '--no-warnings',
      '--eval',
      'require(require.resolve(\'expo-modules-autolinking\', { paths: [require.resolve(\'expo/package.json\')] }))(process.argv.slice(1))',
      'react-native-config',
      '--json',
      '--platform',
      'android'
    ].toList()
    ex.autolinkLibrariesFromCommand(command)
  }
}

rootProject.name = 'expo-app-boilerplate'

dependencyResolutionManagement {
  versionCatalogs {
    reactAndroidLibs {
      from(files(new File(["node", "--print", "require.resolve('react-native/package.json')"].execute(null, rootDir).text.trim(), "../gradle/libs.versions.toml")))
    }
  }
}

apply from: new File(["node", "--print", "require.resolve('expo/package.json')"].execute(null, rootDir).text.trim(), "../scripts/autolinking.gradle");
useExpoModules()

include ':app'
includeBuild(new File(["node", "--print", "require.resolve('@react-native/gradle-plugin/package.json', { paths: [require.resolve('react-native/package.json')] })"].execute(null, rootDir).text.trim()).getParentFile())

```

# app.json

```json
{
  "expo": {
    "name": "expo-app-boilerplate",
    "slug": "expo-app-boilerplate",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "myapp",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.cqjack.expoappboilerplate",
      "infoPlist": {
        "ITSAppUsesNonExemptEncryption": false
      }
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/images/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      },
      "package": "com.cqjack.expoappboilerplate"
    },
    "web": {
      "bundler": "metro",
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff"
        }
      ]
    ],
    "experiments": {
      "typedRoutes": true
    },
    "extra": {
      "router": {
        "origin": false
      },
      "eas": {
        "projectId": "e55fcdea-13e2-4c3b-a4c3-59c174184eb5"
      }
    }
  }
}

```

# app/_layout.tsx

```tsx
import { DarkTheme, DefaultTheme, ThemeProvider } from '@react-navigation/native';
import { useFonts } from 'expo-font';
import { Stack } from 'expo-router';
import * as SplashScreen from 'expo-splash-screen';
import { StatusBar } from 'expo-status-bar';
import { useEffect } from 'react';
import { Platform } from 'react-native';
import 'react-native-reanimated';
import { GestureHandlerRootView } from 'react-native-gesture-handler';

import { superwallService } from '@/services/superwall';
import { useColorScheme } from '@/hooks/useColorScheme';
import { OnboardingProvider } from '@/contexts/OnboardingContext';

// Prevent the splash screen from auto-hiding before asset loading is complete.
SplashScreen.preventAutoHideAsync();

export default function RootLayout() {
  const colorScheme = useColorScheme();
  const [loaded] = useFonts({
    SpaceMono: require('../assets/fonts/SpaceMono-Regular.ttf'),
  });

  useEffect(() => {
    if (Platform.OS !== 'web') {
      superwallService.initialize();
    }
  }, []);

  useEffect(() => {
    if (loaded) {
      SplashScreen.hideAsync();
    }
  }, [loaded]);

  if (!loaded) return null;

  return (
    <GestureHandlerRootView style={{ flex: 1 }}>
      <OnboardingProvider>
        <ThemeProvider value={colorScheme === 'dark' ? DarkTheme : DefaultTheme}>
          <Stack screenOptions={{ headerShown: false }}>
            <Stack.Screen name="(tabs)" />
            <Stack.Screen name="onboarding" />
            <Stack.Screen name="+not-found" />
          </Stack>
          <StatusBar style="auto" />
        </ThemeProvider>
      </OnboardingProvider>
    </GestureHandlerRootView>
  );
}

```

# app/(tabs)/_layout.tsx

```tsx
import { Tabs } from 'expo-router';
import React from 'react';
import { Platform } from 'react-native';

import { HapticTab } from '@/components/HapticTab';
import { IconSymbol } from '@/components/ui/IconSymbol';
import TabBarBackground from '@/components/ui/TabBarBackground';
import { Colors } from '@/constants/Colors';
import { useColorScheme } from '@/hooks/useColorScheme';

export default function TabLayout() {
  const colorScheme = useColorScheme();

  return (
    <Tabs
      screenOptions={{
        tabBarActiveTintColor: Colors[colorScheme ?? 'light'].tint,
        headerShown: false,
        tabBarButton: HapticTab,
        tabBarBackground: TabBarBackground,
        tabBarStyle: Platform.select({
          ios: {
            // Use a transparent background on iOS to show the blur effect
            position: 'absolute',
          },
          default: {},
        }),
      }}>
      <Tabs.Screen
        name="index"
        options={{
          title: 'Home',
          tabBarIcon: ({ color }) => <IconSymbol size={28} name="house.fill" color={color} />,
        }}
      />
      <Tabs.Screen
        name="explore"
        options={{
          title: 'Explore',
          tabBarIcon: ({ color }) => <IconSymbol size={28} name="paperplane.fill" color={color} />,
        }}
      />
    </Tabs>
  );
}

```

# app/(tabs)/explore.tsx

```tsx
import { StyleSheet, Image, Platform, TouchableOpacity } from 'react-native';

import { Collapsible } from '@/components/Collapsible';
import { ExternalLink } from '@/components/ExternalLink';
import ParallaxScrollView from '@/components/ParallaxScrollView';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { IconSymbol } from '@/components/ui/IconSymbol';
import { useSuperwall } from '@/hooks/useSuperwall';
import { SUPERWALL_TRIGGERS } from '@/config/superwall';
import { Colors } from '@/constants/Colors';

export default function TabTwoScreen() {
  const { isSubscribed, isLoading, showPaywall } = useSuperwall();

  const handlePremiumFeature = () => {
    if (!isSubscribed && !isLoading) {
      showPaywall(SUPERWALL_TRIGGERS.FEATURE_UNLOCK);
    }
  };

  return (
    <ParallaxScrollView
      headerBackgroundColor={{ light: '#D0D0D0', dark: '#353636' }}
      headerImage={
        <IconSymbol
          size={310}
          color="#808080"
          name="chevron.left.forwardslash.chevron.right"
          style={styles.headerImage}
        />
      }>
      <ThemedView style={styles.titleContainer}>
        <ThemedText type="title">Explore</ThemedText>
      </ThemedView>
      <ThemedText>This app includes example code to help you get started.</ThemedText>
      <Collapsible title="File-based routing">
        <ThemedText>
          This app has two screens:{' '}
          <ThemedText type="defaultSemiBold">app/(tabs)/index.tsx</ThemedText> and{' '}
          <ThemedText type="defaultSemiBold">app/(tabs)/explore.tsx</ThemedText>
        </ThemedText>
        <ThemedText>
          The layout file in <ThemedText type="defaultSemiBold">app/(tabs)/_layout.tsx</ThemedText>{' '}
          sets up the tab navigator.
        </ThemedText>
        <ExternalLink href="https://docs.expo.dev/router/introduction">
          <ThemedText type="link">Learn more</ThemedText>
        </ExternalLink>
      </Collapsible>
      <Collapsible title="Android, iOS, and web support">
        <ThemedText>
          You can open this project on Android, iOS, and the web. To open the web version, press{' '}
          <ThemedText type="defaultSemiBold">w</ThemedText> in the terminal running this project.
        </ThemedText>
      </Collapsible>
      <Collapsible title="Images">
        <ThemedText>
          For static images, you can use the <ThemedText type="defaultSemiBold">@2x</ThemedText> and{' '}
          <ThemedText type="defaultSemiBold">@3x</ThemedText> suffixes to provide files for
          different screen densities
        </ThemedText>
        <Image source={require('@/assets/images/react-logo.png')} style={{ alignSelf: 'center' }} />
        <ExternalLink href="https://reactnative.dev/docs/images">
          <ThemedText type="link">Learn more</ThemedText>
        </ExternalLink>
      </Collapsible>
      <Collapsible title="Custom fonts">
        <ThemedText>
          Open <ThemedText type="defaultSemiBold">app/_layout.tsx</ThemedText> to see how to load{' '}
          <ThemedText style={{ fontFamily: 'SpaceMono' }}>
            custom fonts such as this one.
          </ThemedText>
        </ThemedText>
        <ExternalLink href="https://docs.expo.dev/versions/latest/sdk/font">
          <ThemedText type="link">Learn more</ThemedText>
        </ExternalLink>
      </Collapsible>
      <Collapsible title="Light and dark mode components">
        <ThemedText>
          This template has light and dark mode support. The{' '}
          <ThemedText type="defaultSemiBold">useColorScheme()</ThemedText> hook lets you inspect
          what the user's current color scheme is, and so you can adjust UI colors accordingly.
        </ThemedText>
        <ExternalLink href="https://docs.expo.dev/develop/user-interface/color-themes/">
          <ThemedText type="link">Learn more</ThemedText>
        </ExternalLink>
      </Collapsible>
      <Collapsible title="Animations">
        <ThemedText>
          This template includes an example of an animated component. The{' '}
          <ThemedText type="defaultSemiBold">components/HelloWave.tsx</ThemedText> component uses
          the powerful <ThemedText type="defaultSemiBold">react-native-reanimated</ThemedText>{' '}
          library to create a waving hand animation.
        </ThemedText>
        {Platform.select({
          ios: (
            <ThemedText>
              The <ThemedText type="defaultSemiBold">components/ParallaxScrollView.tsx</ThemedText>{' '}
              component provides a parallax effect for the header image.
            </ThemedText>
          ),
        })}
      </Collapsible>
      <Collapsible title="Premium Features">
        <ThemedText>
          Tap to explore premium features {isSubscribed ? '(Subscribed)' : '(Upgrade Required)'}
        </ThemedText>
        {!isSubscribed && (
          <TouchableOpacity onPress={handlePremiumFeature} style={styles.upgradeButton}>
            <ThemedText type="defaultSemiBold">Upgrade Now</ThemedText>
          </TouchableOpacity>
        )}
      </Collapsible>
    </ParallaxScrollView>
  );
}

const styles = StyleSheet.create({
  headerImage: {
    color: '#808080',
    bottom: -90,
    left: -35,
    position: 'absolute',
  },
  titleContainer: {
    flexDirection: 'row',
    gap: 8,
  },
  upgradeButton: {
    marginTop: 12,
    padding: 8,
    backgroundColor: Colors.light.tint,
    borderRadius: 8,
    alignItems: 'center',
  },
});

```

# app/(tabs)/index.tsx

```tsx
import { Image, StyleSheet, Platform, View } from 'react-native';
import { TouchableOpacity } from 'react-native-gesture-handler';

import { HelloWave } from '@/components/HelloWave';
import ParallaxScrollView from '@/components/ParallaxScrollView';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { useSuperwall } from '@/hooks/useSuperwall';
import { useOnboarding } from '@/contexts/OnboardingContext';
import { useRouter } from 'expo-router';
import { SUPERWALL_TRIGGERS } from '@/config/superwall';

export default function HomeScreen() {
  const { setIsOnboarded } = useOnboarding();
  const { showPaywall } = useSuperwall();
  const router = useRouter();

  const handleRestartOnboarding = async () => {
    await setIsOnboarded(false);
    router.push('/onboarding');
  };

  const handleShowPaywall = () => {
    showPaywall(SUPERWALL_TRIGGERS.ONBOARDING);
  };

  return (
    <ParallaxScrollView
      headerBackgroundColor={{ light: '#A1CEDC', dark: '#1D3D47' }}
      headerImage={
        <Image
          source={require('@/assets/images/partial-react-logo.png')}
          style={styles.reactLogo}
        />
      }>
      <ThemedView style={styles.titleContainer}>
        <ThemedText type="title">Welcome!</ThemedText>
        <HelloWave />
      </ThemedView>
      <ThemedView style={styles.stepContainer}>
        <ThemedText type="subtitle">Step 1: Try it</ThemedText>
        <ThemedText>
          Edit <ThemedText type="defaultSemiBold">app/(tabs)/index.tsx</ThemedText> to see changes.
          Press{' '}
          <ThemedText type="defaultSemiBold">
            {Platform.select({
              ios: 'cmd + d',
              android: 'cmd + m',
              web: 'F12'
            })}
          </ThemedText>{' '}
          to open developer tools.
        </ThemedText>
      </ThemedView>
      <ThemedView style={styles.stepContainer}>
        <ThemedText type="subtitle">Step 2: Explore</ThemedText>
        <ThemedText>
          Tap the Explore tab to learn more about what's included in this starter app.
        </ThemedText>
      </ThemedView>
      <ThemedView style={styles.stepContainer}>
        <ThemedText type="subtitle">Step 3: Get a fresh start</ThemedText>
        <ThemedText>
          When you're ready, run{' '}
          <ThemedText type="defaultSemiBold">npm run reset-project</ThemedText> to get a fresh{' '}
          <ThemedText type="defaultSemiBold">app</ThemedText> directory. This will move the current{' '}
          <ThemedText type="defaultSemiBold">app</ThemedText> to{' '}
          <ThemedText type="defaultSemiBold">app-example</ThemedText>.
        </ThemedText>
      </ThemedView>
      
      <View style={styles.buttonContainer}>
        <TouchableOpacity style={styles.button} onPress={handleShowPaywall}>
          <ThemedText type="defaultSemiBold" style={styles.buttonText}>
            Show Paywall
          </ThemedText>
        </TouchableOpacity>

        <TouchableOpacity style={[styles.button, styles.secondaryButton]} onPress={handleRestartOnboarding}>
          <ThemedText type="defaultSemiBold" style={styles.secondaryButtonText}>
            Restart Onboarding
          </ThemedText>
        </TouchableOpacity>
      </View>
    </ParallaxScrollView>
  );
}

const styles = StyleSheet.create({
  titleContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 8,
  },
  stepContainer: {
    gap: 8,
    marginBottom: 8,
  },
  reactLogo: {
    height: 178,
    width: 290,
    bottom: 0,
    left: 0,
    position: 'absolute',
  },
  buttonContainer: {
    marginTop: 24,
    gap: 12,
  },
  button: {
    padding: 16,
    borderRadius: 12,
    alignItems: 'center',
    backgroundColor: '#0A7EA4',
  },
  buttonText: {
    color: 'white',
  },
  secondaryButton: {
    backgroundColor: '#0A7EA420',
  },
  secondaryButtonText: {
    color: '#0A7EA4',
  },
});

```

# app/+not-found.tsx

```tsx
import { Link, Stack } from 'expo-router';
import { StyleSheet } from 'react-native';

import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';

export default function NotFoundScreen() {
  return (
    <>
      <Stack.Screen options={{ title: 'Oops!' }} />
      <ThemedView style={styles.container}>
        <ThemedText type="title">This screen doesn't exist.</ThemedText>
        <Link href="/" style={styles.link}>
          <ThemedText type="link">Go to home screen!</ThemedText>
        </Link>
      </ThemedView>
    </>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  link: {
    marginTop: 15,
    paddingVertical: 15,
  },
});

```

# app/onboarding/_layout.tsx

```tsx
import { Stack } from 'expo-router';
import { SafeAreaProvider } from 'react-native-safe-area-context';

export default function Layout() {
  return (
    <SafeAreaProvider>
      <Stack
        screenOptions={{
          headerShown: false,
          animation: 'slide_from_right',
          gestureEnabled: false,
          contentStyle: {
            backgroundColor: 'transparent',
          },
        }}
      />
    </SafeAreaProvider>
  );
} 
```

# app/onboarding/features.tsx

```tsx
import { StyleSheet, View, ScrollView } from 'react-native';
import { useRouter } from 'expo-router';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { SafeAreaView } from 'react-native-safe-area-context';
import { MaterialCommunityIcons } from '@expo/vector-icons';
import type { MaterialCommunityIcons as IconType } from '@expo/vector-icons';

export default function FeaturesScreen() {
  const router = useRouter();

  const handleNext = () => {
    router.push('/onboarding/final');
  };

  return (
    <ThemedView style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <ScrollView 
          style={styles.scroll} 
          contentContainerStyle={styles.scrollContent}
          showsVerticalScrollIndicator={false}
        >
          <View style={styles.header}>
            <MaterialCommunityIcons name="check-decagram" size={48} color="#0A7EA4" />
            <ThemedText type="title" style={styles.title}>
              Ready to Use
            </ThemedText>
          </View>

          <View style={styles.features}>
            <Feature
              icon="cart-variant"
              title="In-App Purchases"
              description="Superwall integration for subscriptions and one-time purchases"
            />
            <Feature
              icon="navigation"
              title="Modern Navigation"
              description="File-based routing with Expo Router for a great UX"
            />
            <Feature
              icon="theme-light-dark"
              title="Theming System"
              description="Beautiful dark and light mode support out of the box"
            />
          </View>
        </ScrollView>

        <View style={styles.buttonContainer}>
          <TouchableOpacity style={styles.button} onPress={handleNext}>
            <ThemedText type="defaultSemiBold" style={styles.buttonText}>
              Almost There
            </ThemedText>
          </TouchableOpacity>
        </View>
      </SafeAreaView>
    </ThemedView>
  );
}

function Feature({ icon, title, description }: { 
  icon: keyof typeof IconType.glyphMap;
  title: string;
  description: string;
}) {
  return (
    <View style={styles.feature}>
      <View style={styles.featureHeader}>
        <View style={styles.iconContainer}>
          <MaterialCommunityIcons name={icon} size={24} color="#0A7EA4" />
        </View>
        <View style={styles.featureText}>
          <ThemedText type="defaultSemiBold" style={styles.featureTitle}>
            {title}
          </ThemedText>
          <ThemedText style={styles.featureDescription}>{description}</ThemedText>
        </View>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  safeArea: {
    flex: 1,
  },
  scroll: {
    flex: 1,
  },
  scrollContent: {
    paddingHorizontal: 24,
    paddingTop: 24,
    paddingBottom: 16,
    gap: 24,
  },
  header: {
    alignItems: 'center',
    gap: 16,
  },
  title: {
    fontSize: 32,
    textAlign: 'center',
  },
  features: {
    gap: 16,
  },
  feature: {
    backgroundColor: '#0A7EA410',
    padding: 16,
    borderRadius: 12,
  },
  featureHeader: {
    flexDirection: 'row',
    gap: 16,
    alignItems: 'flex-start',
  },
  iconContainer: {
    width: 40,
    height: 40,
    borderRadius: 20,
    backgroundColor: '#0A7EA420',
    alignItems: 'center',
    justifyContent: 'center',
  },
  featureText: {
    flex: 1,
    gap: 4,
  },
  featureTitle: {
    fontSize: 17,
  },
  featureDescription: {
    fontSize: 15,
    opacity: 0.7,
    lineHeight: 20,
  },
  buttonContainer: {
    paddingHorizontal: 24,
    paddingVertical: 16,
  },
  button: {
    backgroundColor: '#0A7EA4',
    padding: 20,
    borderRadius: 16,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
  },
}); 
```

# app/onboarding/final.tsx

```tsx
import { StyleSheet, View, ScrollView } from 'react-native';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { useSuperwall } from '@/hooks/useSuperwall';
import { useOnboarding } from '@/contexts/OnboardingContext';
import { SUPERWALL_TRIGGERS } from '@/config/superwall';
import { SafeAreaView } from 'react-native-safe-area-context';
import { MaterialCommunityIcons } from '@expo/vector-icons';
import type { MaterialCommunityIcons as IconType } from '@expo/vector-icons';

export default function FinalScreen() {
  const { showPaywall } = useSuperwall();
  const { setIsOnboarded } = useOnboarding();

  const handleGetStarted = async () => {
    try {
      await showPaywall(SUPERWALL_TRIGGERS.ONBOARDING);
      setIsOnboarded(true);
    } catch (error) {
      console.error('Failed to show paywall:', error);
    }
  };

  return (
    <ThemedView style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <ScrollView style={styles.scroll} contentContainerStyle={styles.scrollContent}>
          <View style={styles.header}>
            <MaterialCommunityIcons name="rocket-launch" size={48} color="#0A7EA4" />
            <ThemedText type="title" style={styles.title}>
              Start Building Today
            </ThemedText>
            <ThemedText style={styles.description}>
              You're all set to create your next great app. Get started now and save weeks of development time!
            </ThemedText>
          </View>

          <View style={styles.benefits}>
            <Benefit icon="lightning-bolt" text="Launch faster" />
            <Benefit icon="palette" text="Professional design" />
            <Benefit icon="cash-multiple" text="Ready for monetization" />
          </View>
        </ScrollView>

        <TouchableOpacity style={styles.button} onPress={handleGetStarted}>
          <ThemedText type="defaultSemiBold" style={styles.buttonText}>
            Get Started Now
          </ThemedText>
        </TouchableOpacity>
      </SafeAreaView>
    </ThemedView>
  );
}

function Benefit({ icon, text }: { icon: keyof typeof IconType.glyphMap; text: string }) {
  return (
    <View style={styles.benefitContainer}>
      <View style={styles.iconContainer}>
        <MaterialCommunityIcons name={icon} size={24} color="#0A7EA4" />
      </View>
      <ThemedText style={styles.benefitText}>{text}</ThemedText>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  safeArea: {
    flex: 1,
  },
  scroll: {
    flex: 1,
  },
  scrollContent: {
    paddingHorizontal: 24,
    paddingTop: 24,
    gap: 32,
  },
  header: {
    alignItems: 'center',
    gap: 16,
  },
  title: {
    fontSize: 32,
    textAlign: 'center',
  },
  description: {
    textAlign: 'center',
    fontSize: 18,
    lineHeight: 28,
    opacity: 0.7,
  },
  benefits: {
    gap: 16,
    paddingBottom: 24,
  },
  benefitContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 16,
    backgroundColor: '#0A7EA410',
    padding: 16,
    borderRadius: 12,
  },
  iconContainer: {
    width: 40,
    height: 40,
    borderRadius: 20,
    backgroundColor: '#0A7EA420',
    alignItems: 'center',
    justifyContent: 'center',
  },
  benefitText: {
    fontSize: 17,
  },
  button: {
    backgroundColor: '#0A7EA4',
    padding: 16,
    borderRadius: 12,
    alignItems: 'center',
    marginHorizontal: 24,
    marginBottom: 16,
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
  },
}); 
```

# app/onboarding/index.tsx

```tsx
import { StyleSheet, View } from 'react-native';
import { useRouter } from 'expo-router';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { SafeAreaView } from 'react-native-safe-area-context';
import { StatusBar } from 'expo-status-bar';
import { MaterialCommunityIcons } from '@expo/vector-icons';

export default function WelcomeScreen() {
  const router = useRouter();

  const handleNext = () => {
    router.push('/onboarding/problem');
  };

  return (
    <ThemedView style={styles.container}>
      <StatusBar style="auto" />
      <SafeAreaView style={styles.safeArea}>
        <View style={styles.content}>
          <View style={styles.main}>
            <MaterialCommunityIcons name="star" size={64} color="#0A7EA4" />
            <ThemedText type="title" style={styles.title}>
              Your App Name
            </ThemedText>
            <View style={styles.subtitleContainer}>
              <ThemedText style={styles.subtitle}>
                A short, compelling tagline that captures your app's value
              </ThemedText>
            </View>
          </View>

          <TouchableOpacity style={styles.button} onPress={handleNext}>
            <ThemedText type="defaultSemiBold" style={styles.buttonText}>
              Get Started
            </ThemedText>
          </TouchableOpacity>
        </View>
      </SafeAreaView>
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  safeArea: {
    flex: 1,
  },
  content: {
    flex: 1,
    paddingHorizontal: 24,
    justifyContent: 'space-between',
    paddingVertical: 24,
  },
  main: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    gap: 24,
  },
  title: {
    fontSize: 36,
    textAlign: 'center',
    paddingHorizontal: 16,
  },
  subtitleContainer: {
    paddingHorizontal: 32,
  },
  subtitle: {
    fontSize: 18,
    opacity: 0.7,
    textAlign: 'center',
    lineHeight: 24,
  },
  button: {
    backgroundColor: '#0A7EA4',
    padding: 20,
    borderRadius: 16,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
  },
}); 
```

# app/onboarding/problem.tsx

```tsx
import { StyleSheet, View, ScrollView } from 'react-native';
import { useRouter } from 'expo-router';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { SafeAreaView } from 'react-native-safe-area-context';
import { MaterialCommunityIcons } from '@expo/vector-icons';

export default function ProblemScreen() {
  const router = useRouter();

  const handleNext = () => {
    router.push('/onboarding/solution');
  };

  return (
    <ThemedView style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <ScrollView 
          style={styles.scroll} 
          contentContainerStyle={styles.scrollContent}
          showsVerticalScrollIndicator={false}
        >
          <View style={styles.header}>
            <ThemedText type="title" style={styles.title}>
              The Problem
            </ThemedText>
            <ThemedText style={styles.description}>
              Describe the main challenge or pain point your users face. Make it relatable and specific.
            </ThemedText>
          </View>

          <View style={styles.content}>
            <View style={styles.example}>
              <MaterialCommunityIcons name="alert-circle" size={32} color="#0A7EA4" />
              <ThemedText style={styles.exampleText}>
                "I struggle with X every day, and it costs me Y hours per week..."
              </ThemedText>
            </View>

            <View style={styles.points}>
              <View style={styles.point}>
                <MaterialCommunityIcons name="close" size={24} color="#E11D48" />
                <ThemedText style={styles.pointText}>
                  Current solutions are expensive and complex
                </ThemedText>
              </View>
              <View style={styles.point}>
                <MaterialCommunityIcons name="close" size={24} color="#E11D48" />
                <ThemedText style={styles.pointText}>
                  Users waste time on manual workarounds
                </ThemedText>
              </View>
              <View style={styles.point}>
                <MaterialCommunityIcons name="close" size={24} color="#E11D48" />
                <ThemedText style={styles.pointText}>
                  Existing tools lack key features
                </ThemedText>
              </View>
            </View>
          </View>
        </ScrollView>

        <View style={styles.buttonContainer}>
          <TouchableOpacity style={styles.button} onPress={handleNext}>
            <ThemedText type="defaultSemiBold" style={styles.buttonText}>
              See the Solution
            </ThemedText>
          </TouchableOpacity>
        </View>
      </SafeAreaView>
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  safeArea: {
    flex: 1,
  },
  scroll: {
    flex: 1,
  },
  scrollContent: {
    flexGrow: 1,
    paddingHorizontal: 24,
    paddingTop: 24,
  },
  header: {
    gap: 8,
    marginBottom: 32,
  },
  title: {
    fontSize: 36,
  },
  description: {
    fontSize: 16,
    opacity: 0.7,
    lineHeight: 22,
  },
  content: {
    gap: 24,
  },
  example: {
    backgroundColor: '#0A7EA410',
    padding: 16,
    borderRadius: 16,
    flexDirection: 'row',
    gap: 12,
    alignItems: 'center',
  },
  exampleText: {
    flex: 1,
    fontSize: 16,
    fontStyle: 'italic',
    lineHeight: 22,
  },
  points: {
    gap: 12,
  },
  point: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 12,
    backgroundColor: '#E11D4810',
    padding: 14,
    borderRadius: 12,
  },
  pointText: {
    flex: 1,
    fontSize: 15,
    lineHeight: 20,
  },
  buttonContainer: {
    paddingHorizontal: 24,
    paddingVertical: 12,
  },
  button: {
    backgroundColor: '#0A7EA4',
    padding: 16,
    borderRadius: 16,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 17,
  },
}); 
```

# app/onboarding/solution.tsx

```tsx
import { StyleSheet, View, ScrollView } from 'react-native';
import { useRouter } from 'expo-router';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { SafeAreaView } from 'react-native-safe-area-context';
import { MaterialCommunityIcons } from '@expo/vector-icons';

export default function SolutionScreen() {
  const router = useRouter();

  const handleNext = () => {
    router.push('/onboarding/features');
  };

  return (
    <ThemedView style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <ScrollView 
          style={styles.scroll} 
          contentContainerStyle={styles.scrollContent}
          showsVerticalScrollIndicator={false}
        >
          <View style={styles.header}>
            <MaterialCommunityIcons name="lightbulb-on" size={48} color="#0A7EA4" />
            <ThemedText type="title" style={styles.title}>
              Introducing a Better Way
            </ThemedText>
          </View>

          <View style={styles.mainFeature}>
            <ThemedText type="defaultSemiBold" style={styles.mainTitle}>
              Your App's Core Value
            </ThemedText>
            <ThemedText style={styles.mainDescription}>
              One clear, powerful sentence that explains exactly how you solve the user's problem.
            </ThemedText>
          </View>

          <View style={styles.benefits}>
            <View style={styles.benefit}>
              <MaterialCommunityIcons name="check-circle" size={24} color="#0A7EA4" />
              <ThemedText style={styles.benefitText}>
                Key benefit or feature that solves their pain
              </ThemedText>
            </View>
            <View style={styles.benefit}>
              <MaterialCommunityIcons name="check-circle" size={24} color="#0A7EA4" />
              <ThemedText style={styles.benefitText}>
                Another important advantage of your solution
              </ThemedText>
            </View>
            <View style={styles.benefit}>
              <MaterialCommunityIcons name="check-circle" size={24} color="#0A7EA4" />
              <ThemedText style={styles.benefitText}>
                A third compelling reason to use your app
              </ThemedText>
            </View>
          </View>
        </ScrollView>

        <View style={styles.buttonContainer}>
          <TouchableOpacity style={styles.button} onPress={handleNext}>
            <ThemedText type="defaultSemiBold" style={styles.buttonText}>
              Show Me How
            </ThemedText>
          </TouchableOpacity>
        </View>
      </SafeAreaView>
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  safeArea: {
    flex: 1,
  },
  scroll: {
    flex: 1,
  },
  scrollContent: {
    paddingHorizontal: 24,
    paddingTop: 24,
    paddingBottom: 16,
    gap: 24,
  },
  header: {
    alignItems: 'center',
    gap: 16,
  },
  title: {
    fontSize: 32,
    textAlign: 'center',
  },
  mainFeature: {
    backgroundColor: '#0A7EA410',
    padding: 24,
    borderRadius: 20,
    gap: 8,
  },
  mainTitle: {
    fontSize: 22,
    textAlign: 'center',
  },
  mainDescription: {
    fontSize: 16,
    opacity: 0.7,
    textAlign: 'center',
    lineHeight: 22,
  },
  benefits: {
    gap: 12,
  },
  benefit: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 12,
    backgroundColor: '#0A7EA408',
    padding: 16,
    borderRadius: 12,
  },
  benefitText: {
    flex: 1,
    fontSize: 16,
    lineHeight: 22,
  },
  buttonContainer: {
    paddingHorizontal: 24,
    paddingVertical: 16,
  },
  button: {
    backgroundColor: '#0A7EA4',
    padding: 20,
    borderRadius: 16,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
  },
}); 
```

# assets/fonts/SpaceMono-Regular.ttf

This is a binary file of the type: Binary

# assets/images/adaptive-icon.png

This is a binary file of the type: Image

# assets/images/favicon.png

This is a binary file of the type: Image

# assets/images/icon.png

This is a binary file of the type: Image

# assets/images/partial-react-logo.png

This is a binary file of the type: Image

# assets/images/react-logo.png

This is a binary file of the type: Image

# assets/images/react-logo@2x.png

This is a binary file of the type: Image

# assets/images/react-logo@3x.png

This is a binary file of the type: Image

# assets/images/splash-icon.png

This is a binary file of the type: Image

# components/__tests__/__snapshots__/ThemedText-test.tsx.snap

```snap
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`renders correctly 1`] = `
<Text
  style={
    [
      {
        "color": "#11181C",
      },
      {
        "fontSize": 16,
        "lineHeight": 24,
      },
      undefined,
      undefined,
      undefined,
      undefined,
      undefined,
    ]
  }
>
  Snapshot test!
</Text>
`;

```

# components/__tests__/ThemedText-test.tsx

```tsx
import * as React from 'react';
import renderer from 'react-test-renderer';

import { ThemedText } from '../ThemedText';

it(`renders correctly`, () => {
  const tree = renderer.create(<ThemedText>Snapshot test!</ThemedText>).toJSON();

  expect(tree).toMatchSnapshot();
});

```

# components/Collapsible.tsx

```tsx
import { PropsWithChildren, useState } from 'react';
import { StyleSheet, TouchableOpacity } from 'react-native';

import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { IconSymbol } from '@/components/ui/IconSymbol';
import { Colors } from '@/constants/Colors';
import { useColorScheme } from '@/hooks/useColorScheme';

export function Collapsible({ children, title }: PropsWithChildren & { title: string }) {
  const [isOpen, setIsOpen] = useState(false);
  const theme = useColorScheme() ?? 'light';

  return (
    <ThemedView>
      <TouchableOpacity
        style={styles.heading}
        onPress={() => setIsOpen((value) => !value)}
        activeOpacity={0.8}>
        <IconSymbol
          name="chevron.right"
          size={18}
          weight="medium"
          color={theme === 'light' ? Colors.light.icon : Colors.dark.icon}
          style={{ transform: [{ rotate: isOpen ? '90deg' : '0deg' }] }}
        />

        <ThemedText type="defaultSemiBold">{title}</ThemedText>
      </TouchableOpacity>
      {isOpen && <ThemedView style={styles.content}>{children}</ThemedView>}
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  heading: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 6,
  },
  content: {
    marginTop: 6,
    marginLeft: 24,
  },
});

```

# components/ExternalLink.tsx

```tsx
import { Link } from 'expo-router';
import { openBrowserAsync } from 'expo-web-browser';
import { type ComponentProps } from 'react';
import { Platform } from 'react-native';

type Props = Omit<ComponentProps<typeof Link>, 'href'> & { href: string };

export function ExternalLink({ href, ...rest }: Props) {
  return (
    <Link
      target="_blank"
      {...rest}
      href={href}
      onPress={async (event) => {
        if (Platform.OS !== 'web') {
          // Prevent the default behavior of linking to the default browser on native.
          event.preventDefault();
          // Open the link in an in-app browser.
          await openBrowserAsync(href);
        }
      }}
    />
  );
}

```

# components/HapticTab.tsx

```tsx
import { BottomTabBarButtonProps } from '@react-navigation/bottom-tabs';
import { PlatformPressable } from '@react-navigation/elements';
import * as Haptics from 'expo-haptics';

export function HapticTab(props: BottomTabBarButtonProps) {
  return (
    <PlatformPressable
      {...props}
      onPressIn={(ev) => {
        if (process.env.EXPO_OS === 'ios') {
          // Add a soft haptic feedback when pressing down on the tabs.
          Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);
        }
        props.onPressIn?.(ev);
      }}
    />
  );
}

```

# components/HelloWave.tsx

```tsx
import { useEffect } from 'react';
import { StyleSheet } from 'react-native';
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withTiming,
  withRepeat,
  withSequence,
} from 'react-native-reanimated';

import { ThemedText } from '@/components/ThemedText';

export function HelloWave() {
  const rotationAnimation = useSharedValue(0);

  useEffect(() => {
    rotationAnimation.value = withRepeat(
      withSequence(withTiming(25, { duration: 150 }), withTiming(0, { duration: 150 })),
      4 // Run the animation 4 times
    );
  }, []);

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ rotate: `${rotationAnimation.value}deg` }],
  }));

  return (
    <Animated.View style={animatedStyle}>
      <ThemedText style={styles.text}>👋</ThemedText>
    </Animated.View>
  );
}

const styles = StyleSheet.create({
  text: {
    fontSize: 28,
    lineHeight: 32,
    marginTop: -6,
  },
});

```

# components/ParallaxScrollView.tsx

```tsx
import type { PropsWithChildren, ReactElement } from 'react';
import { StyleSheet } from 'react-native';
import Animated, {
  interpolate,
  useAnimatedRef,
  useAnimatedStyle,
  useScrollViewOffset,
} from 'react-native-reanimated';

import { ThemedView } from '@/components/ThemedView';
import { useBottomTabOverflow } from '@/components/ui/TabBarBackground';
import { useColorScheme } from '@/hooks/useColorScheme';

const HEADER_HEIGHT = 250;

type Props = PropsWithChildren<{
  headerImage: ReactElement;
  headerBackgroundColor: { dark: string; light: string };
}>;

export default function ParallaxScrollView({
  children,
  headerImage,
  headerBackgroundColor,
}: Props) {
  const colorScheme = useColorScheme() ?? 'light';
  const scrollRef = useAnimatedRef<Animated.ScrollView>();
  const scrollOffset = useScrollViewOffset(scrollRef);
  const bottom = useBottomTabOverflow();
  const headerAnimatedStyle = useAnimatedStyle(() => {
    return {
      transform: [
        {
          translateY: interpolate(
            scrollOffset.value,
            [-HEADER_HEIGHT, 0, HEADER_HEIGHT],
            [-HEADER_HEIGHT / 2, 0, HEADER_HEIGHT * 0.75]
          ),
        },
        {
          scale: interpolate(scrollOffset.value, [-HEADER_HEIGHT, 0, HEADER_HEIGHT], [2, 1, 1]),
        },
      ],
    };
  });

  return (
    <ThemedView style={styles.container}>
      <Animated.ScrollView
        ref={scrollRef}
        scrollEventThrottle={16}
        scrollIndicatorInsets={{ bottom }}
        contentContainerStyle={{ paddingBottom: bottom }}>
        <Animated.View
          style={[
            styles.header,
            { backgroundColor: headerBackgroundColor[colorScheme] },
            headerAnimatedStyle,
          ]}>
          {headerImage}
        </Animated.View>
        <ThemedView style={styles.content}>{children}</ThemedView>
      </Animated.ScrollView>
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  header: {
    height: HEADER_HEIGHT,
    overflow: 'hidden',
  },
  content: {
    flex: 1,
    padding: 32,
    gap: 16,
    overflow: 'hidden',
  },
});

```

# components/PaywallButton.tsx

```tsx
import { StyleSheet } from 'react-native';
import { TouchableOpacity } from 'react-native-gesture-handler';
import { ThemedText } from './ThemedText';
import { useSuperwall } from '@/hooks/useSuperwall';
import { SUPERWALL_TRIGGERS } from '@/config/superwall';

export function PaywallButton() {
  const { showPaywall } = useSuperwall();

  const handlePress = () => {
    showPaywall(SUPERWALL_TRIGGERS.ONBOARDING);
  };

  return (
    <TouchableOpacity style={styles.button} onPress={handlePress}>
      <ThemedText type="defaultSemiBold">Show Paywall</ThemedText>
    </TouchableOpacity>
  );
}

const styles = StyleSheet.create({
  button: {
    padding: 16,
    borderRadius: 8,
    backgroundColor: '#0a7ea4',
    alignItems: 'center',
  },
});
```

# components/ThemedText.tsx

```tsx
import { Text, type TextProps, StyleSheet } from 'react-native';

import { useThemeColor } from '@/hooks/useThemeColor';

export type ThemedTextProps = TextProps & {
  lightColor?: string;
  darkColor?: string;
  type?: 'default' | 'title' | 'defaultSemiBold' | 'subtitle' | 'link';
};

export function ThemedText({
  style,
  lightColor,
  darkColor,
  type = 'default',
  ...rest
}: ThemedTextProps) {
  const color = useThemeColor({ light: lightColor, dark: darkColor }, 'text');

  return (
    <Text
      style={[
        { color },
        type === 'default' ? styles.default : undefined,
        type === 'title' ? styles.title : undefined,
        type === 'defaultSemiBold' ? styles.defaultSemiBold : undefined,
        type === 'subtitle' ? styles.subtitle : undefined,
        type === 'link' ? styles.link : undefined,
        style,
      ]}
      {...rest}
    />
  );
}

const styles = StyleSheet.create({
  default: {
    fontSize: 16,
    lineHeight: 24,
  },
  defaultSemiBold: {
    fontSize: 16,
    lineHeight: 24,
    fontWeight: '600',
  },
  title: {
    fontSize: 32,
    fontWeight: 'bold',
    lineHeight: 32,
  },
  subtitle: {
    fontSize: 20,
    fontWeight: 'bold',
  },
  link: {
    lineHeight: 30,
    fontSize: 16,
    color: '#0a7ea4',
  },
});

```

# components/ThemedView.tsx

```tsx
import { View, type ViewProps } from 'react-native';

import { useThemeColor } from '@/hooks/useThemeColor';

export type ThemedViewProps = ViewProps & {
  lightColor?: string;
  darkColor?: string;
};

export function ThemedView({ style, lightColor, darkColor, ...otherProps }: ThemedViewProps) {
  const backgroundColor = useThemeColor({ light: lightColor, dark: darkColor }, 'background');

  return <View style={[{ backgroundColor }, style]} {...otherProps} />;
}

```

# components/ui/IconSymbol.ios.tsx

```tsx
import { SymbolView, SymbolViewProps, SymbolWeight } from 'expo-symbols';
import { StyleProp, ViewStyle } from 'react-native';

export function IconSymbol({
  name,
  size = 24,
  color,
  style,
  weight = 'regular',
}: {
  name: SymbolViewProps['name'];
  size?: number;
  color: string;
  style?: StyleProp<ViewStyle>;
  weight?: SymbolWeight;
}) {
  return (
    <SymbolView
      weight={weight}
      tintColor={color}
      resizeMode="scaleAspectFit"
      name={name}
      style={[
        {
          width: size,
          height: size,
        },
        style,
      ]}
    />
  );
}

```

# components/ui/IconSymbol.tsx

```tsx
// This file is a fallback for using MaterialIcons on Android and web.

import MaterialIcons from '@expo/vector-icons/MaterialIcons';
import { SymbolWeight } from 'expo-symbols';
import React from 'react';
import { OpaqueColorValue, StyleProp, ViewStyle } from 'react-native';

// Add your SFSymbol to MaterialIcons mappings here.
const MAPPING = {
  // See MaterialIcons here: https://icons.expo.fyi
  // See SF Symbols in the SF Symbols app on Mac.
  'house.fill': 'home',
  'paperplane.fill': 'send',
  'chevron.left.forwardslash.chevron.right': 'code',
  'chevron.right': 'chevron-right',
} as Partial<
  Record<
    import('expo-symbols').SymbolViewProps['name'],
    React.ComponentProps<typeof MaterialIcons>['name']
  >
>;

export type IconSymbolName = keyof typeof MAPPING;

/**
 * An icon component that uses native SFSymbols on iOS, and MaterialIcons on Android and web. This ensures a consistent look across platforms, and optimal resource usage.
 *
 * Icon `name`s are based on SFSymbols and require manual mapping to MaterialIcons.
 */
export function IconSymbol({
  name,
  size = 24,
  color,
  style,
}: {
  name: IconSymbolName;
  size?: number;
  color: string | OpaqueColorValue;
  style?: StyleProp<ViewStyle>;
  weight?: SymbolWeight;
}) {
  return <MaterialIcons color={color} size={size} name={MAPPING[name]} style={style} />;
}

```

# components/ui/TabBarBackground.ios.tsx

```tsx
import { useBottomTabBarHeight } from '@react-navigation/bottom-tabs';
import { BlurView } from 'expo-blur';
import { StyleSheet } from 'react-native';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

export default function BlurTabBarBackground() {
  return (
    <BlurView
      // System chrome material automatically adapts to the system's theme
      // and matches the native tab bar appearance on iOS.
      tint="systemChromeMaterial"
      intensity={100}
      style={StyleSheet.absoluteFill}
    />
  );
}

export function useBottomTabOverflow() {
  const tabHeight = useBottomTabBarHeight();
  const { bottom } = useSafeAreaInsets();
  return tabHeight - bottom;
}

```

# components/ui/TabBarBackground.tsx

```tsx
// This is a shim for web and Android where the tab bar is generally opaque.
export default undefined;

export function useBottomTabOverflow() {
  return 0;
}

```

# config/superwall.ts

```ts
import { SuperwallOptions, LogLevel, LogScope } from '@superwall/react-native-superwall';

export const SUPERWALL_TRIGGERS = {
  ONBOARDING: 'campaign_trigger',
  FEATURE_UNLOCK: 'campaign_trigger',
  // Add more triggers as needed
} as const;

export const createSuperwallConfig = () => {
  const options = new SuperwallOptions();
  
  // Enable debug logging in development
  if (__DEV__) {
    options.logging.level = LogLevel.Debug;
    options.logging.scopes = [
      LogScope.PaywallPresentation,
      LogScope.PaywallTransactions,
      LogScope.Network,
    ];
  }

  return options;
}; 
```

# constants/Colors.ts

```ts
/**
 * Below are the colors that are used in the app. The colors are defined in the light and dark mode.
 * There are many other ways to style your app. For example, [Nativewind](https://www.nativewind.dev/), [Tamagui](https://tamagui.dev/), [unistyles](https://reactnativeunistyles.vercel.app), etc.
 */

const tintColorLight = '#0a7ea4';
const tintColorDark = '#fff';

export const Colors = {
  light: {
    text: '#11181C',
    background: '#fff',
    tint: tintColorLight,
    icon: '#687076',
    tabIconDefault: '#687076',
    tabIconSelected: tintColorLight,
  },
  dark: {
    text: '#ECEDEE',
    background: '#151718',
    tint: tintColorDark,
    icon: '#9BA1A6',
    tabIconDefault: '#9BA1A6',
    tabIconSelected: tintColorDark,
  },
};

```

# contexts/OnboardingContext.tsx

```tsx
import { createContext, useContext, useEffect, useState } from 'react';
import AsyncStorage from '@react-native-async-storage/async-storage';
import { useRouter, useSegments } from 'expo-router';

type OnboardingContextType = {
  isOnboarded: boolean;
  setIsOnboarded: (value: boolean) => void;
};

const OnboardingContext = createContext<OnboardingContextType | undefined>(undefined);

const STORAGE_KEY = 'hasCompletedOnboarding';

export function OnboardingProvider({ children }: { children: React.ReactNode }) {
  const [isOnboarded, setIsOnboarded] = useState<boolean | null>(null);
  const segments = useSegments();
  const router = useRouter();

  useEffect(() => {
    checkOnboardingStatus();
  }, []);

  useEffect(() => {
    if (isOnboarded === null) return;

    const inAuthGroup = segments[0] === 'onboarding';

    if (!isOnboarded && !inAuthGroup) {
      router.replace('/onboarding');
    } else if (isOnboarded && inAuthGroup) {
      router.replace('/');
    }
  }, [isOnboarded, segments]);

  const checkOnboardingStatus = async () => {
    try {
      const value = await AsyncStorage.getItem(STORAGE_KEY);
      setIsOnboarded(value === 'true');
    } catch (error) {
      console.error('Error checking onboarding status:', error);
      setIsOnboarded(false);
    }
  };

  const handleSetIsOnboarded = async (value: boolean) => {
    try {
      await AsyncStorage.setItem(STORAGE_KEY, String(value));
      setIsOnboarded(value);
    } catch (error) {
      console.error('Error setting onboarding status:', error);
    }
  };

  return (
    <OnboardingContext.Provider
      value={{
        isOnboarded: isOnboarded ?? false,
        setIsOnboarded: handleSetIsOnboarded,
      }}>
      {children}
    </OnboardingContext.Provider>
  );
}

export function useOnboarding() {
  const context = useContext(OnboardingContext);
  if (context === undefined) {
    throw new Error('useOnboarding must be used within an OnboardingProvider');
  }
  return context;
} 
```

# eas.json

```json
{
  "cli": {
    "version": ">= 15.0.10",
    "appVersionSource": "remote"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "ios": {
        "simulator": true
      },
      "env": {
        "EXPO_PUBLIC_SUPERWALL_API_KEY_IOS": "your_ios_api_key",
        "EXPO_PUBLIC_SUPERWALL_API_KEY_ANDROID": "your_android_key_here"
      }
    },
    "preview": {
      "distribution": "internal",
      "env": {
        "EXPO_PUBLIC_SUPERWALL_API_KEY_IOS": "your_ios_api_key",
        "EXPO_PUBLIC_SUPERWALL_API_KEY_ANDROID": "your_android_key_here"
      }
    },
    "production": {
      "autoIncrement": true,
      "env": {
        "EXPO_PUBLIC_SUPERWALL_API_KEY_IOS": "your_ios_api_key",
        "EXPO_PUBLIC_SUPERWALL_API_KEY_ANDROID": "your_android_key_here"
      }
    }
  },
  "submit": {
    "production": {}
  }
}

```

# hooks/useColorScheme.ts

```ts
export { useColorScheme } from 'react-native';

```

# hooks/useColorScheme.web.ts

```ts
import { useEffect, useState } from 'react';
import { useColorScheme as useRNColorScheme } from 'react-native';

/**
 * To support static rendering, this value needs to be re-calculated on the client side for web
 */
export function useColorScheme() {
  const [hasHydrated, setHasHydrated] = useState(false);

  useEffect(() => {
    setHasHydrated(true);
  }, []);

  const colorScheme = useRNColorScheme();

  if (hasHydrated) {
    return colorScheme;
  }

  return 'light';
}

```

# hooks/useSuperwall.ts

```ts
import { useEffect, useState } from 'react';
import { Platform } from 'react-native';
import { SubscriptionStatus } from '@superwall/react-native-superwall';
import { superwallService } from '@/services/superwall';

export function useSuperwall() {
  const [isSubscribed, setIsSubscribed] = useState(false);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    if (Platform.OS === 'web') {
      setIsLoading(false);
      return;
    }

    superwallService.initialize();
    checkSubscription();
  }, []);

  const checkSubscription = async () => {
    try {
      const status = await superwallService.getSubscriptionStatus();
      setIsSubscribed(status === SubscriptionStatus.ACTIVE);
    } catch (error) {
      console.error('[Superwall] Hook subscription check failed:', error);
    } finally {
      setIsLoading(false);
    }
  };

  const showPaywall = async (triggerId: string) => {
    if (isLoading || Platform.OS === 'web') return;
    
    try {
      await superwallService.presentPaywall(triggerId);
      // Refresh subscription status after paywall interaction
      await checkSubscription();
    } catch (error) {
      console.error('[Superwall] Hook failed to show paywall:', error);
    }
  };

  return {
    isSubscribed,
    isLoading,
    showPaywall,
    checkSubscription,
  };
} 
```

# hooks/useThemeColor.ts

```ts
/**
 * Learn more about light and dark modes:
 * https://docs.expo.dev/guides/color-schemes/
 */

import { Colors } from '@/constants/Colors';
import { useColorScheme } from '@/hooks/useColorScheme';

export function useThemeColor(
  props: { light?: string; dark?: string },
  colorName: keyof typeof Colors.light & keyof typeof Colors.dark
) {
  const theme = useColorScheme() ?? 'light';
  const colorFromProps = props[theme];

  if (colorFromProps) {
    return colorFromProps;
  } else {
    return Colors[theme][colorName];
  }
}

```

# ios/.gitignore

```
# OSX
#
.DS_Store

# Xcode
#
build/
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata
*.xccheckout
*.moved-aside
DerivedData
*.hmap
*.ipa
*.xcuserstate
project.xcworkspace
.xcode.env.local

# Bundle artifacts
*.jsbundle

# CocoaPods
/Pods/

```

# ios/expoappboilerplate.xcodeproj/project.pbxproj

```pbxproj
// !$*UTF8*$!
{
	archiveVersion = 1;
	classes = {
	};
	objectVersion = 46;
	objects = {

/* Begin PBXBuildFile section */
		13B07FBC1A68108700A75B9A /* AppDelegate.mm in Sources */ = {isa = PBXBuildFile; fileRef = 13B07FB01A68108700A75B9A /* AppDelegate.mm */; };
		13B07FBF1A68108700A75B9A /* Images.xcassets in Resources */ = {isa = PBXBuildFile; fileRef = 13B07FB51A68108700A75B9A /* Images.xcassets */; };
		13B07FC11A68108700A75B9A /* main.m in Sources */ = {isa = PBXBuildFile; fileRef = 13B07FB71A68108700A75B9A /* main.m */; };
		190D622815B7446E85DE663F /* noop-file.swift in Sources */ = {isa = PBXBuildFile; fileRef = 5A4ECB565DE44967A4A9E63A /* noop-file.swift */; };
		3E461D99554A48A4959DE609 /* SplashScreen.storyboard in Resources */ = {isa = PBXBuildFile; fileRef = AA286B85B6C04FC6940260E9 /* SplashScreen.storyboard */; };
		64A27CBA0C15D3668487CE31 /* PrivacyInfo.xcprivacy in Resources */ = {isa = PBXBuildFile; fileRef = 25D8759A3F37E8B85A1B85A4 /* PrivacyInfo.xcprivacy */; };
		96905EF65AED1B983A6B3ABC /* libPods-expoappboilerplate.a in Frameworks */ = {isa = PBXBuildFile; fileRef = 58EEBF8E8E6FB1BC6CAF49B5 /* libPods-expoappboilerplate.a */; };
		B18059E884C0ABDD17F3DC3D /* ExpoModulesProvider.swift in Sources */ = {isa = PBXBuildFile; fileRef = FAC715A2D49A985799AEE119 /* ExpoModulesProvider.swift */; };
		BB2F792D24A3F905000567C9 /* Expo.plist in Resources */ = {isa = PBXBuildFile; fileRef = BB2F792C24A3F905000567C9 /* Expo.plist */; };
/* End PBXBuildFile section */

/* Begin PBXFileReference section */
		13B07F961A680F5B00A75B9A /* expoappboilerplate.app */ = {isa = PBXFileReference; explicitFileType = wrapper.application; includeInIndex = 0; path = expoappboilerplate.app; sourceTree = BUILT_PRODUCTS_DIR; };
		13B07FAF1A68108700A75B9A /* AppDelegate.h */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = sourcecode.c.h; name = AppDelegate.h; path = expoappboilerplate/AppDelegate.h; sourceTree = "<group>"; };
		13B07FB01A68108700A75B9A /* AppDelegate.mm */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = sourcecode.cpp.objcpp; name = AppDelegate.mm; path = expoappboilerplate/AppDelegate.mm; sourceTree = "<group>"; };
		13B07FB51A68108700A75B9A /* Images.xcassets */ = {isa = PBXFileReference; lastKnownFileType = folder.assetcatalog; name = Images.xcassets; path = expoappboilerplate/Images.xcassets; sourceTree = "<group>"; };
		13B07FB61A68108700A75B9A /* Info.plist */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = text.plist.xml; name = Info.plist; path = expoappboilerplate/Info.plist; sourceTree = "<group>"; };
		13B07FB71A68108700A75B9A /* main.m */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = sourcecode.c.objc; name = main.m; path = expoappboilerplate/main.m; sourceTree = "<group>"; };
		25D8759A3F37E8B85A1B85A4 /* PrivacyInfo.xcprivacy */ = {isa = PBXFileReference; includeInIndex = 1; name = PrivacyInfo.xcprivacy; path = expoappboilerplate/PrivacyInfo.xcprivacy; sourceTree = "<group>"; };
		58EEBF8E8E6FB1BC6CAF49B5 /* libPods-expoappboilerplate.a */ = {isa = PBXFileReference; explicitFileType = archive.ar; includeInIndex = 0; path = "libPods-expoappboilerplate.a"; sourceTree = BUILT_PRODUCTS_DIR; };
		5A4ECB565DE44967A4A9E63A /* noop-file.swift */ = {isa = PBXFileReference; explicitFileType = undefined; fileEncoding = 4; includeInIndex = 0; lastKnownFileType = sourcecode.swift; name = "noop-file.swift"; path = "expoappboilerplate/noop-file.swift"; sourceTree = "<group>"; };
		6C2E3173556A471DD304B334 /* Pods-expoappboilerplate.debug.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-expoappboilerplate.debug.xcconfig"; path = "Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate.debug.xcconfig"; sourceTree = "<group>"; };
		7A4D352CD337FB3A3BF06240 /* Pods-expoappboilerplate.release.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-expoappboilerplate.release.xcconfig"; path = "Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate.release.xcconfig"; sourceTree = "<group>"; };
		98CC00954968407EA3D35E84 /* expoappboilerplate-Bridging-Header.h */ = {isa = PBXFileReference; explicitFileType = undefined; fileEncoding = 4; includeInIndex = 0; lastKnownFileType = sourcecode.c.h; name = "expoappboilerplate-Bridging-Header.h"; path = "expoappboilerplate/expoappboilerplate-Bridging-Header.h"; sourceTree = "<group>"; };
		AA286B85B6C04FC6940260E9 /* SplashScreen.storyboard */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = file.storyboard; name = SplashScreen.storyboard; path = expoappboilerplate/SplashScreen.storyboard; sourceTree = "<group>"; };
		BB2F792C24A3F905000567C9 /* Expo.plist */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = text.plist.xml; path = Expo.plist; sourceTree = "<group>"; };
		ED297162215061F000B7C4FE /* JavaScriptCore.framework */ = {isa = PBXFileReference; lastKnownFileType = wrapper.framework; name = JavaScriptCore.framework; path = System/Library/Frameworks/JavaScriptCore.framework; sourceTree = SDKROOT; };
		FAC715A2D49A985799AEE119 /* ExpoModulesProvider.swift */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = sourcecode.swift; name = ExpoModulesProvider.swift; path = "Pods/Target Support Files/Pods-expoappboilerplate/ExpoModulesProvider.swift"; sourceTree = "<group>"; };
/* End PBXFileReference section */

/* Begin PBXFrameworksBuildPhase section */
		13B07F8C1A680F5B00A75B9A /* Frameworks */ = {
			isa = PBXFrameworksBuildPhase;
			buildActionMask = 2147483647;
			files = (
				96905EF65AED1B983A6B3ABC /* libPods-expoappboilerplate.a in Frameworks */,
			);
			runOnlyForDeploymentPostprocessing = 0;
		};
/* End PBXFrameworksBuildPhase section */

/* Begin PBXGroup section */
		13B07FAE1A68108700A75B9A /* expoappboilerplate */ = {
			isa = PBXGroup;
			children = (
				BB2F792B24A3F905000567C9 /* Supporting */,
				13B07FAF1A68108700A75B9A /* AppDelegate.h */,
				13B07FB01A68108700A75B9A /* AppDelegate.mm */,
				13B07FB51A68108700A75B9A /* Images.xcassets */,
				13B07FB61A68108700A75B9A /* Info.plist */,
				13B07FB71A68108700A75B9A /* main.m */,
				AA286B85B6C04FC6940260E9 /* SplashScreen.storyboard */,
				5A4ECB565DE44967A4A9E63A /* noop-file.swift */,
				98CC00954968407EA3D35E84 /* expoappboilerplate-Bridging-Header.h */,
				25D8759A3F37E8B85A1B85A4 /* PrivacyInfo.xcprivacy */,
			);
			name = expoappboilerplate;
			sourceTree = "<group>";
		};
		2D16E6871FA4F8E400B85C8A /* Frameworks */ = {
			isa = PBXGroup;
			children = (
				ED297162215061F000B7C4FE /* JavaScriptCore.framework */,
				58EEBF8E8E6FB1BC6CAF49B5 /* libPods-expoappboilerplate.a */,
			);
			name = Frameworks;
			sourceTree = "<group>";
		};
		832341AE1AAA6A7D00B99B32 /* Libraries */ = {
			isa = PBXGroup;
			children = (
			);
			name = Libraries;
			sourceTree = "<group>";
		};
		83CBB9F61A601CBA00E9B192 = {
			isa = PBXGroup;
			children = (
				13B07FAE1A68108700A75B9A /* expoappboilerplate */,
				832341AE1AAA6A7D00B99B32 /* Libraries */,
				83CBBA001A601CBA00E9B192 /* Products */,
				2D16E6871FA4F8E400B85C8A /* Frameworks */,
				D65327D7A22EEC0BE12398D9 /* Pods */,
				D7E4C46ADA2E9064B798F356 /* ExpoModulesProviders */,
			);
			indentWidth = 2;
			sourceTree = "<group>";
			tabWidth = 2;
			usesTabs = 0;
		};
		83CBBA001A601CBA00E9B192 /* Products */ = {
			isa = PBXGroup;
			children = (
				13B07F961A680F5B00A75B9A /* expoappboilerplate.app */,
			);
			name = Products;
			sourceTree = "<group>";
		};
		92DBD88DE9BF7D494EA9DA96 /* expoappboilerplate */ = {
			isa = PBXGroup;
			children = (
				FAC715A2D49A985799AEE119 /* ExpoModulesProvider.swift */,
			);
			name = expoappboilerplate;
			sourceTree = "<group>";
		};
		BB2F792B24A3F905000567C9 /* Supporting */ = {
			isa = PBXGroup;
			children = (
				BB2F792C24A3F905000567C9 /* Expo.plist */,
			);
			name = Supporting;
			path = expoappboilerplate/Supporting;
			sourceTree = "<group>";
		};
		D65327D7A22EEC0BE12398D9 /* Pods */ = {
			isa = PBXGroup;
			children = (
				6C2E3173556A471DD304B334 /* Pods-expoappboilerplate.debug.xcconfig */,
				7A4D352CD337FB3A3BF06240 /* Pods-expoappboilerplate.release.xcconfig */,
			);
			path = Pods;
			sourceTree = "<group>";
		};
		D7E4C46ADA2E9064B798F356 /* ExpoModulesProviders */ = {
			isa = PBXGroup;
			children = (
				92DBD88DE9BF7D494EA9DA96 /* expoappboilerplate */,
			);
			name = ExpoModulesProviders;
			sourceTree = "<group>";
		};
/* End PBXGroup section */

/* Begin PBXNativeTarget section */
		13B07F861A680F5B00A75B9A /* expoappboilerplate */ = {
			isa = PBXNativeTarget;
			buildConfigurationList = 13B07F931A680F5B00A75B9A /* Build configuration list for PBXNativeTarget "expoappboilerplate" */;
			buildPhases = (
				08A4A3CD28434E44B6B9DE2E /* [CP] Check Pods Manifest.lock */,
				96E2F22856A27C2ACE5CE685 /* [Expo] Configure project */,
				13B07F871A680F5B00A75B9A /* Sources */,
				13B07F8C1A680F5B00A75B9A /* Frameworks */,
				13B07F8E1A680F5B00A75B9A /* Resources */,
				00DD1BFF1BD5951E006B06BC /* Bundle React Native code and images */,
				800E24972A6A228C8D4807E9 /* [CP] Copy Pods Resources */,
				68ACA832FEA019FF7556258A /* [CP] Embed Pods Frameworks */,
			);
			buildRules = (
			);
			dependencies = (
			);
			name = expoappboilerplate;
			productName = expoappboilerplate;
			productReference = 13B07F961A680F5B00A75B9A /* expoappboilerplate.app */;
			productType = "com.apple.product-type.application";
		};
/* End PBXNativeTarget section */

/* Begin PBXProject section */
		83CBB9F71A601CBA00E9B192 /* Project object */ = {
			isa = PBXProject;
			attributes = {
				LastUpgradeCheck = 1130;
				TargetAttributes = {
					13B07F861A680F5B00A75B9A = {
						LastSwiftMigration = 1250;
					};
				};
			};
			buildConfigurationList = 83CBB9FA1A601CBA00E9B192 /* Build configuration list for PBXProject "expoappboilerplate" */;
			compatibilityVersion = "Xcode 3.2";
			developmentRegion = en;
			hasScannedForEncodings = 0;
			knownRegions = (
				en,
				Base,
			);
			mainGroup = 83CBB9F61A601CBA00E9B192;
			productRefGroup = 83CBBA001A601CBA00E9B192 /* Products */;
			projectDirPath = "";
			projectRoot = "";
			targets = (
				13B07F861A680F5B00A75B9A /* expoappboilerplate */,
			);
		};
/* End PBXProject section */

/* Begin PBXResourcesBuildPhase section */
		13B07F8E1A680F5B00A75B9A /* Resources */ = {
			isa = PBXResourcesBuildPhase;
			buildActionMask = 2147483647;
			files = (
				BB2F792D24A3F905000567C9 /* Expo.plist in Resources */,
				13B07FBF1A68108700A75B9A /* Images.xcassets in Resources */,
				3E461D99554A48A4959DE609 /* SplashScreen.storyboard in Resources */,
				64A27CBA0C15D3668487CE31 /* PrivacyInfo.xcprivacy in Resources */,
			);
			runOnlyForDeploymentPostprocessing = 0;
		};
/* End PBXResourcesBuildPhase section */

/* Begin PBXShellScriptBuildPhase section */
		00DD1BFF1BD5951E006B06BC /* Bundle React Native code and images */ = {
			isa = PBXShellScriptBuildPhase;
			alwaysOutOfDate = 1;
			buildActionMask = 2147483647;
			files = (
			);
			inputPaths = (
			);
			name = "Bundle React Native code and images";
			outputPaths = (
			);
			runOnlyForDeploymentPostprocessing = 0;
			shellPath = /bin/sh;
			shellScript = "if [[ -f \"$PODS_ROOT/../.xcode.env\" ]]; then\n  source \"$PODS_ROOT/../.xcode.env\"\nfi\nif [[ -f \"$PODS_ROOT/../.xcode.env.local\" ]]; then\n  source \"$PODS_ROOT/../.xcode.env.local\"\nfi\n\n# The project root by default is one level up from the ios directory\nexport PROJECT_ROOT=\"$PROJECT_DIR\"/..\n\nif [[ \"$CONFIGURATION\" = *Debug* ]]; then\n  export SKIP_BUNDLING=1\nfi\nif [[ -z \"$ENTRY_FILE\" ]]; then\n  # Set the entry JS file using the bundler's entry resolution.\n  export ENTRY_FILE=\"$(\"$NODE_BINARY\" -e \"require('expo/scripts/resolveAppEntry')\" \"$PROJECT_ROOT\" ios absolute | tail -n 1)\"\nfi\n\nif [[ -z \"$CLI_PATH\" ]]; then\n  # Use Expo CLI\n  export CLI_PATH=\"$(\"$NODE_BINARY\" --print \"require.resolve('@expo/cli', { paths: [require.resolve('expo/package.json')] })\")\"\nfi\nif [[ -z \"$BUNDLE_COMMAND\" ]]; then\n  # Default Expo CLI command for bundling\n  export BUNDLE_COMMAND=\"export:embed\"\nfi\n\n# Source .xcode.env.updates if it exists to allow\n# SKIP_BUNDLING to be unset if needed\nif [[ -f \"$PODS_ROOT/../.xcode.env.updates\" ]]; then\n  source \"$PODS_ROOT/../.xcode.env.updates\"\nfi\n# Source local changes to allow overrides\n# if needed\nif [[ -f \"$PODS_ROOT/../.xcode.env.local\" ]]; then\n  source \"$PODS_ROOT/../.xcode.env.local\"\nfi\n\n`\"$NODE_BINARY\" --print \"require('path').dirname(require.resolve('react-native/package.json')) + '/scripts/react-native-xcode.sh'\"`\n\n";
		};
		08A4A3CD28434E44B6B9DE2E /* [CP] Check Pods Manifest.lock */ = {
			isa = PBXShellScriptBuildPhase;
			buildActionMask = 2147483647;
			files = (
			);
			inputFileListPaths = (
			);
			inputPaths = (
				"${PODS_PODFILE_DIR_PATH}/Podfile.lock",
				"${PODS_ROOT}/Manifest.lock",
			);
			name = "[CP] Check Pods Manifest.lock";
			outputFileListPaths = (
			);
			outputPaths = (
				"$(DERIVED_FILE_DIR)/Pods-expoappboilerplate-checkManifestLockResult.txt",
			);
			runOnlyForDeploymentPostprocessing = 0;
			shellPath = /bin/sh;
			shellScript = "diff \"${PODS_PODFILE_DIR_PATH}/Podfile.lock\" \"${PODS_ROOT}/Manifest.lock\" > /dev/null\nif [ $? != 0 ] ; then\n    # print error to STDERR\n    echo \"error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.\" >&2\n    exit 1\nfi\n# This output is used by Xcode 'outputs' to avoid re-running this script phase.\necho \"SUCCESS\" > \"${SCRIPT_OUTPUT_FILE_0}\"\n";
			showEnvVarsInLog = 0;
		};
		68ACA832FEA019FF7556258A /* [CP] Embed Pods Frameworks */ = {
			isa = PBXShellScriptBuildPhase;
			buildActionMask = 2147483647;
			files = (
			);
			inputPaths = (
				"${PODS_ROOT}/Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate-frameworks.sh",
				"${PODS_XCFRAMEWORKS_BUILD_DIR}/hermes-engine/Pre-built/hermes.framework/hermes",
			);
			name = "[CP] Embed Pods Frameworks";
			outputPaths = (
				"${TARGET_BUILD_DIR}/${FRAMEWORKS_FOLDER_PATH}/hermes.framework",
			);
			runOnlyForDeploymentPostprocessing = 0;
			shellPath = /bin/sh;
			shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate-frameworks.sh\"\n";
			showEnvVarsInLog = 0;
		};
		800E24972A6A228C8D4807E9 /* [CP] Copy Pods Resources */ = {
			isa = PBXShellScriptBuildPhase;
			buildActionMask = 2147483647;
			files = (
			);
			inputPaths = (
				"${PODS_ROOT}/Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate-resources.sh",
				"${PODS_CONFIGURATION_BUILD_DIR}/EXConstants/EXConstants.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/EXConstants/ExpoConstants_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/ExpoFileSystem/ExpoFileSystem_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/ExpoSystemUI/ExpoSystemUI_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/RCT-Folly/RCT-Folly_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/RNCAsyncStorage/RNCAsyncStorage_resources.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/React-Core/React-Core_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/React-cxxreact/React-cxxreact_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/SuperwallKit/SuperwallKit.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/boost/boost_privacy.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/expo-dev-launcher/EXDevLauncher.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/expo-dev-menu/EXDevMenu.bundle",
				"${PODS_CONFIGURATION_BUILD_DIR}/glog/glog_privacy.bundle",
			);
			name = "[CP] Copy Pods Resources";
			outputPaths = (
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/EXConstants.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/ExpoConstants_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/ExpoFileSystem_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/ExpoSystemUI_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/RCT-Folly_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/RNCAsyncStorage_resources.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/React-Core_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/React-cxxreact_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/SuperwallKit.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/boost_privacy.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/EXDevLauncher.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/EXDevMenu.bundle",
				"${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/glog_privacy.bundle",
			);
			runOnlyForDeploymentPostprocessing = 0;
			shellPath = /bin/sh;
			shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-expoappboilerplate/Pods-expoappboilerplate-resources.sh\"\n";
			showEnvVarsInLog = 0;
		};
		96E2F22856A27C2ACE5CE685 /* [Expo] Configure project */ = {
			isa = PBXShellScriptBuildPhase;
			alwaysOutOfDate = 1;
			buildActionMask = 2147483647;
			files = (
			);
			inputFileListPaths = (
			);
			inputPaths = (
			);
			name = "[Expo] Configure project";
			outputFileListPaths = (
			);
			outputPaths = (
			);
			runOnlyForDeploymentPostprocessing = 0;
			shellPath = /bin/sh;
			shellScript = "# This script configures Expo modules and generates the modules provider file.\nbash -l -c \"./Pods/Target\\ Support\\ Files/Pods-expoappboilerplate/expo-configure-project.sh\"\n";
		};
/* End PBXShellScriptBuildPhase section */

/* Begin PBXSourcesBuildPhase section */
		13B07F871A680F5B00A75B9A /* Sources */ = {
			isa = PBXSourcesBuildPhase;
			buildActionMask = 2147483647;
			files = (
				13B07FBC1A68108700A75B9A /* AppDelegate.mm in Sources */,
				13B07FC11A68108700A75B9A /* main.m in Sources */,
				B18059E884C0ABDD17F3DC3D /* ExpoModulesProvider.swift in Sources */,
				190D622815B7446E85DE663F /* noop-file.swift in Sources */,
			);
			runOnlyForDeploymentPostprocessing = 0;
		};
/* End PBXSourcesBuildPhase section */

/* Begin XCBuildConfiguration section */
		13B07F941A680F5B00A75B9A /* Debug */ = {
			isa = XCBuildConfiguration;
			baseConfigurationReference = 6C2E3173556A471DD304B334 /* Pods-expoappboilerplate.debug.xcconfig */;
			buildSettings = {
				ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;
				CLANG_ENABLE_MODULES = YES;
				CODE_SIGN_ENTITLEMENTS = expoappboilerplate/expoappboilerplate.entitlements;
				CURRENT_PROJECT_VERSION = 1;
				ENABLE_BITCODE = NO;
				GCC_PREPROCESSOR_DEFINITIONS = (
					"$(inherited)",
					"FB_SONARKIT_ENABLED=1",
				);
				INFOPLIST_FILE = expoappboilerplate/Info.plist;
				IPHONEOS_DEPLOYMENT_TARGET = 15.1;
				LD_RUNPATH_SEARCH_PATHS = "$(inherited) @executable_path/Frameworks";
				MARKETING_VERSION = 1.0;
				OTHER_LDFLAGS = (
					"$(inherited)",
					"-ObjC",
					"-lc++",
				);
				OTHER_SWIFT_FLAGS = "$(inherited) -D EXPO_CONFIGURATION_DEBUG";
				PRODUCT_BUNDLE_IDENTIFIER = com.cqjack.expoappboilerplate;
				PRODUCT_NAME = expoappboilerplate;
				SWIFT_OBJC_BRIDGING_HEADER = "expoappboilerplate/expoappboilerplate-Bridging-Header.h";
				SWIFT_OPTIMIZATION_LEVEL = "-Onone";
				SWIFT_VERSION = 5.0;
				TARGETED_DEVICE_FAMILY = "1,2";
				VERSIONING_SYSTEM = "apple-generic";
			};
			name = Debug;
		};
		13B07F951A680F5B00A75B9A /* Release */ = {
			isa = XCBuildConfiguration;
			baseConfigurationReference = 7A4D352CD337FB3A3BF06240 /* Pods-expoappboilerplate.release.xcconfig */;
			buildSettings = {
				ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;
				CLANG_ENABLE_MODULES = YES;
				CODE_SIGN_ENTITLEMENTS = expoappboilerplate/expoappboilerplate.entitlements;
				CURRENT_PROJECT_VERSION = 1;
				INFOPLIST_FILE = expoappboilerplate/Info.plist;
				IPHONEOS_DEPLOYMENT_TARGET = 15.1;
				LD_RUNPATH_SEARCH_PATHS = "$(inherited) @executable_path/Frameworks";
				MARKETING_VERSION = 1.0;
				OTHER_LDFLAGS = (
					"$(inherited)",
					"-ObjC",
					"-lc++",
				);
				OTHER_SWIFT_FLAGS = "$(inherited) -D EXPO_CONFIGURATION_RELEASE";
				PRODUCT_BUNDLE_IDENTIFIER = com.cqjack.expoappboilerplate;
				PRODUCT_NAME = expoappboilerplate;
				SWIFT_OBJC_BRIDGING_HEADER = "expoappboilerplate/expoappboilerplate-Bridging-Header.h";
				SWIFT_VERSION = 5.0;
				TARGETED_DEVICE_FAMILY = "1,2";
				VERSIONING_SYSTEM = "apple-generic";
			};
			name = Release;
		};
		83CBBA201A601CBA00E9B192 /* Debug */ = {
			isa = XCBuildConfiguration;
			buildSettings = {
				ALWAYS_SEARCH_USER_PATHS = NO;
				CLANG_ANALYZER_LOCALIZABILITY_NONLOCALIZED = YES;
				CLANG_CXX_LANGUAGE_STANDARD = "c++20";
				CLANG_CXX_LIBRARY = "libc++";
				CLANG_ENABLE_MODULES = YES;
				CLANG_ENABLE_OBJC_ARC = YES;
				CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;
				CLANG_WARN_BOOL_CONVERSION = YES;
				CLANG_WARN_COMMA = YES;
				CLANG_WARN_CONSTANT_CONVERSION = YES;
				CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;
				CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;
				CLANG_WARN_EMPTY_BODY = YES;
				CLANG_WARN_ENUM_CONVERSION = YES;
				CLANG_WARN_INFINITE_RECURSION = YES;
				CLANG_WARN_INT_CONVERSION = YES;
				CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;
				CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;
				CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;
				CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;
				CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;
				CLANG_WARN_STRICT_PROTOTYPES = YES;
				CLANG_WARN_SUSPICIOUS_MOVE = YES;
				CLANG_WARN_UNREACHABLE_CODE = YES;
				CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;
				"CODE_SIGN_IDENTITY[sdk=iphoneos*]" = "iPhone Developer";
				COPY_PHASE_STRIP = NO;
				ENABLE_STRICT_OBJC_MSGSEND = YES;
				ENABLE_TESTABILITY = YES;
				GCC_C_LANGUAGE_STANDARD = gnu99;
				GCC_DYNAMIC_NO_PIC = NO;
				GCC_NO_COMMON_BLOCKS = YES;
				GCC_OPTIMIZATION_LEVEL = 0;
				GCC_PREPROCESSOR_DEFINITIONS = (
					"DEBUG=1",
					"$(inherited)",
				);
				GCC_SYMBOLS_PRIVATE_EXTERN = NO;
				GCC_WARN_64_TO_32_BIT_CONVERSION = YES;
				GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;
				GCC_WARN_UNDECLARED_SELECTOR = YES;
				GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;
				GCC_WARN_UNUSED_FUNCTION = YES;
				GCC_WARN_UNUSED_VARIABLE = YES;
				IPHONEOS_DEPLOYMENT_TARGET = 15.1;
				LD_RUNPATH_SEARCH_PATHS = "/usr/lib/swift $(inherited)";
				LIBRARY_SEARCH_PATHS = "$(SDKROOT)/usr/lib/swift\"$(inherited)\"";
				MTL_ENABLE_DEBUG_INFO = YES;
				ONLY_ACTIVE_ARCH = YES;
				OTHER_LDFLAGS = (
					"$(inherited)",
					" ",
				);
				REACT_NATIVE_PATH = "${PODS_ROOT}/../../node_modules/react-native";
				SDKROOT = iphoneos;
				SWIFT_ACTIVE_COMPILATION_CONDITIONS = "$(inherited) DEBUG";
				USE_HERMES = true;
			};
			name = Debug;
		};
		83CBBA211A601CBA00E9B192 /* Release */ = {
			isa = XCBuildConfiguration;
			buildSettings = {
				ALWAYS_SEARCH_USER_PATHS = NO;
				CLANG_ANALYZER_LOCALIZABILITY_NONLOCALIZED = YES;
				CLANG_CXX_LANGUAGE_STANDARD = "c++20";
				CLANG_CXX_LIBRARY = "libc++";
				CLANG_ENABLE_MODULES = YES;
				CLANG_ENABLE_OBJC_ARC = YES;
				CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;
				CLANG_WARN_BOOL_CONVERSION = YES;
				CLANG_WARN_COMMA = YES;
				CLANG_WARN_CONSTANT_CONVERSION = YES;
				CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;
				CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;
				CLANG_WARN_EMPTY_BODY = YES;
				CLANG_WARN_ENUM_CONVERSION = YES;
				CLANG_WARN_INFINITE_RECURSION = YES;
				CLANG_WARN_INT_CONVERSION = YES;
				CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;
				CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;
				CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;
				CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;
				CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;
				CLANG_WARN_STRICT_PROTOTYPES = YES;
				CLANG_WARN_SUSPICIOUS_MOVE = YES;
				CLANG_WARN_UNREACHABLE_CODE = YES;
				CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;
				"CODE_SIGN_IDENTITY[sdk=iphoneos*]" = "iPhone Developer";
				COPY_PHASE_STRIP = YES;
				ENABLE_NS_ASSERTIONS = NO;
				ENABLE_STRICT_OBJC_MSGSEND = YES;
				GCC_C_LANGUAGE_STANDARD = gnu99;
				GCC_NO_COMMON_BLOCKS = YES;
				GCC_WARN_64_TO_32_BIT_CONVERSION = YES;
				GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;
				GCC_WARN_UNDECLARED_SELECTOR = YES;
				GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;
				GCC_WARN_UNUSED_FUNCTION = YES;
				GCC_WARN_UNUSED_VARIABLE = YES;
				IPHONEOS_DEPLOYMENT_TARGET = 15.1;
				LD_RUNPATH_SEARCH_PATHS = "/usr/lib/swift $(inherited)";
				LIBRARY_SEARCH_PATHS = "$(SDKROOT)/usr/lib/swift\"$(inherited)\"";
				MTL_ENABLE_DEBUG_INFO = NO;
				OTHER_LDFLAGS = (
					"$(inherited)",
					" ",
				);
				REACT_NATIVE_PATH = "${PODS_ROOT}/../../node_modules/react-native";
				SDKROOT = iphoneos;
				USE_HERMES = true;
				VALIDATE_PRODUCT = YES;
			};
			name = Release;
		};
/* End XCBuildConfiguration section */

/* Begin XCConfigurationList section */
		13B07F931A680F5B00A75B9A /* Build configuration list for PBXNativeTarget "expoappboilerplate" */ = {
			isa = XCConfigurationList;
			buildConfigurations = (
				13B07F941A680F5B00A75B9A /* Debug */,
				13B07F951A680F5B00A75B9A /* Release */,
			);
			defaultConfigurationIsVisible = 0;
			defaultConfigurationName = Release;
		};
		83CBB9FA1A601CBA00E9B192 /* Build configuration list for PBXProject "expoappboilerplate" */ = {
			isa = XCConfigurationList;
			buildConfigurations = (
				83CBBA201A601CBA00E9B192 /* Debug */,
				83CBBA211A601CBA00E9B192 /* Release */,
			);
			defaultConfigurationIsVisible = 0;
			defaultConfigurationName = Release;
		};
/* End XCConfigurationList section */
	};
	rootObject = 83CBB9F71A601CBA00E9B192 /* Project object */;
}

```

# ios/expoappboilerplate.xcodeproj/xcshareddata/xcschemes/expoappboilerplate.xcscheme

```xcscheme
<?xml version="1.0" encoding="UTF-8"?>
<Scheme
   LastUpgradeVersion = "1130"
   version = "1.3">
   <BuildAction
      parallelizeBuildables = "YES"
      buildImplicitDependencies = "YES">
      <BuildActionEntries>
         <BuildActionEntry
            buildForTesting = "YES"
            buildForRunning = "YES"
            buildForProfiling = "YES"
            buildForArchiving = "YES"
            buildForAnalyzing = "YES">
            <BuildableReference
               BuildableIdentifier = "primary"
               BlueprintIdentifier = "13B07F861A680F5B00A75B9A"
               BuildableName = "expoappboilerplate.app"
               BlueprintName = "expoappboilerplate"
               ReferencedContainer = "container:expoappboilerplate.xcodeproj">
            </BuildableReference>
         </BuildActionEntry>
      </BuildActionEntries>
   </BuildAction>
   <TestAction
      buildConfiguration = "Debug"
      selectedDebuggerIdentifier = "Xcode.DebuggerFoundation.Debugger.LLDB"
      selectedLauncherIdentifier = "Xcode.DebuggerFoundation.Launcher.LLDB"
      shouldUseLaunchSchemeArgsEnv = "YES">
      <Testables>
         <TestableReference
            skipped = "NO">
            <BuildableReference
               BuildableIdentifier = "primary"
               BlueprintIdentifier = "00E356ED1AD99517003FC87E"
               BuildableName = "expoappboilerplateTests.xctest"
               BlueprintName = "expoappboilerplateTests"
               ReferencedContainer = "container:expoappboilerplate.xcodeproj">
            </BuildableReference>
         </TestableReference>
      </Testables>
   </TestAction>
   <LaunchAction
      buildConfiguration = "Debug"
      selectedDebuggerIdentifier = "Xcode.DebuggerFoundation.Debugger.LLDB"
      selectedLauncherIdentifier = "Xcode.DebuggerFoundation.Launcher.LLDB"
      launchStyle = "0"
      useCustomWorkingDirectory = "NO"
      ignoresPersistentStateOnLaunch = "NO"
      debugDocumentVersioning = "YES"
      debugServiceExtension = "internal"
      allowLocationSimulation = "YES">
      <BuildableProductRunnable
         runnableDebuggingMode = "0">
         <BuildableReference
            BuildableIdentifier = "primary"
            BlueprintIdentifier = "13B07F861A680F5B00A75B9A"
            BuildableName = "expoappboilerplate.app"
            BlueprintName = "expoappboilerplate"
            ReferencedContainer = "container:expoappboilerplate.xcodeproj">
         </BuildableReference>
      </BuildableProductRunnable>
   </LaunchAction>
   <ProfileAction
      buildConfiguration = "Release"
      shouldUseLaunchSchemeArgsEnv = "YES"
      savedToolIdentifier = ""
      useCustomWorkingDirectory = "NO"
      debugDocumentVersioning = "YES">
      <BuildableProductRunnable
         runnableDebuggingMode = "0">
         <BuildableReference
            BuildableIdentifier = "primary"
            BlueprintIdentifier = "13B07F861A680F5B00A75B9A"
            BuildableName = "expoappboilerplate.app"
            BlueprintName = "expoappboilerplate"
            ReferencedContainer = "container:expoappboilerplate.xcodeproj">
         </BuildableReference>
      </BuildableProductRunnable>
   </ProfileAction>
   <AnalyzeAction
      buildConfiguration = "Debug">
   </AnalyzeAction>
   <ArchiveAction
      buildConfiguration = "Release"
      revealArchiveInOrganizer = "YES">
   </ArchiveAction>
</Scheme>

```

# ios/expoappboilerplate.xcworkspace/contents.xcworkspacedata

```xcworkspacedata
<?xml version="1.0" encoding="UTF-8"?>
<Workspace
   version = "1.0">
   <FileRef
      location = "group:expoappboilerplate.xcodeproj">
   </FileRef>
   <FileRef
      location = "group:Pods/Pods.xcodeproj">
   </FileRef>
</Workspace>

```

# ios/expoappboilerplate/AppDelegate.h

```h
#import <RCTAppDelegate.h>
#import <UIKit/UIKit.h>
#import <Expo/Expo.h>

@interface AppDelegate : EXAppDelegateWrapper

@end

```

# ios/expoappboilerplate/AppDelegate.mm

```mm
#import "AppDelegate.h"

#import <React/RCTBundleURLProvider.h>
#import <React/RCTLinkingManager.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.moduleName = @"main";

  // You can add your custom initial props in the dictionary below.
  // They will be passed down to the ViewController used by React Native.
  self.initialProps = @{};

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  return [self bundleURL];
}

- (NSURL *)bundleURL
{
#if DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@".expo/.virtual-metro-entry"];
#else
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
}

// Linking API
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
  return [super application:application openURL:url options:options] || [RCTLinkingManager application:application openURL:url options:options];
}

// Universal Links
- (BOOL)application:(UIApplication *)application continueUserActivity:(nonnull NSUserActivity *)userActivity restorationHandler:(nonnull void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler {
  BOOL result = [RCTLinkingManager application:application continueUserActivity:userActivity restorationHandler:restorationHandler];
  return [super application:application continueUserActivity:userActivity restorationHandler:restorationHandler] || result;
}

// Explicitly define remote notification delegates to ensure compatibility with some third-party libraries
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
{
  return [super application:application didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
}

// Explicitly define remote notification delegates to ensure compatibility with some third-party libraries
- (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
{
  return [super application:application didFailToRegisterForRemoteNotificationsWithError:error];
}

// Explicitly define remote notification delegates to ensure compatibility with some third-party libraries
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
{
  return [super application:application didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
}

@end

```

# ios/expoappboilerplate/expoappboilerplate-Bridging-Header.h

```h
//
//  Use this file to import your target's public headers that you would like to expose to Swift.
//

```

# ios/expoappboilerplate/expoappboilerplate.entitlements

```entitlements
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict/>
</plist>
```

# ios/expoappboilerplate/Images.xcassets/AppIcon.appiconset/App-Icon-1024x1024@1x.png

This is a binary file of the type: Image

# ios/expoappboilerplate/Images.xcassets/AppIcon.appiconset/Contents.json

```json
{
  "images": [
    {
      "filename": "App-Icon-1024x1024@1x.png",
      "idiom": "universal",
      "platform": "ios",
      "size": "1024x1024"
    }
  ],
  "info": {
    "version": 1,
    "author": "expo"
  }
}
```

# ios/expoappboilerplate/Images.xcassets/Contents.json

```json
{
  "info" : {
    "version" : 1,
    "author" : "expo"
  }
}

```

# ios/expoappboilerplate/Images.xcassets/SplashScreenBackground.colorset/Contents.json

```json
{
  "colors": [
    {
      "color": {
        "components": {
          "alpha": "1.000",
          "blue": "1.00000000000000",
          "green": "1.00000000000000",
          "red": "1.00000000000000"
        },
        "color-space": "srgb"
      },
      "idiom": "universal"
    }
  ],
  "info": {
    "version": 1,
    "author": "expo"
  }
}
```

# ios/expoappboilerplate/Images.xcassets/SplashScreenLogo.imageset/Contents.json

```json
{
  "images": [
    {
      "idiom": "universal",
      "filename": "image.png",
      "scale": "1x"
    },
    {
      "idiom": "universal",
      "filename": "image@2x.png",
      "scale": "2x"
    },
    {
      "idiom": "universal",
      "filename": "image@3x.png",
      "scale": "3x"
    }
  ],
  "info": {
    "version": 1,
    "author": "expo"
  }
}
```

# ios/expoappboilerplate/Images.xcassets/SplashScreenLogo.imageset/image.png

This is a binary file of the type: Image

# ios/expoappboilerplate/Images.xcassets/SplashScreenLogo.imageset/image@2x.png

This is a binary file of the type: Image

# ios/expoappboilerplate/Images.xcassets/SplashScreenLogo.imageset/image@3x.png

This is a binary file of the type: Image

# ios/expoappboilerplate/Info.plist

```plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>CADisableMinimumFrameDurationOnPhone</key>
    <true/>
    <key>CFBundleDevelopmentRegion</key>
    <string>$(DEVELOPMENT_LANGUAGE)</string>
    <key>CFBundleDisplayName</key>
    <string>expo-app-boilerplate</string>
    <key>CFBundleExecutable</key>
    <string>$(EXECUTABLE_NAME)</string>
    <key>CFBundleIdentifier</key>
    <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
    <key>CFBundleInfoDictionaryVersion</key>
    <string>6.0</string>
    <key>CFBundleName</key>
    <string>$(PRODUCT_NAME)</string>
    <key>CFBundlePackageType</key>
    <string>$(PRODUCT_BUNDLE_PACKAGE_TYPE)</string>
    <key>CFBundleShortVersionString</key>
    <string>1.0.0</string>
    <key>CFBundleSignature</key>
    <string>????</string>
    <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>myapp</string>
          <string>com.cqjack.expoappboilerplate</string>
        </array>
      </dict>
      <dict>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>exp+expo-app-boilerplate</string>
        </array>
      </dict>
    </array>
    <key>CFBundleVersion</key>
    <string>1</string>
    <key>ITSAppUsesNonExemptEncryption</key>
    <false/>
    <key>LSMinimumSystemVersion</key>
    <string>12.0</string>
    <key>LSRequiresIPhoneOS</key>
    <true/>
    <key>NSAppTransportSecurity</key>
    <dict>
      <key>NSAllowsArbitraryLoads</key>
      <false/>
      <key>NSAllowsLocalNetworking</key>
      <true/>
    </dict>
    <key>NSUserActivityTypes</key>
    <array>
      <string>$(PRODUCT_BUNDLE_IDENTIFIER).expo.index_route</string>
    </array>
    <key>UILaunchStoryboardName</key>
    <string>SplashScreen</string>
    <key>UIRequiredDeviceCapabilities</key>
    <array>
      <string>arm64</string>
    </array>
    <key>UIRequiresFullScreen</key>
    <false/>
    <key>UIStatusBarStyle</key>
    <string>UIStatusBarStyleDefault</string>
    <key>UISupportedInterfaceOrientations</key>
    <array>
      <string>UIInterfaceOrientationPortrait</string>
      <string>UIInterfaceOrientationPortraitUpsideDown</string>
    </array>
    <key>UISupportedInterfaceOrientations~ipad</key>
    <array>
      <string>UIInterfaceOrientationPortrait</string>
      <string>UIInterfaceOrientationPortraitUpsideDown</string>
      <string>UIInterfaceOrientationLandscapeLeft</string>
      <string>UIInterfaceOrientationLandscapeRight</string>
    </array>
    <key>UIUserInterfaceStyle</key>
    <string>Automatic</string>
    <key>UIViewControllerBasedStatusBarAppearance</key>
    <false/>
  </dict>
</plist>
```

# ios/expoappboilerplate/main.m

```m
#import <UIKit/UIKit.h>

#import "AppDelegate.h"

int main(int argc, char * argv[]) {
  @autoreleasepool {
    return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
  }
}


```

# ios/expoappboilerplate/noop-file.swift

```swift
//
// @generated
// A blank Swift file must be created for native modules with Swift files to work correctly.
//

```

# ios/expoappboilerplate/PrivacyInfo.xcprivacy

```xcprivacy
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>NSPrivacyAccessedAPITypes</key>
	<array>
		<dict>
			<key>NSPrivacyAccessedAPIType</key>
			<string>NSPrivacyAccessedAPICategoryUserDefaults</string>
			<key>NSPrivacyAccessedAPITypeReasons</key>
			<array>
				<string>CA92.1</string>
			</array>
		</dict>
		<dict>
			<key>NSPrivacyAccessedAPIType</key>
			<string>NSPrivacyAccessedAPICategoryFileTimestamp</string>
			<key>NSPrivacyAccessedAPITypeReasons</key>
			<array>
				<string>0A2A.1</string>
				<string>3B52.1</string>
				<string>C617.1</string>
			</array>
		</dict>
		<dict>
			<key>NSPrivacyAccessedAPIType</key>
			<string>NSPrivacyAccessedAPICategoryDiskSpace</string>
			<key>NSPrivacyAccessedAPITypeReasons</key>
			<array>
				<string>E174.1</string>
				<string>85F4.1</string>
			</array>
		</dict>
		<dict>
			<key>NSPrivacyAccessedAPIType</key>
			<string>NSPrivacyAccessedAPICategorySystemBootTime</string>
			<key>NSPrivacyAccessedAPITypeReasons</key>
			<array>
				<string>35F9.1</string>
			</array>
		</dict>
	</array>
	<key>NSPrivacyCollectedDataTypes</key>
	<array/>
	<key>NSPrivacyTracking</key>
	<false/>
</dict>
</plist>

```

# ios/expoappboilerplate/SplashScreen.storyboard

```storyboard
<?xml version="1.0" encoding="UTF-8"?>
<document type="com.apple.InterfaceBuilder3.CocoaTouch.Storyboard.XIB" version="3.0" toolsVersion="32700.99.1234" targetRuntime="iOS.CocoaTouch" propertyAccessControl="none" useAutolayout="YES" launchScreen="YES" useTraitCollections="YES" useSafeAreas="YES" colorMatched="YES" initialViewController="EXPO-VIEWCONTROLLER-1">
    <device id="retina6_12" orientation="portrait" appearance="light"/>
    <dependencies>
        <deployment identifier="iOS"/>
        <plugIn identifier="com.apple.InterfaceBuilder.IBCocoaTouchPlugin" version="22685"/>
        <capability name="Named colors" minToolsVersion="9.0"/>
        <capability name="Safe area layout guides" minToolsVersion="9.0"/>
        <capability name="documents saved in the Xcode 8 format" minToolsVersion="8.0"/>
    </dependencies>
    <scenes>
        <scene sceneID="EXPO-SCENE-1">
            <objects>
                <viewController storyboardIdentifier="SplashScreenViewController" id="EXPO-VIEWCONTROLLER-1" sceneMemberID="viewController">
                    <view key="view" userInteractionEnabled="NO" contentMode="scaleToFill" insetsLayoutMarginsFromSafeArea="NO" id="EXPO-ContainerView" userLabel="ContainerView">
                        <rect key="frame" x="0.0" y="0.0" width="393" height="852"/>
                        <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                        <subviews>
                            <imageView id="EXPO-SplashScreen" userLabel="SplashScreenLogo" image="SplashScreenLogo" contentMode="scaleAspectFit" clipsSubviews="true" userInteractionEnabled="false" translatesAutoresizingMaskIntoConstraints="false">
                                <rect key="frame" x="96.5" y="326" width="200" height="200"/>
                            </imageView>
                        </subviews>
                        <viewLayoutGuide key="safeArea" id="Rmq-lb-GrQ"/>
                        <constraints>
                            <constraint firstItem="EXPO-SplashScreen" firstAttribute="centerX" secondItem="EXPO-ContainerView" secondAttribute="centerX" id="cad2ab56f97c5429bf29decf850647a4216861d4"/>
                            <constraint firstItem="EXPO-SplashScreen" firstAttribute="centerY" secondItem="EXPO-ContainerView" secondAttribute="centerY" id="1a145271b085b6ce89b1405a310f5b1bb7656595"/>
                        </constraints>
                        <color key="backgroundColor" name="SplashScreenBackground"/>
                    </view>
                </viewController>
                <placeholder placeholderIdentifier="IBFirstResponder" id="EXPO-PLACEHOLDER-1" userLabel="First Responder" sceneMemberID="firstResponder"/>
            </objects>
            <point key="canvasLocation" x="0.0" y="0.0"/>
        </scene>
    </scenes>
    <resources>
        <image name="SplashScreenLogo" width="200" height="200"/>
        <namedColor name="SplashScreenBackground">
            <color alpha="1.000" blue="1.00000000000000" green="1.00000000000000" red="1.00000000000000" customColorSpace="sRGB" colorSpace="custom"/>
        </namedColor>
    </resources>
</document>
```

# ios/expoappboilerplate/Supporting/Expo.plist

```plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>EXUpdatesCheckOnLaunch</key>
    <string>ALWAYS</string>
    <key>EXUpdatesEnabled</key>
    <false/>
    <key>EXUpdatesLaunchWaitMs</key>
    <integer>0</integer>
  </dict>
</plist>
```

# ios/Podfile

```
require File.join(File.dirname(`node --print "require.resolve('expo/package.json')"`), "scripts/autolinking")
require File.join(File.dirname(`node --print "require.resolve('react-native/package.json')"`), "scripts/react_native_pods")

require 'json'
podfile_properties = JSON.parse(File.read(File.join(__dir__, 'Podfile.properties.json'))) rescue {}

ENV['RCT_NEW_ARCH_ENABLED'] = podfile_properties['newArchEnabled'] == 'true' ? '1' : '0'
ENV['EX_DEV_CLIENT_NETWORK_INSPECTOR'] = podfile_properties['EX_DEV_CLIENT_NETWORK_INSPECTOR']

platform :ios, podfile_properties['ios.deploymentTarget'] || '15.1'
install! 'cocoapods',
  :deterministic_uuids => false

prepare_react_native_project!

target 'expoappboilerplate' do
  use_expo_modules!

  if ENV['EXPO_USE_COMMUNITY_AUTOLINKING'] == '1'
    config_command = ['node', '-e', "process.argv=['', '', 'config'];require('@react-native-community/cli').run()"];
  else
    config_command = [
      'node',
      '--no-warnings',
      '--eval',
      'require(require.resolve(\'expo-modules-autolinking\', { paths: [require.resolve(\'expo/package.json\')] }))(process.argv.slice(1))',
      'react-native-config',
      '--json',
      '--platform',
      'ios'
    ]
  end

  config = use_native_modules!(config_command)

  use_frameworks! :linkage => podfile_properties['ios.useFrameworks'].to_sym if podfile_properties['ios.useFrameworks']
  use_frameworks! :linkage => ENV['USE_FRAMEWORKS'].to_sym if ENV['USE_FRAMEWORKS']

  use_react_native!(
    :path => config[:reactNativePath],
    :hermes_enabled => podfile_properties['expo.jsEngine'] == nil || podfile_properties['expo.jsEngine'] == 'hermes',
    # An absolute path to your application root.
    :app_path => "#{Pod::Config.instance.installation_root}/..",
    :privacy_file_aggregation_enabled => podfile_properties['apple.privacyManifestAggregationEnabled'] != 'false',
  )

  post_install do |installer|
    react_native_post_install(
      installer,
      config[:reactNativePath],
      :mac_catalyst_enabled => false,
      :ccache_enabled => podfile_properties['apple.ccacheEnabled'] == 'true',
    )

    # This is necessary for Xcode 14, because it signs resource bundles by default
    # when building for devices.
    installer.target_installation_results.pod_target_installation_results
      .each do |pod_name, target_installation_result|
      target_installation_result.resource_bundle_targets.each do |resource_bundle_target|
        resource_bundle_target.build_configurations.each do |config|
          config.build_settings['CODE_SIGNING_ALLOWED'] = 'NO'
        end
      end
    end
  end
end

```

# ios/Podfile.lock

```lock
PODS:
  - boost (1.84.0)
  - DoubleConversion (1.1.6)
  - EXConstants (17.0.6):
    - ExpoModulesCore
  - EXJSONUtils (0.14.0)
  - EXManifests (0.15.6):
    - ExpoModulesCore
  - Expo (52.0.35):
    - ExpoModulesCore
  - expo-dev-client (5.0.12):
    - EXManifests
    - expo-dev-launcher
    - expo-dev-menu
    - expo-dev-menu-interface
    - EXUpdatesInterface
  - expo-dev-launcher (5.0.29):
    - DoubleConversion
    - EXManifests
    - expo-dev-launcher/Main (= 5.0.29)
    - expo-dev-menu
    - expo-dev-menu-interface
    - ExpoModulesCore
    - EXUpdatesInterface
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTAppDelegate
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-launcher/Main (5.0.29):
    - DoubleConversion
    - EXManifests
    - expo-dev-launcher/Unsafe
    - expo-dev-menu
    - expo-dev-menu-interface
    - ExpoModulesCore
    - EXUpdatesInterface
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTAppDelegate
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-launcher/Unsafe (5.0.29):
    - DoubleConversion
    - EXManifests
    - expo-dev-menu
    - expo-dev-menu-interface
    - ExpoModulesCore
    - EXUpdatesInterface
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTAppDelegate
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-menu (6.0.19):
    - DoubleConversion
    - expo-dev-menu/Main (= 6.0.19)
    - expo-dev-menu/ReactNativeCompatibles (= 6.0.19)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-menu-interface (1.9.3)
  - expo-dev-menu/Main (6.0.19):
    - DoubleConversion
    - EXManifests
    - expo-dev-menu-interface
    - expo-dev-menu/Vendored
    - ExpoModulesCore
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-menu/ReactNativeCompatibles (6.0.19):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-menu/SafeAreaView (6.0.19):
    - DoubleConversion
    - ExpoModulesCore
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - expo-dev-menu/Vendored (6.0.19):
    - DoubleConversion
    - expo-dev-menu/SafeAreaView
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - ExpoAsset (11.0.3):
    - ExpoModulesCore
  - ExpoBlur (14.0.3):
    - ExpoModulesCore
  - ExpoFileSystem (18.0.10):
    - ExpoModulesCore
  - ExpoFont (13.0.3):
    - ExpoModulesCore
  - ExpoHaptics (14.0.1):
    - ExpoModulesCore
  - ExpoHead (4.0.17):
    - ExpoModulesCore
  - ExpoKeepAwake (14.0.2):
    - ExpoModulesCore
  - ExpoLinking (7.0.5):
    - ExpoModulesCore
  - ExpoModulesCore (2.2.2):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTAppDelegate
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - ExpoSplashScreen (0.29.22):
    - ExpoModulesCore
  - ExpoSymbols (0.2.2):
    - ExpoModulesCore
  - ExpoSystemUI (4.0.8):
    - ExpoModulesCore
  - ExpoWebBrowser (14.0.2):
    - ExpoModulesCore
  - EXUpdatesInterface (1.0.0):
    - ExpoModulesCore
  - FBLazyVector (0.76.7)
  - fmt (9.1.0)
  - glog (0.3.5)
  - hermes-engine (0.76.7):
    - hermes-engine/Pre-built (= 0.76.7)
  - hermes-engine/Pre-built (0.76.7)
  - RCT-Folly (2024.01.01.00):
    - boost
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - RCT-Folly/Default (= 2024.01.01.00)
  - RCT-Folly/Default (2024.01.01.00):
    - boost
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
  - RCT-Folly/Fabric (2024.01.01.00):
    - boost
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
  - RCTDeprecation (0.76.7)
  - RCTRequired (0.76.7)
  - RCTTypeSafety (0.76.7):
    - FBLazyVector (= 0.76.7)
    - RCTRequired (= 0.76.7)
    - React-Core (= 0.76.7)
  - React (0.76.7):
    - React-Core (= 0.76.7)
    - React-Core/DevSupport (= 0.76.7)
    - React-Core/RCTWebSocket (= 0.76.7)
    - React-RCTActionSheet (= 0.76.7)
    - React-RCTAnimation (= 0.76.7)
    - React-RCTBlob (= 0.76.7)
    - React-RCTImage (= 0.76.7)
    - React-RCTLinking (= 0.76.7)
    - React-RCTNetwork (= 0.76.7)
    - React-RCTSettings (= 0.76.7)
    - React-RCTText (= 0.76.7)
    - React-RCTVibration (= 0.76.7)
  - React-callinvoker (0.76.7)
  - React-Core (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default (= 0.76.7)
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/CoreModulesHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/Default (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/DevSupport (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default (= 0.76.7)
    - React-Core/RCTWebSocket (= 0.76.7)
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTActionSheetHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTAnimationHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTBlobHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTImageHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTLinkingHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTNetworkHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTSettingsHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTTextHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTVibrationHeaders (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-Core/RCTWebSocket (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTDeprecation
    - React-Core/Default (= 0.76.7)
    - React-cxxreact
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-perflogger
    - React-runtimescheduler
    - React-utils
    - SocketRocket (= 0.7.1)
    - Yoga
  - React-CoreModules (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - RCT-Folly (= 2024.01.01.00)
    - RCTTypeSafety (= 0.76.7)
    - React-Core/CoreModulesHeaders (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTBlob
    - React-RCTImage (= 0.76.7)
    - ReactCodegen
    - ReactCommon
    - SocketRocket (= 0.7.1)
  - React-cxxreact (0.76.7):
    - boost
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-callinvoker (= 0.76.7)
    - React-debug (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-jsinspector
    - React-logger (= 0.76.7)
    - React-perflogger (= 0.76.7)
    - React-runtimeexecutor (= 0.76.7)
    - React-timing (= 0.76.7)
  - React-debug (0.76.7)
  - React-defaultsnativemodule (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-domnativemodule
    - React-Fabric
    - React-featureflags
    - React-featureflagsnativemodule
    - React-graphics
    - React-idlecallbacksnativemodule
    - React-ImageManager
    - React-microtasksnativemodule
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - React-domnativemodule (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-FabricComponents
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - React-Fabric (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric/animations (= 0.76.7)
    - React-Fabric/attributedstring (= 0.76.7)
    - React-Fabric/componentregistry (= 0.76.7)
    - React-Fabric/componentregistrynative (= 0.76.7)
    - React-Fabric/components (= 0.76.7)
    - React-Fabric/core (= 0.76.7)
    - React-Fabric/dom (= 0.76.7)
    - React-Fabric/imagemanager (= 0.76.7)
    - React-Fabric/leakchecker (= 0.76.7)
    - React-Fabric/mounting (= 0.76.7)
    - React-Fabric/observers (= 0.76.7)
    - React-Fabric/scheduler (= 0.76.7)
    - React-Fabric/telemetry (= 0.76.7)
    - React-Fabric/templateprocessor (= 0.76.7)
    - React-Fabric/uimanager (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/animations (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/attributedstring (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/componentregistry (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/componentregistrynative (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/components (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric/components/legacyviewmanagerinterop (= 0.76.7)
    - React-Fabric/components/root (= 0.76.7)
    - React-Fabric/components/view (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/components/legacyviewmanagerinterop (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/components/root (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/components/view (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
    - Yoga
  - React-Fabric/core (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/dom (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/imagemanager (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/leakchecker (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/mounting (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/observers (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric/observers/events (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/observers/events (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/scheduler (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric/observers/events
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-performancetimeline
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/telemetry (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/templateprocessor (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/uimanager (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric/uimanager/consistency (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererconsistency
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-Fabric/uimanager/consistency (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererconsistency
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCommon/turbomodule/core
  - React-FabricComponents (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-FabricComponents/components (= 0.76.7)
    - React-FabricComponents/textlayoutmanager (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-FabricComponents/components/inputaccessory (= 0.76.7)
    - React-FabricComponents/components/iostextinput (= 0.76.7)
    - React-FabricComponents/components/modal (= 0.76.7)
    - React-FabricComponents/components/rncore (= 0.76.7)
    - React-FabricComponents/components/safeareaview (= 0.76.7)
    - React-FabricComponents/components/scrollview (= 0.76.7)
    - React-FabricComponents/components/text (= 0.76.7)
    - React-FabricComponents/components/textinput (= 0.76.7)
    - React-FabricComponents/components/unimplementedview (= 0.76.7)
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/inputaccessory (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/iostextinput (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/modal (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/rncore (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/safeareaview (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/scrollview (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/text (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/textinput (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/components/unimplementedview (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricComponents/textlayoutmanager (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-cxxreact
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-logger
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/core
    - Yoga
  - React-FabricImage (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - RCTRequired (= 0.76.7)
    - RCTTypeSafety (= 0.76.7)
    - React-Fabric
    - React-graphics
    - React-ImageManager
    - React-jsi
    - React-jsiexecutor (= 0.76.7)
    - React-logger
    - React-rendererdebug
    - React-utils
    - ReactCommon
    - Yoga
  - React-featureflags (0.76.7)
  - React-featureflagsnativemodule (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - React-graphics (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-jsi
    - React-jsiexecutor
    - React-utils
  - React-hermes (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-cxxreact (= 0.76.7)
    - React-jsi
    - React-jsiexecutor (= 0.76.7)
    - React-jsinspector
    - React-perflogger (= 0.76.7)
    - React-runtimeexecutor
  - React-idlecallbacksnativemodule (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - React-ImageManager (0.76.7):
    - glog
    - RCT-Folly/Fabric
    - React-Core/Default
    - React-debug
    - React-Fabric
    - React-graphics
    - React-rendererdebug
    - React-utils
  - React-jserrorhandler (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-cxxreact
    - React-debug
    - React-jsi
  - React-jsi (0.76.7):
    - boost
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
  - React-jsiexecutor (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-cxxreact (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-jsinspector
    - React-perflogger (= 0.76.7)
  - React-jsinspector (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-featureflags
    - React-jsi
    - React-perflogger (= 0.76.7)
    - React-runtimeexecutor (= 0.76.7)
  - React-jsitracing (0.76.7):
    - React-jsi
  - React-logger (0.76.7):
    - glog
  - React-Mapbuffer (0.76.7):
    - glog
    - React-debug
  - React-microtasksnativemodule (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - react-native-safe-area-context (4.12.0):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - react-native-safe-area-context/common (= 4.12.0)
    - react-native-safe-area-context/fabric (= 4.12.0)
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - react-native-safe-area-context/common (4.12.0):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - react-native-safe-area-context/fabric (4.12.0):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - react-native-safe-area-context/common
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - react-native-webview (13.12.5):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - React-nativeconfig (0.76.7)
  - React-NativeModulesApple (0.76.7):
    - glog
    - hermes-engine
    - React-callinvoker
    - React-Core
    - React-cxxreact
    - React-jsi
    - React-jsinspector
    - React-runtimeexecutor
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
  - React-perflogger (0.76.7):
    - DoubleConversion
    - RCT-Folly (= 2024.01.01.00)
  - React-performancetimeline (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - React-cxxreact
    - React-timing
  - React-RCTActionSheet (0.76.7):
    - React-Core/RCTActionSheetHeaders (= 0.76.7)
  - React-RCTAnimation (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - RCTTypeSafety
    - React-Core/RCTAnimationHeaders
    - React-jsi
    - React-NativeModulesApple
    - ReactCodegen
    - ReactCommon
  - React-RCTAppDelegate (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-CoreModules
    - React-debug
    - React-defaultsnativemodule
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-hermes
    - React-nativeconfig
    - React-NativeModulesApple
    - React-RCTFabric
    - React-RCTImage
    - React-RCTNetwork
    - React-rendererdebug
    - React-RuntimeApple
    - React-RuntimeCore
    - React-RuntimeHermes
    - React-runtimescheduler
    - React-utils
    - ReactCodegen
    - ReactCommon
  - React-RCTBlob (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-Core/RCTBlobHeaders
    - React-Core/RCTWebSocket
    - React-jsi
    - React-jsinspector
    - React-NativeModulesApple
    - React-RCTNetwork
    - ReactCodegen
    - ReactCommon
  - React-RCTFabric (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-Core
    - React-debug
    - React-Fabric
    - React-FabricComponents
    - React-FabricImage
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-jsi
    - React-jsinspector
    - React-nativeconfig
    - React-performancetimeline
    - React-RCTImage
    - React-RCTText
    - React-rendererconsistency
    - React-rendererdebug
    - React-runtimescheduler
    - React-utils
    - Yoga
  - React-RCTImage (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - RCTTypeSafety
    - React-Core/RCTImageHeaders
    - React-jsi
    - React-NativeModulesApple
    - React-RCTNetwork
    - ReactCodegen
    - ReactCommon
  - React-RCTLinking (0.76.7):
    - React-Core/RCTLinkingHeaders (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-NativeModulesApple
    - ReactCodegen
    - ReactCommon
    - ReactCommon/turbomodule/core (= 0.76.7)
  - React-RCTNetwork (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - RCTTypeSafety
    - React-Core/RCTNetworkHeaders
    - React-jsi
    - React-NativeModulesApple
    - ReactCodegen
    - ReactCommon
  - React-RCTSettings (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - RCTTypeSafety
    - React-Core/RCTSettingsHeaders
    - React-jsi
    - React-NativeModulesApple
    - ReactCodegen
    - ReactCommon
  - React-RCTText (0.76.7):
    - React-Core/RCTTextHeaders (= 0.76.7)
    - Yoga
  - React-RCTVibration (0.76.7):
    - RCT-Folly (= 2024.01.01.00)
    - React-Core/RCTVibrationHeaders
    - React-jsi
    - React-NativeModulesApple
    - ReactCodegen
    - ReactCommon
  - React-rendererconsistency (0.76.7)
  - React-rendererdebug (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - RCT-Folly (= 2024.01.01.00)
    - React-debug
  - React-rncore (0.76.7)
  - React-RuntimeApple (0.76.7):
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-callinvoker
    - React-Core/Default
    - React-CoreModules
    - React-cxxreact
    - React-jserrorhandler
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-Mapbuffer
    - React-NativeModulesApple
    - React-RCTFabric
    - React-RuntimeCore
    - React-runtimeexecutor
    - React-RuntimeHermes
    - React-runtimescheduler
    - React-utils
  - React-RuntimeCore (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-cxxreact
    - React-featureflags
    - React-jserrorhandler
    - React-jsi
    - React-jsiexecutor
    - React-jsinspector
    - React-performancetimeline
    - React-runtimeexecutor
    - React-runtimescheduler
    - React-utils
  - React-runtimeexecutor (0.76.7):
    - React-jsi (= 0.76.7)
  - React-RuntimeHermes (0.76.7):
    - hermes-engine
    - RCT-Folly/Fabric (= 2024.01.01.00)
    - React-featureflags
    - React-hermes
    - React-jsi
    - React-jsinspector
    - React-jsitracing
    - React-nativeconfig
    - React-RuntimeCore
    - React-utils
  - React-runtimescheduler (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-callinvoker
    - React-cxxreact
    - React-debug
    - React-featureflags
    - React-jsi
    - React-performancetimeline
    - React-rendererconsistency
    - React-rendererdebug
    - React-runtimeexecutor
    - React-timing
    - React-utils
  - React-timing (0.76.7)
  - React-utils (0.76.7):
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-debug
    - React-jsi (= 0.76.7)
  - ReactCodegen (0.76.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-FabricImage
    - React-featureflags
    - React-graphics
    - React-jsi
    - React-jsiexecutor
    - React-NativeModulesApple
    - React-rendererdebug
    - React-utils
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
  - ReactCommon (0.76.7):
    - ReactCommon/turbomodule (= 0.76.7)
  - ReactCommon/turbomodule (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-callinvoker (= 0.76.7)
    - React-cxxreact (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-logger (= 0.76.7)
    - React-perflogger (= 0.76.7)
    - ReactCommon/turbomodule/bridging (= 0.76.7)
    - ReactCommon/turbomodule/core (= 0.76.7)
  - ReactCommon/turbomodule/bridging (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-callinvoker (= 0.76.7)
    - React-cxxreact (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-logger (= 0.76.7)
    - React-perflogger (= 0.76.7)
  - ReactCommon/turbomodule/core (0.76.7):
    - DoubleConversion
    - fmt (= 9.1.0)
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - React-callinvoker (= 0.76.7)
    - React-cxxreact (= 0.76.7)
    - React-debug (= 0.76.7)
    - React-featureflags (= 0.76.7)
    - React-jsi (= 0.76.7)
    - React-logger (= 0.76.7)
    - React-perflogger (= 0.76.7)
    - React-utils (= 0.76.7)
  - RNCAsyncStorage (1.23.1):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - RNGestureHandler (2.20.2):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - RNReanimated (3.16.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - RNReanimated/reanimated (= 3.16.7)
    - RNReanimated/worklets (= 3.16.7)
    - Yoga
  - RNReanimated/reanimated (3.16.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - RNReanimated/reanimated/apple (= 3.16.7)
    - Yoga
  - RNReanimated/reanimated/apple (3.16.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - RNReanimated/worklets (3.16.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - RNScreens (4.4.0):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-RCTImage
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - RNScreens/common (= 4.4.0)
    - Yoga
  - RNScreens/common (4.4.0):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-RCTImage
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - Yoga
  - SocketRocket (0.7.1)
  - Superscript (0.1.16)
  - superwall-react-native (1.4.7):
    - DoubleConversion
    - glog
    - hermes-engine
    - RCT-Folly (= 2024.01.01.00)
    - RCTRequired
    - RCTTypeSafety
    - React-Core
    - React-debug
    - React-Fabric
    - React-featureflags
    - React-graphics
    - React-ImageManager
    - React-NativeModulesApple
    - React-RCTFabric
    - React-rendererdebug
    - React-utils
    - ReactCodegen
    - ReactCommon/turbomodule/bridging
    - ReactCommon/turbomodule/core
    - SuperwallKit (= 3.12.4)
    - Yoga
  - SuperwallKit (3.12.4):
    - Superscript (= 0.1.16)
  - Yoga (0.0.0)

DEPENDENCIES:
  - boost (from `../node_modules/react-native/third-party-podspecs/boost.podspec`)
  - DoubleConversion (from `../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec`)
  - EXConstants (from `../node_modules/expo-constants/ios`)
  - EXJSONUtils (from `../node_modules/expo-json-utils/ios`)
  - EXManifests (from `../node_modules/expo-manifests/ios`)
  - Expo (from `../node_modules/expo`)
  - expo-dev-client (from `../node_modules/expo-dev-client/ios`)
  - expo-dev-launcher (from `../node_modules/expo-dev-launcher`)
  - expo-dev-menu (from `../node_modules/expo-dev-menu`)
  - expo-dev-menu-interface (from `../node_modules/expo-dev-menu-interface/ios`)
  - ExpoAsset (from `../node_modules/expo-asset/ios`)
  - ExpoBlur (from `../node_modules/expo-blur/ios`)
  - ExpoFileSystem (from `../node_modules/expo-file-system/ios`)
  - ExpoFont (from `../node_modules/expo-font/ios`)
  - ExpoHaptics (from `../node_modules/expo-haptics/ios`)
  - ExpoHead (from `../node_modules/expo-router/ios`)
  - ExpoKeepAwake (from `../node_modules/expo-keep-awake/ios`)
  - ExpoLinking (from `../node_modules/expo-linking/ios`)
  - ExpoModulesCore (from `../node_modules/expo-modules-core`)
  - ExpoSplashScreen (from `../node_modules/expo-splash-screen/ios`)
  - ExpoSymbols (from `../node_modules/expo-symbols/ios`)
  - ExpoSystemUI (from `../node_modules/expo-system-ui/ios`)
  - ExpoWebBrowser (from `../node_modules/expo-web-browser/ios`)
  - EXUpdatesInterface (from `../node_modules/expo-updates-interface/ios`)
  - FBLazyVector (from `../node_modules/react-native/Libraries/FBLazyVector`)
  - fmt (from `../node_modules/react-native/third-party-podspecs/fmt.podspec`)
  - glog (from `../node_modules/react-native/third-party-podspecs/glog.podspec`)
  - hermes-engine (from `../node_modules/react-native/sdks/hermes-engine/hermes-engine.podspec`)
  - RCT-Folly (from `../node_modules/react-native/third-party-podspecs/RCT-Folly.podspec`)
  - RCT-Folly/Fabric (from `../node_modules/react-native/third-party-podspecs/RCT-Folly.podspec`)
  - RCTDeprecation (from `../node_modules/react-native/ReactApple/Libraries/RCTFoundation/RCTDeprecation`)
  - RCTRequired (from `../node_modules/react-native/Libraries/Required`)
  - RCTTypeSafety (from `../node_modules/react-native/Libraries/TypeSafety`)
  - React (from `../node_modules/react-native/`)
  - React-callinvoker (from `../node_modules/react-native/ReactCommon/callinvoker`)
  - React-Core (from `../node_modules/react-native/`)
  - React-Core/RCTWebSocket (from `../node_modules/react-native/`)
  - React-CoreModules (from `../node_modules/react-native/React/CoreModules`)
  - React-cxxreact (from `../node_modules/react-native/ReactCommon/cxxreact`)
  - React-debug (from `../node_modules/react-native/ReactCommon/react/debug`)
  - React-defaultsnativemodule (from `../node_modules/react-native/ReactCommon/react/nativemodule/defaults`)
  - React-domnativemodule (from `../node_modules/react-native/ReactCommon/react/nativemodule/dom`)
  - React-Fabric (from `../node_modules/react-native/ReactCommon`)
  - React-FabricComponents (from `../node_modules/react-native/ReactCommon`)
  - React-FabricImage (from `../node_modules/react-native/ReactCommon`)
  - React-featureflags (from `../node_modules/react-native/ReactCommon/react/featureflags`)
  - React-featureflagsnativemodule (from `../node_modules/react-native/ReactCommon/react/nativemodule/featureflags`)
  - React-graphics (from `../node_modules/react-native/ReactCommon/react/renderer/graphics`)
  - React-hermes (from `../node_modules/react-native/ReactCommon/hermes`)
  - React-idlecallbacksnativemodule (from `../node_modules/react-native/ReactCommon/react/nativemodule/idlecallbacks`)
  - React-ImageManager (from `../node_modules/react-native/ReactCommon/react/renderer/imagemanager/platform/ios`)
  - React-jserrorhandler (from `../node_modules/react-native/ReactCommon/jserrorhandler`)
  - React-jsi (from `../node_modules/react-native/ReactCommon/jsi`)
  - React-jsiexecutor (from `../node_modules/react-native/ReactCommon/jsiexecutor`)
  - React-jsinspector (from `../node_modules/react-native/ReactCommon/jsinspector-modern`)
  - React-jsitracing (from `../node_modules/react-native/ReactCommon/hermes/executor/`)
  - React-logger (from `../node_modules/react-native/ReactCommon/logger`)
  - React-Mapbuffer (from `../node_modules/react-native/ReactCommon`)
  - React-microtasksnativemodule (from `../node_modules/react-native/ReactCommon/react/nativemodule/microtasks`)
  - react-native-safe-area-context (from `../node_modules/react-native-safe-area-context`)
  - react-native-webview (from `../node_modules/react-native-webview`)
  - React-nativeconfig (from `../node_modules/react-native/ReactCommon`)
  - React-NativeModulesApple (from `../node_modules/react-native/ReactCommon/react/nativemodule/core/platform/ios`)
  - React-perflogger (from `../node_modules/react-native/ReactCommon/reactperflogger`)
  - React-performancetimeline (from `../node_modules/react-native/ReactCommon/react/performance/timeline`)
  - React-RCTActionSheet (from `../node_modules/react-native/Libraries/ActionSheetIOS`)
  - React-RCTAnimation (from `../node_modules/react-native/Libraries/NativeAnimation`)
  - React-RCTAppDelegate (from `../node_modules/react-native/Libraries/AppDelegate`)
  - React-RCTBlob (from `../node_modules/react-native/Libraries/Blob`)
  - React-RCTFabric (from `../node_modules/react-native/React`)
  - React-RCTImage (from `../node_modules/react-native/Libraries/Image`)
  - React-RCTLinking (from `../node_modules/react-native/Libraries/LinkingIOS`)
  - React-RCTNetwork (from `../node_modules/react-native/Libraries/Network`)
  - React-RCTSettings (from `../node_modules/react-native/Libraries/Settings`)
  - React-RCTText (from `../node_modules/react-native/Libraries/Text`)
  - React-RCTVibration (from `../node_modules/react-native/Libraries/Vibration`)
  - React-rendererconsistency (from `../node_modules/react-native/ReactCommon/react/renderer/consistency`)
  - React-rendererdebug (from `../node_modules/react-native/ReactCommon/react/renderer/debug`)
  - React-rncore (from `../node_modules/react-native/ReactCommon`)
  - React-RuntimeApple (from `../node_modules/react-native/ReactCommon/react/runtime/platform/ios`)
  - React-RuntimeCore (from `../node_modules/react-native/ReactCommon/react/runtime`)
  - React-runtimeexecutor (from `../node_modules/react-native/ReactCommon/runtimeexecutor`)
  - React-RuntimeHermes (from `../node_modules/react-native/ReactCommon/react/runtime`)
  - React-runtimescheduler (from `../node_modules/react-native/ReactCommon/react/renderer/runtimescheduler`)
  - React-timing (from `../node_modules/react-native/ReactCommon/react/timing`)
  - React-utils (from `../node_modules/react-native/ReactCommon/react/utils`)
  - ReactCodegen (from `build/generated/ios`)
  - ReactCommon/turbomodule/core (from `../node_modules/react-native/ReactCommon`)
  - "RNCAsyncStorage (from `../node_modules/@react-native-async-storage/async-storage`)"
  - RNGestureHandler (from `../node_modules/react-native-gesture-handler`)
  - RNReanimated (from `../node_modules/react-native-reanimated`)
  - RNScreens (from `../node_modules/react-native-screens`)
  - "superwall-react-native (from `../node_modules/@superwall/react-native-superwall`)"
  - Yoga (from `../node_modules/react-native/ReactCommon/yoga`)

SPEC REPOS:
  trunk:
    - SocketRocket
    - Superscript
    - SuperwallKit

EXTERNAL SOURCES:
  boost:
    :podspec: "../node_modules/react-native/third-party-podspecs/boost.podspec"
  DoubleConversion:
    :podspec: "../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec"
  EXConstants:
    :path: "../node_modules/expo-constants/ios"
  EXJSONUtils:
    :path: "../node_modules/expo-json-utils/ios"
  EXManifests:
    :path: "../node_modules/expo-manifests/ios"
  Expo:
    :path: "../node_modules/expo"
  expo-dev-client:
    :path: "../node_modules/expo-dev-client/ios"
  expo-dev-launcher:
    :path: "../node_modules/expo-dev-launcher"
  expo-dev-menu:
    :path: "../node_modules/expo-dev-menu"
  expo-dev-menu-interface:
    :path: "../node_modules/expo-dev-menu-interface/ios"
  ExpoAsset:
    :path: "../node_modules/expo-asset/ios"
  ExpoBlur:
    :path: "../node_modules/expo-blur/ios"
  ExpoFileSystem:
    :path: "../node_modules/expo-file-system/ios"
  ExpoFont:
    :path: "../node_modules/expo-font/ios"
  ExpoHaptics:
    :path: "../node_modules/expo-haptics/ios"
  ExpoHead:
    :path: "../node_modules/expo-router/ios"
  ExpoKeepAwake:
    :path: "../node_modules/expo-keep-awake/ios"
  ExpoLinking:
    :path: "../node_modules/expo-linking/ios"
  ExpoModulesCore:
    :path: "../node_modules/expo-modules-core"
  ExpoSplashScreen:
    :path: "../node_modules/expo-splash-screen/ios"
  ExpoSymbols:
    :path: "../node_modules/expo-symbols/ios"
  ExpoSystemUI:
    :path: "../node_modules/expo-system-ui/ios"
  ExpoWebBrowser:
    :path: "../node_modules/expo-web-browser/ios"
  EXUpdatesInterface:
    :path: "../node_modules/expo-updates-interface/ios"
  FBLazyVector:
    :path: "../node_modules/react-native/Libraries/FBLazyVector"
  fmt:
    :podspec: "../node_modules/react-native/third-party-podspecs/fmt.podspec"
  glog:
    :podspec: "../node_modules/react-native/third-party-podspecs/glog.podspec"
  hermes-engine:
    :podspec: "../node_modules/react-native/sdks/hermes-engine/hermes-engine.podspec"
    :tag: hermes-2024-11-12-RNv0.76.2-5b4aa20c719830dcf5684832b89a6edb95ac3d64
  RCT-Folly:
    :podspec: "../node_modules/react-native/third-party-podspecs/RCT-Folly.podspec"
  RCTDeprecation:
    :path: "../node_modules/react-native/ReactApple/Libraries/RCTFoundation/RCTDeprecation"
  RCTRequired:
    :path: "../node_modules/react-native/Libraries/Required"
  RCTTypeSafety:
    :path: "../node_modules/react-native/Libraries/TypeSafety"
  React:
    :path: "../node_modules/react-native/"
  React-callinvoker:
    :path: "../node_modules/react-native/ReactCommon/callinvoker"
  React-Core:
    :path: "../node_modules/react-native/"
  React-CoreModules:
    :path: "../node_modules/react-native/React/CoreModules"
  React-cxxreact:
    :path: "../node_modules/react-native/ReactCommon/cxxreact"
  React-debug:
    :path: "../node_modules/react-native/ReactCommon/react/debug"
  React-defaultsnativemodule:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/defaults"
  React-domnativemodule:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/dom"
  React-Fabric:
    :path: "../node_modules/react-native/ReactCommon"
  React-FabricComponents:
    :path: "../node_modules/react-native/ReactCommon"
  React-FabricImage:
    :path: "../node_modules/react-native/ReactCommon"
  React-featureflags:
    :path: "../node_modules/react-native/ReactCommon/react/featureflags"
  React-featureflagsnativemodule:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/featureflags"
  React-graphics:
    :path: "../node_modules/react-native/ReactCommon/react/renderer/graphics"
  React-hermes:
    :path: "../node_modules/react-native/ReactCommon/hermes"
  React-idlecallbacksnativemodule:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/idlecallbacks"
  React-ImageManager:
    :path: "../node_modules/react-native/ReactCommon/react/renderer/imagemanager/platform/ios"
  React-jserrorhandler:
    :path: "../node_modules/react-native/ReactCommon/jserrorhandler"
  React-jsi:
    :path: "../node_modules/react-native/ReactCommon/jsi"
  React-jsiexecutor:
    :path: "../node_modules/react-native/ReactCommon/jsiexecutor"
  React-jsinspector:
    :path: "../node_modules/react-native/ReactCommon/jsinspector-modern"
  React-jsitracing:
    :path: "../node_modules/react-native/ReactCommon/hermes/executor/"
  React-logger:
    :path: "../node_modules/react-native/ReactCommon/logger"
  React-Mapbuffer:
    :path: "../node_modules/react-native/ReactCommon"
  React-microtasksnativemodule:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/microtasks"
  react-native-safe-area-context:
    :path: "../node_modules/react-native-safe-area-context"
  react-native-webview:
    :path: "../node_modules/react-native-webview"
  React-nativeconfig:
    :path: "../node_modules/react-native/ReactCommon"
  React-NativeModulesApple:
    :path: "../node_modules/react-native/ReactCommon/react/nativemodule/core/platform/ios"
  React-perflogger:
    :path: "../node_modules/react-native/ReactCommon/reactperflogger"
  React-performancetimeline:
    :path: "../node_modules/react-native/ReactCommon/react/performance/timeline"
  React-RCTActionSheet:
    :path: "../node_modules/react-native/Libraries/ActionSheetIOS"
  React-RCTAnimation:
    :path: "../node_modules/react-native/Libraries/NativeAnimation"
  React-RCTAppDelegate:
    :path: "../node_modules/react-native/Libraries/AppDelegate"
  React-RCTBlob:
    :path: "../node_modules/react-native/Libraries/Blob"
  React-RCTFabric:
    :path: "../node_modules/react-native/React"
  React-RCTImage:
    :path: "../node_modules/react-native/Libraries/Image"
  React-RCTLinking:
    :path: "../node_modules/react-native/Libraries/LinkingIOS"
  React-RCTNetwork:
    :path: "../node_modules/react-native/Libraries/Network"
  React-RCTSettings:
    :path: "../node_modules/react-native/Libraries/Settings"
  React-RCTText:
    :path: "../node_modules/react-native/Libraries/Text"
  React-RCTVibration:
    :path: "../node_modules/react-native/Libraries/Vibration"
  React-rendererconsistency:
    :path: "../node_modules/react-native/ReactCommon/react/renderer/consistency"
  React-rendererdebug:
    :path: "../node_modules/react-native/ReactCommon/react/renderer/debug"
  React-rncore:
    :path: "../node_modules/react-native/ReactCommon"
  React-RuntimeApple:
    :path: "../node_modules/react-native/ReactCommon/react/runtime/platform/ios"
  React-RuntimeCore:
    :path: "../node_modules/react-native/ReactCommon/react/runtime"
  React-runtimeexecutor:
    :path: "../node_modules/react-native/ReactCommon/runtimeexecutor"
  React-RuntimeHermes:
    :path: "../node_modules/react-native/ReactCommon/react/runtime"
  React-runtimescheduler:
    :path: "../node_modules/react-native/ReactCommon/react/renderer/runtimescheduler"
  React-timing:
    :path: "../node_modules/react-native/ReactCommon/react/timing"
  React-utils:
    :path: "../node_modules/react-native/ReactCommon/react/utils"
  ReactCodegen:
    :path: build/generated/ios
  ReactCommon:
    :path: "../node_modules/react-native/ReactCommon"
  RNCAsyncStorage:
    :path: "../node_modules/@react-native-async-storage/async-storage"
  RNGestureHandler:
    :path: "../node_modules/react-native-gesture-handler"
  RNReanimated:
    :path: "../node_modules/react-native-reanimated"
  RNScreens:
    :path: "../node_modules/react-native-screens"
  superwall-react-native:
    :path: "../node_modules/@superwall/react-native-superwall"
  Yoga:
    :path: "../node_modules/react-native/ReactCommon/yoga"

SPEC CHECKSUMS:
  boost: 1dca942403ed9342f98334bf4c3621f011aa7946
  DoubleConversion: f16ae600a246532c4020132d54af21d0ddb2a385
  EXConstants: 9b47f3f07f01c5afaa23b1fc9e49bb87f70e3576
  EXJSONUtils: 01fc7492b66c234e395dcffdd5f53439c5c29c93
  EXManifests: b1586049a30ba7ec9ca7ec9afbd4cc233c4b2316
  Expo: 684b9146eabc19a5aa6994f8d6b9131f6facbb3b
  expo-dev-client: be653fa8a57337215e8557c07ddced0e8c576af6
  expo-dev-launcher: e791741ba46eafa8db5fc72c387ffee208bdb29c
  expo-dev-menu: 3bdb3a7eda40823ef01975ef39bc319aa068b483
  expo-dev-menu-interface: 00dc42302a72722fdecec3fa048de84a9133bcc4
  ExpoAsset: 77e4579f19b48d6b3f4055c933e86ce45e80caeb
  ExpoBlur: 567af66164e3043a9a30069594aed1ddf0a88d97
  ExpoFileSystem: dfca98dde30460788cb9f6f51f177888fa021934
  ExpoFont: 8b14db1afebf62e9774613a082e402985f4e747c
  ExpoHaptics: e01cce0741d68c281853118eb0267f88d42c6b7a
  ExpoHead: 3d555eb5d0d68c478b9ea259337686c58de22c71
  ExpoKeepAwake: 1f74cdf53ab3b3aeb7110f18eb053c92e7c3e710
  ExpoLinking: 0381341519ca7180a3a057d20edb1cf6a908aaf4
  ExpoModulesCore: 561a787bc626ab9bcfee2a688bd588bf2667405f
  ExpoSplashScreen: 643666b96a84e8d97d55fac43926fb4183c692d9
  ExpoSymbols: b9f255ce49868d46a73f30e12859efeb8117bcad
  ExpoSystemUI: ba4507df7d8d15f5e1694a3c7fc6bc3cca3803e5
  ExpoWebBrowser: 6890a769e6c9d83da938dceb9a03e764afc3ec9c
  EXUpdatesInterface: 1dcebac98ac5dad4289e6ff2bd5616822e894397
  FBLazyVector: ca8044c9df513671c85167838b4188791b6f37e1
  fmt: 10c6e61f4be25dc963c36bd73fc7b1705fe975be
  glog: 08b301085f15bcbb6ff8632a8ebaf239aae04e6a
  hermes-engine: eb4a80f6bf578536c58a44198ec93a30f6e69218
  RCT-Folly: bf5c0376ffe4dd2cf438dcf86db385df9fdce648
  RCTDeprecation: 7691283dd69fed46f6653d376de6fa83aaad774c
  RCTRequired: eac044a04629288f272ee6706e31f81f3a2b4bfe
  RCTTypeSafety: cfe499e127eda6dd46e5080e12d80d0bfe667228
  React: 1f3737a983fdd26fb3d388ddbca41a26950fe929
  React-callinvoker: 5c15ac628eab5468fe0b4dc453495f4742761f00
  React-Core: 4b90a977a5b2777fd8f4a8db7325a83431ecd2d8
  React-CoreModules: 385bbacfa34ac9208aa24f239a5184fa7ab1cd28
  React-cxxreact: 3e09bcdf1f86b931b5e96bf5429d7c274a0ec168
  React-debug: 2086b55a5e55fb0abae58c42b8f280ebd708c956
  React-defaultsnativemodule: 491e2541856e3580dae7f29d80754673a2134e48
  React-domnativemodule: 4aaed5d5eef3da7d7d49b1f2ae8f422a4d7794b7
  React-Fabric: 5b8373d1bd34bf269b13529a0ebee0643165ccf8
  React-FabricComponents: 3f8528c3ed060464a120e161ffaef9307a88817b
  React-FabricImage: 8efa4e206b1e5cf2e8e1e48fd345619c5c0484f4
  React-featureflags: 4503c901bf16b267b689e8a1aed24e951e0b091b
  React-featureflagsnativemodule: 415168f5d23413fd0cc55ad98c41a3f3f135b2a7
  React-graphics: c619a6e974baf9a7dbae8442944c7b7408391d46
  React-hermes: 24bfc254f1ba83182d4936641898fe963af343fb
  React-idlecallbacksnativemodule: 2c2e4c3f561a98c84a7a68c0d1f868b64ca5f839
  React-ImageManager: ba9c89729be310413c610444a658fac505253d2c
  React-jserrorhandler: bf16ea495377b22223bf93f3ef6d0711b9852613
  React-jsi: ede7e8c96f997f8772871c82688cea53c1ffb148
  React-jsiexecutor: fc9b287189ce800a92d5ab4e7291508eacaab451
  React-jsinspector: fa5e8b22102b599c2bb2aeafebbf957a1ab836da
  React-jsitracing: f38c15aeb910bafcf3ba2e24af8c92e6af4ce1d4
  React-logger: f9d104eace4ce03d7d5ab96802069d9905082225
  React-Mapbuffer: 23ffe602d0f5ca53b861ef8534cb8c63c4479671
  React-microtasksnativemodule: 73fdf0c53b6d50d55de2d5bd9abfb8c006b043a4
  react-native-safe-area-context: 458f6b948437afcb59198016b26bbd02ff9c3b47
  react-native-webview: 40b8823be3fac70f0404016e6aed754ef4307517
  React-nativeconfig: 67fa7a63ea288cb5b1d0dd2deaf240405fec164f
  React-NativeModulesApple: cbf1a34443e1f67b56344547f4b0af69e1c685ba
  React-perflogger: f02ee21d98773121d77993b3c1a8be445840fae3
  React-performancetimeline: 7021d68884291b649b4c39ecb71e0fd3a2e53a59
  React-RCTActionSheet: ad84d5a0bd1ad1782f0b78b280c6f329ad79a53a
  React-RCTAnimation: 388460f7c124c76e337c6646738a83d6ea147095
  React-RCTAppDelegate: 4661e2a44f7ce1033bf6f373f7d5368b11f5a2be
  React-RCTBlob: 07cccbb74e22ce66745358799f6ab02a5bed2993
  React-RCTFabric: 77ebcd07a3c1f3d4c2d2f67f69033a65d16a36a8
  React-RCTImage: 8fbdae841ea1217c44f4c413bba2403134b83cd1
  React-RCTLinking: c59bf8286ba2cc327b01bb524fb9c16446dc18bc
  React-RCTNetwork: 2c137a0aaaed2cf4bb53aff82a2bb8c34f2fbeac
  React-RCTSettings: 9fcd32c5b38af6421a3dd20cdd9ebf09df0a9a6d
  React-RCTText: 5308618477fec454282809065bd121c2bd3dd5e1
  React-RCTVibration: 7b2a186756b5c8e586e3e7948eed4432a93299c0
  React-rendererconsistency: 65d4692825fda4d9516924b68c29d0f28da3158c
  React-rendererdebug: 0b97f49d44c91862e1576961faf6bde836ed4eb3
  React-rncore: 6aca111c05a48c58189a005cb10a7b52780738dc
  React-RuntimeApple: aa20633298595444bf2dfbc5246889b4f475b871
  React-RuntimeCore: 8ac56cc6d82a1090f1d15d48b487c9a5a1d7d915
  React-runtimeexecutor: 732038d7c356ba74132f1d16253a410621d3c2c1
  React-RuntimeHermes: a695d944686adc97f85a1b34c31840a0a39e356c
  React-runtimescheduler: 00666e100e35a13f28fb2fdab22817cf62bbd6a3
  React-timing: c2915214b94a62bdf77d2965c31f76bc25b362a5
  React-utils: 9f9a6a31d703b136eb1614d914c10a3c96b1e6dd
  ReactCodegen: 9a99ced51aab02909c7ab5056f33dff7054305bb
  ReactCommon: 04292c6f596181ebf755e7929d96d2148542b0e8
  RNCAsyncStorage: a927b768986f83467b635cf6d7559e6edb46db7a
  RNGestureHandler: fc5ce5bf284640d3af6431c3a5c3bc121e98d045
  RNReanimated: 9ee6347ca0aa3cf78cae715455e781728ae142e2
  RNScreens: d022507f2b6d76c73335e9e35aedcf7bb2f791b0
  SocketRocket: d4aabe649be1e368d1318fdf28a022d714d65748
  Superscript: 17e2597de5e1ddfa132e217b33d1eb8eddf13e0f
  superwall-react-native: 0c0ab78844acd6cc2f6944f9f7041f5b81b08864
  SuperwallKit: e89ac4f4093ed4135aa4876d506bec443cb933f2
  Yoga: 90d80701b27946c4b23461c00a7207f300a6ff71

PODFILE CHECKSUM: ac32857d9cc1f445437085fa069222add4a5dc94

COCOAPODS: 1.16.2

```

# ios/Podfile.properties.json

```json
{
  "expo.jsEngine": "hermes",
  "EX_DEV_CLIENT_NETWORK_INSPECTOR": "true",
  "newArchEnabled": "true"
}

```

# package.json

```json
{
  "name": "expo-app-boilerplate",
  "main": "expo-router/entry",
  "version": "1.0.0",
  "scripts": {
    "start": "expo start",
    "reset-project": "node ./scripts/reset-project.js",
    "android": "expo run:android",
    "ios": "expo run:ios",
    "web": "expo start --web",
    "test": "jest --watchAll",
    "lint": "expo lint"
  },
  "jest": {
    "preset": "jest-expo"
  },
  "dependencies": {
    "@expo/vector-icons": "^14.0.2",
    "@react-navigation/bottom-tabs": "^7.2.0",
    "@react-navigation/native": "^7.0.14",
    "@superwall/react-native-superwall": "^1.4.7",
    "expo": "~52.0.35",
    "expo-blur": "~14.0.3",
    "expo-constants": "~17.0.6",
    "expo-dev-client": "~5.0.12",
    "expo-font": "~13.0.3",
    "expo-haptics": "~14.0.1",
    "expo-linking": "~7.0.5",
    "expo-router": "~4.0.17",
    "expo-splash-screen": "~0.29.22",
    "expo-status-bar": "~2.0.1",
    "expo-symbols": "~0.2.2",
    "expo-system-ui": "~4.0.8",
    "expo-web-browser": "~14.0.2",
    "react": "18.3.1",
    "react-dom": "18.3.1",
    "react-native": "0.76.7",
    "react-native-gesture-handler": "~2.20.2",
    "react-native-reanimated": "~3.16.1",
    "react-native-safe-area-context": "4.12.0",
    "react-native-screens": "~4.4.0",
    "react-native-web": "~0.19.13",
    "react-native-webview": "13.12.5",
    "@react-native-async-storage/async-storage": "1.23.1"
  },
  "devDependencies": {
    "@babel/core": "^7.25.2",
    "@types/jest": "^29.5.12",
    "@types/react": "~18.3.12",
    "@types/react-native": "^0.72.8",
    "@types/react-test-renderer": "^18.3.0",
    "jest": "^29.2.1",
    "jest-expo": "~52.0.4",
    "react-test-renderer": "18.3.1",
    "typescript": "^5.3.3"
  },
  "private": true
}

```

# README.md

```md
# Welcome to jack's Expo React Native free boilerplate 👋

This is an [Expo](https://expo.dev) template project with Superwall libraries ready to use and a simple onboarding sequence for first time users.

This free boilerplate is sponsored by [post bridge](https://post-bridge.com) - a super simple and affordable social media scheduling tool for small teams and founders.

## Get started

1. Clone this repository 

2. Install dependencies

   \`\`\`bash
   npm install
   \`\`\`
Or 

  \`\`\`bash
   npx expo install
   \`\`\`

3. Start the app

   \`\`\`bash
    npx expo start
   \`\`\`
-- you will need to make a development build or run in development mode as Superwall does not work in Expo GO

In the output, you'll find options to open the app in a

- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo

You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).

## Need help?

Join [the discord](https://discord.gg/XuT2V5GUkA) for app founders and @jackfriks for help.

## Learn more

To learn more about developing your project with Expo, look at the following resources:

- [Expo documentation](https://docs.expo.dev/): Learn fundamentals, or go into advanced topics with our [guides](https://docs.expo.dev/guides).
- [Learn Expo tutorial](https://docs.expo.dev/tutorial/introduction/): Follow a step-by-step tutorial where you'll create a project that runs on Android, iOS, and the web.

```

# scripts/reset-project.js

```js
#!/usr/bin/env node

/**
 * This script is used to reset the project to a blank state.
 * It deletes or moves the /app, /components, /hooks, /scripts, and /constants directories to /app-example based on user input and creates a new /app directory with an index.tsx and _layout.tsx file.
 * You can remove the `reset-project` script from package.json and safely delete this file after running it.
 */

const fs = require("fs");
const path = require("path");
const readline = require("readline");

const root = process.cwd();
const oldDirs = ["app", "components", "hooks", "constants", "scripts"];
const exampleDir = "app-example";
const newAppDir = "app";
const exampleDirPath = path.join(root, exampleDir);

const indexContent = `import { Text, View } from "react-native";

export default function Index() {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      <Text>Edit app/index.tsx to edit this screen.</Text>
    </View>
  );
}
`;

const layoutContent = `import { Stack } from "expo-router";

export default function RootLayout() {
  return <Stack />;
}
`;

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const moveDirectories = async (userInput) => {
  try {
    if (userInput === "y") {
      // Create the app-example directory
      await fs.promises.mkdir(exampleDirPath, { recursive: true });
      console.log(`📁 /${exampleDir} directory created.`);
    }

    // Move old directories to new app-example directory or delete them
    for (const dir of oldDirs) {
      const oldDirPath = path.join(root, dir);
      if (fs.existsSync(oldDirPath)) {
        if (userInput === "y") {
          const newDirPath = path.join(root, exampleDir, dir);
          await fs.promises.rename(oldDirPath, newDirPath);
          console.log(`➡️ /${dir} moved to /${exampleDir}/${dir}.`);
        } else {
          await fs.promises.rm(oldDirPath, { recursive: true, force: true });
          console.log(`❌ /${dir} deleted.`);
        }
      } else {
        console.log(`➡️ /${dir} does not exist, skipping.`);
      }
    }

    // Create new /app directory
    const newAppDirPath = path.join(root, newAppDir);
    await fs.promises.mkdir(newAppDirPath, { recursive: true });
    console.log("\n📁 New /app directory created.");

    // Create index.tsx
    const indexPath = path.join(newAppDirPath, "index.tsx");
    await fs.promises.writeFile(indexPath, indexContent);
    console.log("📄 app/index.tsx created.");

    // Create _layout.tsx
    const layoutPath = path.join(newAppDirPath, "_layout.tsx");
    await fs.promises.writeFile(layoutPath, layoutContent);
    console.log("📄 app/_layout.tsx created.");

    console.log("\n✅ Project reset complete. Next steps:");
    console.log(
      `1. Run \`npx expo start\` to start a development server.\n2. Edit app/index.tsx to edit the main screen.${
        userInput === "y"
          ? `\n3. Delete the /${exampleDir} directory when you're done referencing it.`
          : ""
      }`
    );
  } catch (error) {
    console.error(`❌ Error during script execution: ${error.message}`);
  }
};

rl.question(
  "Do you want to move existing files to /app-example instead of deleting them? (Y/n): ",
  (answer) => {
    const userInput = answer.trim().toLowerCase() || "y";
    if (userInput === "y" || userInput === "n") {
      moveDirectories(userInput).finally(() => rl.close());
    } else {
      console.log("❌ Invalid input. Please enter 'Y' or 'N'.");
      rl.close();
    }
  }
);

```

# services/superwall.ts

```ts
import { Platform } from 'react-native';
import Superwall, { SubscriptionStatus } from '@superwall/react-native-superwall';
import { createSuperwallConfig } from '@/config/superwall';

class SuperwallService {
  private static instance: SuperwallService;
  private initialized = false;

  private constructor() {}

  static getInstance(): SuperwallService {
    if (!SuperwallService.instance) {
      SuperwallService.instance = new SuperwallService();
    }
    return SuperwallService.instance;
  }

  initialize() {
    if (this.initialized) return;

    const apiKey = Platform.select({
      ios: process.env.EXPO_PUBLIC_SUPERWALL_API_KEY_IOS,
      android: process.env.EXPO_PUBLIC_SUPERWALL_API_KEY_ANDROID,
      default: undefined,
    });

    if (!apiKey) {
      console.warn('[Superwall] No API key found for platform:', Platform.OS);
      return;
    }

    try {
      const options = createSuperwallConfig();
      Superwall.configure(apiKey, options);
      this.initialized = true;
      console.log('[Superwall] Initialized successfully');
    } catch (error) {
      console.error('[Superwall] Initialization failed:', error);
    }
  }

  async presentPaywall(triggerId: string): Promise<void> {
    try {
      console.log('[Superwall] Presenting paywall for trigger:', triggerId);
      await Superwall.shared.register(triggerId);
    } catch (error) {
      console.error('[Superwall] Failed to present paywall:', error);
      throw error;
    }
  }

  async getSubscriptionStatus(): Promise<SubscriptionStatus> {
    try {
      const status = await Superwall.shared.getSubscriptionStatus();
      console.log('[Superwall] Subscription status:', status);
      return status;
    } catch (error) {
      console.error('[Superwall] Failed to get subscription status:', error);
      throw error;
    }
  }
}

export const superwallService = SuperwallService.getInstance(); 
```

# tsconfig.json

```json
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "paths": {
      "@/*": ["./*"],
      "@components/*": ["./components/*"],
      "@utils/*": ["./utils/*"],
      "@constants/*": ["./constants/*"],
      "@types/*": ["./types/*"]
    },
    "jsx": "react-jsx",
    "baseUrl": ".",
    "types": ["node"]
  },
  "include": [
    "**/*.ts",
    "**/*.tsx",
    ".expo/types/**/*.ts",
    "expo-env.d.ts"
  ]
}

```

# types/env.d.ts

```ts
declare module '@env' {
  export const EXPO_PUBLIC_SUPERWALL_API_KEY_IOS: string;
  export const EXPO_PUBLIC_SUPERWALL_API_KEY_ANDROID: string;
} 
```

# types/superwall.ts

```ts
export type SubscriptionStatus = 'active' | 'inactive' | 'unknown';

export interface SuperwallConfig {
  apiKey: string;
  debugMode?: boolean;
  options?: {
    logging?: boolean;
    paywallTimeout?: number;
  };
} 
```

