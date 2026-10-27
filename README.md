# CleanYourTV

A lightweight custom launcher for Android TV (built for a Xiaomi Mi TV 4A, Android 9, 1 GB RAM),
plus scripts to remove bloatware. It replaces the home screen, not the OS.

## Build
Push to GitHub. The Actions workflow builds the APK; download it from the run's **Artifacts**
(`cleanyourtv-debug`). Or open the folder in Android Studio and run `assembleDebug`.

## Connect to the TV (Android 9: no pairing step)
TV: Settings > About > press Build 7 times > Developer options > USB debugging ON.
```
adb connect 192.168.1.8:5555
adb devices
```

## Install and set as default
```
./scripts/install.sh 192.168.1.8:5555 app-debug.apk
```

## Debloat (reversible)
1. Fill `scripts/packages.txt` from your TV's real package list.
2. Dry run: `./scripts/debloat.sh 192.168.1.8:5555`
3. Apply: `./scripts/debloat.sh 192.168.1.8:5555 --apply`
4. Undo: `./scripts/restore.sh 192.168.1.8:5555`

Install the launcher first. Only then disable the stock home (3rd arg of `install.sh`),
otherwise the TV can end up with no home screen.

## Rules for this launcher
No ads, no autoplay video, no recommendation rows, no third-party SDKs.
The INTERNET permission exists only for the weather card (Open-Meteo, plus a one-time
IP lookup via ipwho.is to find your city; the result is cached on the TV).

## Wallpaper
Without a picture you get a plain gradient. To use your own (JPG, any size, it is downscaled):
```
adb push wallpaper.jpg /sdcard/Android/data/com.cleanyourtv.launcher/files/wallpaper.jpg
```
Then reopen the launcher. Or drop `wallpaper.jpg` into `app/src/main/assets/` before building.

## Using the launcher
D-pad to move, OK to open, hold OK to add/remove a favourite.
Top right shows RAM and storage (updates every 5 s). **Clean RAM** stops background apps;
Android may restart some of them later, so the gain can be temporary.
