# Android APK Setup on Windows

Run Android apps (APKs) on Windows using the Google Play Games Developer Emulator with ADB, plus optional root via Magisk.

This repo turns the setup into a clean, repeatable workflow without relying on heavy traditional Android emulators.

## Goal

- Run Android on Windows
- Install and use APK files
- Keep the setup lightweight
- Optionally enable root with Magisk

![Overview](assets/screenshots/screenshot1.png)

## Requirements

1. Google Play Games Developer Emulator
   Download from the official Android Developers page:
   [Google Play Games Developer Emulator](https://developer.android.com/games/playgames/emulator)

   - Download the stable edition
   - Install it
   - Launch it once
   - Close it completely afterward, including from Task Manager if needed

2. Magisk APK
   Download the latest official release:
   [Magisk Releases](https://github.com/topjohnwu/magisk/releases)

3. `hpesuperpower`
   Download the latest release ZIP and unzip it:
   [MagiskOnGooglePlayGames Releases](https://github.com/chsbuffer/MagiskOnGooglePlayGames/releases)

4. ADB platform-tools
   Download the official ZIP and unzip it:
   [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools)

5. Aurora Store APK (optional)
   Download the latest APK:
   [Aurora Store Releases](https://www.auroraoss.com/files/AuroraStore/Release)

## Recommended Folder Layout

Create a working folder anywhere you like. In this guide, we will call it `emulator-windows` and place it inside `Documents`.

```text
emulator-windows/
|-- hpesuperpower-1.2.1/
|   |-- hpesuperpower.exe
|-- platform-tools/
|   `-- adb.exe
`-- apks/
    |-- AuroraStore.apk
    |-- app2.apk
    |-- app3.apk

```

Notes:
- Keep APKs you want to install inside `apks/` for organization.
- You can still install APKs from any path with `adb install` apk path.

![Folder structure](assets/screenshots/screenshot2.png)

## Setup

### 1. Install Google Play Games Developer Emulator

- Install the emulator
- Launch it once
- Close it fully

Inside the emulator, keyboard shortcuts are the main navigation method:

- `Ctrl+B` goes back
- `Ctrl+H` goes to the home screen

![Launch emulator](assets/screenshots/screenshot3.png)

If it stays running in the tray, end it before moving on.

### 2. Add ADB to PATH

1. Launch the emulator once from the hidden taskbar icons area.
2. Locate your `platform-tools` folder.
3. Right-click the folder and choose `Copy as path`.
4. Open `Edit the system environment variables` from the Start menu.
5. Click `Environment Variables`.
6. Under `User variables`, select `Path`.
7. Click `Edit`.
8. Click `New`.
9. Paste the full `platform-tools` path.
10. Click `OK` to save all dialogs.

![Copy platform-tools path](assets/screenshots/screenshot4.png)
![Open system environment variables](assets/screenshots/screenshot5.png)
![Environment Variables button](assets/screenshots/screenshot6.png)
![Select Path under user variables](assets/screenshots/screenshot7.png)
![Click Edit for Path](assets/screenshots/screenshot8.png)
![Click New in the Path editor](assets/screenshots/screenshot9.png)
![Paste the platform-tools path](assets/screenshots/screenshot10.png)

### 3. Test ADB

Open Command Prompt and run:

```powershell
adb devices
```

Expected output:

```text
localhost:xxxx device
```

If you see a connected device entry, ADB is working.

![ADB devices output](assets/screenshots/screenshot11.png)

### 4. Optional Root Setup with Magisk

Skip this section if you do not need root.

1. Open Command Prompt as Administrator in the `hpesuperpower` folder.
2. Change into that folder:

```powershell
cd path\to\emulator-windows\hpesuperpower-x.x.x
```

3. Patch the emulator with your real Magisk APK version:

```powershell
hpesuperpower.exe magisk Magisk-vXX.XX.apk --dev
```

Use the actual filename you downloaded, for example:

```powershell
hpesuperpower.exe magisk Magisk-v30.7.apk --dev
```

## Installing APKs

Put your APK files inside the `apks/` folder if you want to keep everything organized.

### Method 1: ADB

This is the most reliable method.

```powershell
adb install "full\path\to\app.apk"
```

Example:

```powershell
adb install "C:\Users\YourName\Documents\emulator-windows\apks\app1.apk"
```

![ADB install example](assets/screenshots/screenshot12.png)

### Method 2: Aurora Store

Install Aurora Store with ADB:

```powershell
adb install "C:\Users\YourName\Documents\emulator-windows\apks\AuroraStore.apk"
```

Then open Aurora Store inside the emulator and sign in using the mode you prefer.

![Aurora Store or installed app example](assets/screenshots/screenshot13.png)

### Method 3: Browser Inside the Emulator

- Open Chrome or another browser inside the emulator
- Download the APK directly
- Install it manually

## Notes

- The Play Store experience is limited in this environment
- Aurora Store is usually better for daily app installs
- ADB is the most dependable install path
- Navigation inside the emulator is limited, so use `Ctrl+B` for Back and `Ctrl+H` for Home

## Daily Workflow

1. Launch the emulator
2. Install apps with ADB or Aurora Store
3. Open and use the installed apps

## Common Issues

### Device Not Detected

```powershell
adb kill-server
adb start-server
adb devices
```

### APK Install Fails

Try reinstalling with replace mode:

```powershell
adb install -r app.apk
```

### App Installed but Not Showing

List installed packages:

```powershell
adb shell pm list packages
```

Launch an app manually:

```powershell
adb shell monkey -p package.name -c android.intent.category.LAUNCHER 1
```

## Result

After setup, you will have:

- Android running on Windows
- APK installation support
- Optional Magisk root
- A lighter alternative to traditional emulators

## Disclaimer

Rooting or patching the emulator may break in future releases and can carry security or stability risks. Only do it if you understand the tradeoffs.
