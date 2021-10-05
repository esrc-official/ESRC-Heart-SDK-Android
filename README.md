# ESRC Heart SDK for Android

[![Platform](https://img.shields.io/badge/platform-android-orange.svg)](https://github.com/esrc-official/ESRC-Heart-SDK-Android)
[![Languages](https://img.shields.io/badge/language-java-orange.svg)](https://github.com/esrc-official/ESRC-Heart-SDK-Android)
[![Commercial License](https://img.shields.io/badge/license-Commercial-brightgreen.svg)](https://github.com/esrc-official/ESRC-Heart-SDK-Android/blob/master/LICENSE.md)

## Table of contents

  1. [About ESRC Heart SDK](#about-esrc-heart-sdk)
  1. [Install ESRC Heart SDK](#install-esrc-heart-sdk)
  1. [Making your first recognition](#making-your-first-recognition)

<br />

## About ESRC Heart SDK

Through our **ESRC Heart SDK** for Android, you can efficiently integrate real-time recognition of heart response and emotions into your mobile app. This and other pages in the Getting Started provide the ESRC Heart SDK’s structure and installation steps, then goes through the preliminary steps of implementing the ESRC Heart SDK in your own project.

### Requirements

The minimum requirements for ESRC Heart SDK for Android are:

- Android 4.2.0 (API level 17) or later
- Java 8 or later
- Gradle 4.0.0 or later

```groovy
// build.gradle(app)
android {
    defaultConfig {
        minSdkVersion 17
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

<br />

### Key functions

|Function|Description|
|---|---|
|Face Detection| Detect a single face using a front camera on mobile. |
|Heart Rate Estimation| Estimate heart rate from facial color variations and head move-ments caused by heartbeat using Remote Photoplethysmography and Ballistocardiography. |
|Heart Rate Variability Analysis| Extract 19 variables of heart rate variability reflecting autonomic nervous system activity from the accumulated heart rates. |
|Engagement Recognition| Recognize engagement level from balance of autonomic nervous system by heart rate variability analysis. |

<br />

### Try the sample app

Our sample app has the core features of the ESRC HeartSDK. Download the app from our [GitHub repository](https://github.com/esrc-official/ESRC-Heart-Android) to get an idea of what you can build with the actual SDK and start building in your project.

> Note: The fastest way to see our ESRC Heart SDK in action is to build your app on top of our sample app. Make sure to change the application ID of the sample app to your own.


## Install ESRC Heart SDK

This page provides a step-by-step guide that demonstrates how to build and configure an in-app bio-analysis using the ESRC Heart SDK. 

### Step 1: Download and install the third-party libraries (OpenCV, TensorFlow, RxJava)

Installing the OpenCV is simple if you’re familiar with using external libraries or SDKs. First, download the OpenCV `project` files from our [Google repository](https://drive.google.com/drive/folders/1QLduUkf0q2Yg4LwTUrr7EpwENNlea_Ep?usp=sharing).

Then, import the OpenCV `project` files as `opencv-3.4.11 module` in your app. Finally, add the following code to your module `build.gradle` file:
```gradle
allprojects {
	repositories {  
		...    
		maven { url "https://google.bintray.com/tensorflow" }
	}
}
```

> Note: Make sure the above code block isn't added to your module `bundle.gradle` file.

```groovy
dependencies {
    implementation project(path: ':opencv-3.4.11') // OpenCV
    implementation 'org.tensorflow:tensorflow-lite:0.0.0-nightly' // TensorFlow
    implementation 'io.reactivex.rxjava3:rxandroid:3.0.0' // RxJava
    implementation 'io.reactivex.rxjava3:rxjava:3.0.7' // RxJava    
}
```

### Step 2: Install the ESRC Heart SDK

First, copy the ESRC Heart SDK `.aar` file to the `app/libs` folder in your app. Then, add the dependency to your module `build.gradle` file:

```gradle
allprojects {
    packagingOptions {
        pickFirst 'lib/arm64-v8a/*'
        pickFirst 'lib/armeabi-v7a/*'
        pickFirst 'lib/x86/*'
        pickFirst 'lib/x86_64/*'
    }

    repositories {
        ...
        flatDir { dirs 'libs' }
    }
}
```

> Note: Make sure the above code block isn't added to your module `bundle.gradle` file.

```groovy
dependencies {
    implementation name: 'esrc-heart-sdk-2.3.0', ext: 'aar'
}
```

### Step 3: Grant system permissions to the ESRC Heart SDK

The ESRC Heart SDK requires system permissions. These permissions allow the ESRC Heart SDK to use a camera and read from and write on a user device’s storage. To grant system permissions, add the following lines to your `AndroidManifest.xml` file.

```manifest
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

The `CAMERA`, `READ_EXTERNAL_STORAGE` and `WRITE_EXTERNAL_STORAGE` permissions are classified as `dangerous` and require users to grant them explicitly when an app is run for the first time on devices running Android 6.0 or higher.

For more information about requesting app permissions, see  Android’s Request App Permissions [guide](https://developer.android.com/training/permissions/requesting.html).

<br />

## Making your first recognition

The ESRC Heart SDK simplifies vision features into an effortless and straightforward process. To recognize your heart response and emotion, do the following steps:

This page provides a step-by-step guide that demonstrates how to build and configure an in-app bio-analysis using ESRC Heart SDK. License key can be received by requesting by the email: **esrc@esrc.co.kr**.

### Step 1: Initialize the ESRC Heart SDK

Initialization binds the ESRC Heart SDK to Android’s context, thereby allowing it to use a camera in your mobile. To the `init()` method, pass the **App ID** of your ESRC application to initialize the ESRC Heart SDK and the **ESRCLicenseHandler** to received callback for validation of the App ID.

```java
ESRC.init(APP_ID, getApplicationContext(), new ESRCLicense.ESRCLicenseHandler() {
    @Override
    public void onValidatedLicense() {
        …
    }
    
    @Override
    public void onInvalidatedLicense() {
        …
    }
});
```

> Note: The `ESRC.init()` method must be called once across your Android app. It is recommended to initialize the ESRC Heart SDK in the `onCreate()` method of the Application instance.

### (Optional) Step 2: Bind the ESRC Fragment

If you don't want to develop a layout that uses the camera, you can ues the ESRC Fragment provided from the ESRC Heart SDK. Include the **container** to bind the ESRC Fragment in your layout `.xml` file. Please skip the Step 4: Feed the ESRC Heart SDK. The ESRC Fragment will feed the image to our SDK itself.

```xml
<FrameLayout
    android:id=”@+id/container”
    android:layout_width=”match_parent”
    android:layout_height=”match_parent” />
```

> Note: FrameLayout is just one of examples. You can change to other layout type to purpose your app.

Bind the ESRC Fragment to display the image taken with the camera on the screen. ESRC Fragment send the image to the ESRC Heart SDK in real-time to be able to recognize heart response and emotion. ESRC Fragment automatically display the image to fit the size of your custom layout.

```java
// Bind LAYOUT.xml on your Activity.
setContentView(R.layout.LAYOUT);

// Replace the container in LAYOUT as the ESRC Fragment .
getSupportFragmentManager().beginTransaction()
    .replace(R.id.container, ESRCFragment.newInstance())
    .commit();
```

### Step 3: Start the ESRC Heart SDK

Start the ESRC Heart SDK to recognize your heart response and emotion. To the `start()` method, pass the `ENABLE_DRAW` parameter for whether to visualize the face bounding box and the `ESRC.ESRCHandler` to handle the results. You should implement the callback method of `ESRC.ESRCHandler` interface. So, you can receive the results of face, heart rate, heart rate variability and engagement. Please refer to **[sample app](https://github.com/esrc-official/ESRC-Heart-Android)**.

```java
ESRC.start(ENABLE_DRAW, new ESRC.ESRCHandler() {
    @Override
    public void onDetectedFace(ESRCTYPE.Face face, ESRCException e) {
        if(e != null) {
            // Handle error.
        }
        
	// The face is detected.
        // Through the “face” parameter of the onDetectedFace() callback method,
        // you can get the location of the face from the result object
        // that ESRC Heart SDK has passed to the onDetectedFace().
        …
    }
    
    // Please implement other callback method of ESRC.ESRCHandler interface.
    @Override public void onNotDetectedFace( … ) { … }
    @Override public void didChangedProgressRatioOnRemoteHR( … ) { … }
    @Override public void onEstimatedRemoteHR( … ) { … }
    @Override public void didChangedProgressRatioOnHRV( … ) { … }
    @Override public void onAnalyzedHRV( … ) { … }
    @Override public void onRecognizedEngagement( … ) { … }
});
```

### (Optional) Step 4: Feed the ESRC Heart SDK

Feed `OpenCV Mat` on the ESRC Heart SDK. To the `feed()` method, pass the `Mat` image received using a camera in real-time. Please do it at 10 fps. You can skip this step if you follow Step 2: Bind the ESRC Fragment.

```java
ESRC.feed(Mat);
```

### Step 5: Stop the ESRC Heart SDK

When your app is not use the camera or destroyed, stop the ESRC Heart SDK.

```java
ESRC.stop();
```
