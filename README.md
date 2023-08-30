# AsleepSDK Android
# Get Started


# 1. Requirements
## 1.1 Minimum requirements on Asleep SDK for Android
> - Android 8.0 (API level 26) or higher
> - Java 1.8 or higher
> - Android Gradle plugin 8.0 or higher

## 1.2 API Key
The API key is required to use the Asleep SDK.

For how to issue an API key, see this link \[[Generate API key](dashboard-generate-api-key)]

# 2. Getting Ready

## 2.1 Install Asleep SDK and Settings

1. Create a project using Android Studio.
2. Add [\[AsleepSDK.aar\]](android-release-note) file in the [root]/app/libs folder of the generated project folder.
3. Open the AndroidManifest.xml file to add permissions.

```xml AndroidManifest.xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
		...
    
```

> 📘 The REQUEST_IGNORE_BATTERY_OPTIMIZATIONS permission is not required.
> 
> However, it can be helpful if you have the appropriate permissions to prevent you from falling into Doze mode.
> 
> <https://developer.android.com/training/monitoring-device-state/doze-standby>

> 📘 The FOREGROUND_SERVICE permission may be required when developing the app .
> 
> If you want the app you want to develop to work even when the user is sleeping, you must use this feature.
> 
> <https://developer.android.com/guide/components/foreground-services>

4. To add a dependency to your project, specify a dependency configuration such as implementation in the dependencies block of your module's build.gradle file.

```groovy Gradle
plugins {
  ...
}

android {
	...
}

dependencies {
		...
    implementation 'org.jetbrains.bio:npy:0.3.5'
    implementation 'com.squareup.okhttp3:okhttp:4.9.0'
    implementation 'com.google.code.gson:gson:2.9.1'
    implementation fileTree(dir: 'libs', include: ['*.aar', '*.jar'], exclude: [])
}
```

5. Write the source code for obtaining permission in the appropriate location.

```kotlin Kotlin

ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.RECORD_AUDIO), 0)

```

# 3. Sleep Tracking with Asleep SDK

## 3.1 Step 1: Initialize the Asleep SDK

Get AsleepConfig by initAsleepConfig.

Enter apiKey, userId(null if nothing was previously received), baseUrl(address of proxy server, null if not), callbackUrl(address where the session receives the analyzed results), and service(nickname for the app you want to develop).

If you do not have a userId, create a new one and return it from onSuccess(), and if you enter an existing userId, it will be validated and returned by onSuccess().

If the existing userId is in tracking, exit it from inside the SDK.

```kotlin Kotlin
import ai.asleep.asleepsdk.Asleep
import ai.asleep.asleepsdk.data.AsleepConfig
import ai.asleep.asleepsdk.AsleepErrorCode

...

Asleep.initAsleepConfig(  
    context = applicationContext,  
    apiKey = "[input your apiKey]",  
    userId = "",  
    baseUrl = null,  
    callbackUrl = "",  
    service = "[input your AppName]",  
    object : Asleep.AsleepConfigListener {  
        override fun onSuccess(userId: String?, asleepConfig: AsleepConfig?) {  
            ...
            /* save userId and asleepConfig */
        }
    override fun onFail(errorCode: Int, detail: String) {
        ...
    }
})
```

## 3.2 Step 2: Create SleepTrackingManager

From initAsleepConfig, Enter AsleepConfig as a parameter.

```kotlin Kotlin
import ai.asleep.asleepsdk.tracking.SleepTrackingManager

...

var sleepTrackingManager = Asleep.createSleepTrackingManager(asleepConfig, object : SleepTrackingManager.TrackingListener {                           
    override fun onCreate() {
    }

    override fun onUpload(sequence: Int) {
    }
    
    override fun onClose(sessionId: String) {
        ...
        /* save sessionId */
    }
    
    override fun onFail(errorCode: Int, detail: String) {
    }
})
```

## 3.3 Step 3: Start Tracking

1. Start Recording

When you start, the sequence number is called back once every 30 seconds from the onUpload function of the listener that was registered when the SleepTrackingManager was created.

```kotlin
sleepTrackingManager?.startSleepTracking()
```

2. Analyze during Recording

```kotlin
sleepTrackingManager?.requestAnalysis(object : SleepTrackingManager.AnalysisListener {  
    override fun onSuccess(session: Session) {  
        Log.d("", "${session.toString()}")  
    }
}
```

for session information, reference API>Data API>Get a session

## 3.4 Step 4: Stop Tracking

When you stop, the sessionId is called back from the onClose function of the listener registered at the time of the SleepTrackingManager creation.

```kotlin
sleepTrackingManager?.stopSleepTracking()
```

## 3.5 Step 5: Create Reports

From initAsleepConfig, enter AsleepConfig as a parameter.

```kotlin
val reports = Asleep.createReports(asleepConfig)
```

## 3.6 Step 6: Get Report

1. Receive one SessionID result

enter sessionID when sleepTracking is stopped as a parameter

```kotlin
reports?.getReport(sessionId, object : Reports.ReportListener {
    override fun onSuccess(report: Report?) {
    }

    override fun onFail(errorCode: Int, detail: String) {
    }
})
```

2. Get results by date

```kotlin
val today = LocalDate.now()
reports?.getReports(today.minusDays(7).toString(), today.toString(), "DESC", 0, 20, object : Reports.ReportsListener {
    override fun onSuccess(reports: List<SleepSession>?) {
    }

    override fun onFail(errorCode: Int, detail: String) {
    }
})
```

## 3.7 Step 7: Remove Session

1. Deleting recording history

Enter SessionID to delete the corresponding recording history.

```kotlin
reports?.deleteReport(sessionId, object : Reports.DeleteReportListener {
    override fun onSuccess(sessionId: String?) {
    }

    override fun onFail(errorCode: Int, detail: String) {
    }
})
```

2. Deleting all my information

Deletes the userId and all measured records. The deleted userId is no longer available.

```kotlin
Asleep.deleteUser(object : Asleep.DeleteUserIdListener {
    override fun onSuccess(userId: String?) {
    }

    override fun onFail(errorCode: Int, detail: String) {
    }
})
```
