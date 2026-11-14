# CHRONOVA — Time Beyond Limits
Native Android app (Kotlin + Jetpack Compose). Phone, tablet, Android TV. minSdk 26, targetSdk 34.

## APK banane ke 3 tareeke

### A) Bina kuch install kiye (GitHub Actions)
1. github.com par naya repo banao, is folder ki saari files upload/push karo (branch `main`).
2. Repo ▸ **Actions** ▸ "Build CHRONOVA APK" ▸ **Run workflow**.
3. 5-8 min baad run khol ke **Artifacts ▸ chronova-apk** download karo → `app-debug.apk`.

### B) Android Studio
1. Android Studio (Koala+) mein **Open** → ye folder. Gradle sync hone do.
2. **Build ▸ Build Bundle(s)/APK(s) ▸ Build APK(s)** → `app/build/outputs/apk/debug/app-debug.apk`.

### C) Command line (JDK 17 + Android SDK 34 + Gradle 8.7)
    gradle assembleDebug        # or ./gradlew after `gradle wrapper`
Release (signed) build ke liye apna keystore signingConfig mein add karo, phir `gradle assembleRelease`.

## Install
- Phone: APK copy karo → install (Unknown sources allow).
- Android TV: `adb connect <tv-ip>` → `adb install app-debug.apk` (ya "Send files to TV"). Launcher mein Chronova dikhega (banner ke saath).

## Features → code
| Feature | File |
|---|---|
| Home / digital+analog / 12-24h / full-screen / night clock | ui/HomeScreen, ui/Clocks, ui/FullScreen |
| Stopwatch (monotonic `elapsedRealtime`, laps, best/avg/worst) | ui/StopwatchScreen, Storage.kt (StopwatchEngine) |
| Timers (multiple, presets, background ring) | ui/TimerScreen, AppViewModel, Scheduler |
| Alarms (repeat, label, tone, snooze, dismiss) | ui/AlarmScreen, Scheduler, Ring.kt |
| World clock (≈120 cities, search, offset, day/night) | ui/WorldClockScreen, Cities.kt |
| Widgets (stopwatch + clock) | Widgets.kt |
| System screensaver (Daydream) | ClockDream.kt |
| TV: sidebar, D-pad focus, big targets, idle screensaver | ui/App.kt, ui/Components.kt |

## Background alarms
`AlarmManager.setAlarmClock()` (Doze-proof, no special permission) → `AlarmReceiver` → foreground `AlarmService` (MediaPlayer, USAGE_ALARM) + full-screen notification + `AlarmRingActivity`. `BootReceiver` reschedules after reboot / time / timezone change.
Settings ▸ **Test alarm in 5 seconds** se turant verify karo.

## Permissions
POST_NOTIFICATIONS (runtime), USE_FULL_SCREEN_INTENT, USE_EXACT_ALARM (timers), WAKE_LOCK, VIBRATE, RECEIVE_BOOT_COMPLETED, FOREGROUND_SERVICE(+MEDIA_PLAYBACK).
