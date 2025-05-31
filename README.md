# Permissions Library

## Overview

PermissionsLibrary is a lightweight and extensible Android library that simplifies runtime permission handling in your applications.
It helps developers request, explain, and manage Android permissions in a modern and user-friendly way — with full support for rationale dialogs, permanently denied detection, Jetpack Compose, Kotlin coroutines, and more.

## Features

- Request multiple permissions in one call
- Show rationale dialogs before requesting permissions
- Handle "denied" and "permanently denied" permission states
- Guide users to app settings for permanently denied permissions
- Observe permission state changes using LiveData
- Jetpack Compose support
- Support for Kotlin Coroutines and Flow
- Support for special permissions (SYSTEM_ALERT_WINDOW, WRITE_SETTINGS)
- Fully customizable UI dialogs and buttons
- Easily integrated into Java and Kotlin apps

## Installation

1. Clone the repository:

```bash
git clone https://github.com/NoaGilboa/PermissionsLibrary.git
```

2. Include the library module in your project:

Add the following to your project-level `settings.gradle` file:

```gradle
include ':permissionslib'
project(':permissionslib').projectDir = new File(settingsDir, '../path_to_permissionslib_directory')
```

3. Add the library as a dependency in your app-level `build.gradle` file:

```gradle
dependencies {
    implementation project(":permissionslib")
}
```

## Usage

### Step 1: Update Manifest

Ensure your `AndroidManifest.xml` includes the required permissions:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    <uses-permission android:name="android.permission.READ_SMS" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.READ_CALENDAR" />
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```

### Step 2: Initialize PermissionsManager in Your Activity

1. Import the necessary classes:

```java
import com.example.permissionslib.PermissionsManager;
import android.os.Bundle;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
```

2. Initialize and use `PermissionsManager`:

```java
public class MainActivity extends AppCompatActivity {
    private PermissionsManager permissionsManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        permissionsManager = new PermissionsManager(this);

        String[] permissions = {
                android.Manifest.permission.CAMERA,
                android.Manifest.permission.ACCESS_FINE_LOCATION,
                android.Manifest.permission.ACCESS_COARSE_LOCATION,
                android.Manifest.permission.READ_CONTACTS,
                android.Manifest.permission.READ_SMS,
                android.Manifest.permission.CALL_PHONE,
                android.Manifest.permission.RECORD_AUDIO,
                android.Manifest.permission.READ_CALENDAR,
                android.Manifest.permission.ACCESS_BACKGROUND_LOCATION,
                android.Manifest.permission.INTERNET
        };

        permissionsManager.addPermissionsButton(permissions, 1);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (permissionsManager != null) {
            permissionsManager.handlePermissionResult(requestCode, permissions, grantResults);
        }
    }
}
```
---

## 🎯 Advanced Features

### 🔁 Coroutine Support

```kotlin
lifecycleScope.launch {
    val result = permissionsManager.requestPermissionsAsync(
        listOf(Manifest.permission.CAMERA, Manifest.permission.RECORD_AUDIO)
    )
    if (result.allGranted) {
        // Proceed
    }
}
```

---

### 🧱 Jetpack Compose Support (Optional Module)

```kotlin
@Composable
fun CameraPermissionView() {
    val permissionState = rememberPermissionState(Manifest.permission.CAMERA)
    if (permissionState.hasPermission) {
        Text("Permission granted!")
    } else {
        Button(onClick = { permissionState.launchPermissionRequest() }) {
            Text("Request Camera")
        }
    }
}
```

---

### ⚙️ Handling "Don't Ask Again"

```java
if (permissionsManager.isPermissionPermanentlyDenied(Manifest.permission.CAMERA)) {
    permissionsManager.showSettingsDialog();
}
```

---

### 🔒 Requesting Special Permissions

```java
if (!Settings.canDrawOverlays(context)) {
    Intent intent = new Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION);
    startActivity(intent);
}
```
---

## Customization

- **Button Styles**: Customize the `MaterialButton` styles (e.g., corner radius, background color) in `PermissionsManager.java`.
- **Dialog Text**: Change the text in dialogs and toast messages to fit your application's needs.
- **Localization**: Fully support for internationalization (strings.xml).


## video 

https://github.com/user-attachments/assets/d1d429d7-ed15-4207-a334-db21493b23e0


