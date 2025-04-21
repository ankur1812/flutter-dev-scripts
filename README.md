# Flutter / React-Native Scripts
<br>


## 1. Initial Setup: Android Debug Bridge (ADB)
Android Deug Bridge is required to allow connection to your device.
<br>
<br>

### 1.1. Basic ADB commands: 

```
adb kill-server
adb devices
adb tcpip 5555
adb connect phone_ip:5555
```

<br>

### 1.2. Fix failing device connections

In case, there are connection issues with the device, add the device's vendor ID to `android_usb.ini` file
<br>
 
```
# android_usb.ini file location: 

Windows: %USERPROFILE%\.android\adb_usb.ini
Linux/Mac: ~/.android/adb_usb.ini

# Some vendor Hexcodes: 

0x2a96  ← ASUS
0x1ebf  ← BQ
0x2a70  ← Essential
0x18d1  ← Google
0x1004  ← LG
0x2a45  ← Meizu
0x0e8d  ← MediaTek
0x2a4d  ← Micromax
0x12d1  ← Huawei
0x24e3  ← HTC
0x22b8  ← Motorola
0x0bb4  ← OnePlus (formerly HTC's code, reused)
0x04e8  ← Samsung
0x1f3a  ← Sharp
0x0489  ← Sony Ericsson
0x0502  ← Toshiba
0x17ef  ← Lenovo
0x05c6  ← Qualcomm / Xiaomi (shared with some others)
0x2ae5  ← Vivo
0x2a48  ← Wiko
0x2a40  ← ZTE

```

To get the Device ID use the `lsusb` command 

```
> lsusb
Bus 001 Device 002: ID 0e8d:104a MediaTek Inc. LAVA LXX504

# Here 0e8d is the Vendor ID (for MediaTek) and 104a is product ID (specific to LAVA device model).
# NOTE: In android_usb.ini, the hex code must start with 0x. So the entry would be 0x0e8d

```

Once the `android_usg.ini` is updated, restart the adb server.
```
adb kill-server
adb start-server

```

<br>
<br>


### 1.3. (For Linux) You may need to updated udev permissions
<br>


```
sudo nano /etc/udev/rules.d/51-android.rules

# Add the device info to the file:
# SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", MODE="0666", GROUP="plugdev"


# Restart the udev services.

sudo udevadm control --reload-rules
sudo service udev restart

```


<br>


## 2. Flutter commands: 
### 2.1. Create new app
```
flutter create my_todo_app

flutter create --platforms=windows,macos,linux my_todo_app
```

### 2.2. Enable/Disable web pacakging
```
flutter config --enable-web
flutter config --no-enable-web
```

**Use same flags for other platforms :**

```
flutter config --enable-windows
flutter config --no-enable-windows

flutter config --enable-macos
flutter config --no-enable-macos

flutter config --enable-linux
flutter config --no-enable-linux
```

### 2.3. Check devices
flutter devices

### 2.4. Install dependencies
flutter pub get

### 2.5. Run app:
```
flutter run -d your-android-device-id
flutter run -d chrome
```

### 2.6. Build apps
```
flutter build apk --release
flutter build ios --release
flutter build web
flutter run -d windows
flutter run -d macos
flutter run -d linux
```
