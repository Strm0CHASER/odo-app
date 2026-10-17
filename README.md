# Odo

Mobile-first mileage and fuel tracker for South African drivers (km, litres, rand).
One HTML file in `www/index.html`, wrapped as an Android app with Capacitor.
All data stays on the device (browser local storage).

## Get a debug APK (no Android Studio needed)

1. Create a new GitHub repository and upload everything in this folder
   (keep the `.github` folder and the `www` folder).
2. Open the **Actions** tab. The "Build Android APK" workflow runs on every push to `main`.
   You can also run it by hand with **Run workflow**.
3. When it finishes (about 5 minutes), open the run and download **odo-debug-apk** under Artifacts.
4. Unzip it, copy `app-debug.apk` to your phone, tap it, and allow "Install unknown apps".

## Get a signed release APK

1. Make a keystore on your computer (keep it safe, never commit it):

       keytool -genkeypair -v -keystore release.jks -alias odo -keyalg RSA -keysize 2048 -validity 10000

2. Convert it to text: `base64 -w0 release.jks` (on a Mac: `base64 -i release.jks`).
3. In the repo go to **Settings > Secrets and variables > Actions** and add four secrets:
   `KEYSTORE_BASE64` (the text from step 2), `KEYSTORE_PASSWORD`, `KEY_ALIAS` (odo), `KEY_PASSWORD`.
4. Push a tag: `git tag v1.0.0 && git push origin v1.0.0`.
   The workflow builds, signs and attaches `odo-release.apk` to a GitHub Release.

## Change the app id or name

Edit `capacitor.config.json`. Use a unique `appId` such as `com.yourname.odo`.

## Update the app

Replace `www/index.html`, commit and push. A new debug APK is built automatically.

## Notes

- Uninstalling the app deletes its data. Use Setup > export json to keep backups.
- Odo's file exports use browser-style downloads, which Android web views may block.
- The Android project is generated in CI. If you run `npx cap add android` locally and
  commit the `android` folder, the workflow will use it instead.
