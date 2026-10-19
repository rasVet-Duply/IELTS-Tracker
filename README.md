# IELTS Tracker (Android)

Offline IELTS Academic practice and progress tracker for students: Listening, Reading, Writing, Speaking and a General Statistics dashboard. No accounts, no server, no analytics. Built with **Capacitor 7 + Vite + vanilla JavaScript**.

> **Status:** the source project is complete and tested in Node/jsdom (50 automated tests). **An APK has not been built or run on a device yet** — the environment this was written in cannot reach the Android SDK / Gradle servers. See "Build the APK" and `docs/STATUS.md` for exactly what is and is not verified.

## Build the APK

### Option A – GitHub Actions (no local Android tooling)
1. Create a GitHub repository and push this folder.
2. Open **Actions › Build Android APK › Run workflow**.
3. Download the `ielts-tracker-apk` artifact: `app-debug.apk` (installable for testing). A release APK is signed only if you add the secrets `ANDROID_KEYSTORE_B64`, `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_ALIAS`, `ANDROID_KEY_PASSWORD`; otherwise it is unsigned and cannot be installed.

### Option B – on your computer
Requirements: **Node 22**, **JDK 21**, **Android Studio** (or the Android SDK command-line tools with platform 35 and build-tools).
```bash
npm ci
npm test                      # 50 automated tests
npm run apk:debug             # builds web app, syncs, runs gradle  → android/app/build/outputs/apk/debug/app-debug.apk
```
Release build: run `scripts/make-keystore.sh`, create `android/keystore.properties` (see the script output), then `npm run apk:release`. **Back up your keystore and passwords** – without them you cannot update an installed app.

### Install on a phone
Copy `app-debug.apk` to the phone (USB, Drive, Telegram), open it, allow "Install unknown apps" for the app you opened it with, and install. Or: `adb install -r app-debug.apk`.

## Run in a browser (development only)
`npm run dev` – storage uses IndexedDB, audio recording/camera depend on the browser. The product target is the Android app.

## Where data is stored
* Records, settings: IndexedDB inside the app's private WebView storage.
* Recordings / images / attachments: copied into the app's private files (`Directory.Data/media/…`) via Capacitor Filesystem; only the app can read them.
* Android cloud backup and device-transfer are **disabled on purpose** (student data stays local). Use *Settings › Backup & Restore*.
* Remove everything: *Settings › Data management › Delete all data*, or uninstall.

## Documentation
`docs/ARCHITECTURE.md` · `docs/BACKUP_FORMAT.md` · `docs/CONVERSION_TABLES.md` · `docs/STATUS.md` (features, tests, limitations) · `docs/FUTURE_PLATFORMS.md`

## Replace the Writing formula
Everything Writing-related calls `src/core/scoring.js` only (`calcWritingTaskBand`, `combineWriting`). Edit those functions and set `WRITING_SCORING.provisional = false`; screens, statistics and exports pick it up.

## Brand
Logo from the supplied PNG (white-on-transparent copy in `assets/`, launcher icon and splash generated from it). Colours sampled from the supplied images: navy `#00345c`/`#184266`, yellow `#fcbc38`. Fonts: **Anton** (headlines) and **Mulish** (body) – chosen by visual match to the supplied posts/website; swap in `src/styles.css` if your brand fonts differ.
