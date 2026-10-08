# Cricket Rush: Android app (Capacitor 8)

The game is `www/index.html`. Capacitor packs it into a native Android app that runs fully offline.

## What's already set up
- **Bundled font:** Barlow Condensed is in `www/fonts`, so the app makes no network requests at all.
- **Target API level:** `targetSdkVersion 36` (Android 16), which Google Play currently requires.
- **Screen:** locked to portrait, full-screen with the system bars hidden, and the screen stays awake during play.
- **Back button:**
  - In a match: pause, or resume if already paused.
  - Help or leaderboard: go back.
  - Results: go to the menu.
  - Menu: exit the app.
- **App switching:** leaving the app (home button, a call, screen off) pauses the match and stops the sound.
- **Saved data:** the leaderboard and the sound and commentary settings are kept on the phone.
- **Haptics:** native vibration on hits, boundaries and wickets.
- **Commentary:** if the phone has no built-in speech voice, the commentary buttons are hidden automatically.
- **Graphics:** app icon (adaptive, with round and legacy versions), launch splash screen, and Play Store art in `store-assets/`.

## Before you build: choose your package name
Your package name is permanent once the app is published. Replace `com.yourcompany.boundaryblitz` in these files:
- `capacitor.config.json` (`appId`)
- `android/app/build.gradle` (`namespace` and `applicationId`)
- `android/app/src/main/res/values/strings.xml` (`package_name`, `custom_url_scheme`)
- The folder `android/app/src/main/java/com/yourcompany/boundaryblitz/`: rename it, and update the `package` line in `MainActivity.java`.

## Build it (Windows, PowerShell)
You need Node.js 22+ and Android Studio (recent; it brings JDK 21 and Android SDK 36).
```powershell
cd boundary-blitz-app
npm install                  # restores Capacitor (node_modules is not in the zip)
npx cap sync android         # copies www/ into the Android project
npx cap open android         # opens Android Studio
```
In Android Studio, wait for the Gradle sync to finish. Then:
1. **Test on your phone:** plug it in with USB debugging on, then click Run ▶.
2. **Build for the Play Store:** go to **Build → Generate Signed App Bundle / APK → Android App Bundle**, then **Create new…** keystore.
   - **Back up the `.jks` file and both passwords.** You need them for every future update.
   - Choose **release**. The output is `android/app/release/app-release.aab`, which is the file you upload to Play Console.

## When you change the game
Edit `www/index.html`, then run `npx cap sync android` and build again. Before every Play Store upload, raise `versionCode` (1, 2, 3, ...) and `versionName` in `android/app/build.gradle`.

## Play Console checklist
- **Store listing:**
  - App icon: `store-assets/play-icon-512.png`
  - Feature graphic: `store-assets/feature-graphic-1024x500.png`
  - At least 2 phone screenshots
  - Short and full descriptions
- **Category:** Games → Sports.
- **Content rating questionnaire:** a sports game, no violence or gambling.
- **Target audience:** 13+.
- **Ads:** No.
- **Data safety:** no data collected or shared. The app has no internet permission and everything stays on the phone.
- **Privacy policy URL:** required. A short page on Netlify saying the game collects no data is enough.
- **Testing rule:** personal developer accounts must run a closed test with at least 12 testers for 14 days in a row before going public. Organization accounts are exempt.
