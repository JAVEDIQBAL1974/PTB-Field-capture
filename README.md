# PTB Field Capture — Native Android Project

This is a complete, real Capacitor/Android project wrapping the offline-first
field app (`www/index.html`, same file as `mobile-app/index.html` in the main
package) as an installable Android app — not a mockup, not a screenshot: a
genuine Gradle project with a manifest, launcher icons, and a Java
`MainActivity`.

## Why you're not receiving a `.apk` file directly

Compiling an APK requires downloading the Gradle build tool and the Android
SDK/build-tools from Google's servers. Those downloads cannot be completed
in the sandboxed environment this project was built in — there is no way
around that from here, so rather than claim to hand you a working binary
that wasn't actually compiled, this package gives you the complete real
source plus the fastest path to compile it yourself, with **zero Android
experience required**.

## Option A — Build it in the cloud with GitHub Actions (recommended, ~5 minutes, free)

1. Create a new GitHub repository and push this whole `mobile-app-native/`
   folder to it (or push the full project and keep this folder as a
   subdirectory — the workflow file already accounts for that path).
2. GitHub Actions will run automatically on push (see
   `.github/workflows/build-apk.yml`) — or click **Actions → Build PTB Field
   Capture APK → Run workflow** to trigger it manually.
3. When the run finishes (green check), open it and download the
   **PTB_Field_Capture-debug.apk** artifact from the bottom of the page.
4. Transfer that `.apk` to an Android phone (email, USB, Google Drive) and
   tap it to install — you may need to allow "install from unknown sources"
   for the app you used to open the file.

No Android Studio, no SDK setup, nothing to configure — GitHub's runner
already has everything required.

## Option B — Build it locally with Android Studio

1. Install [Android Studio](https://developer.android.com/studio) (this
   step needs to happen on your own machine with normal internet access).
2. Open the `android/` folder in this project as an existing project.
3. Let Android Studio finish its first-time Gradle sync (it will download
   the SDK components it needs automatically).
4. Build → Build Bundle(s) / APK(s) → Build APK(s).
5. The APK appears under `android/app/build/outputs/apk/debug/app-debug.apk`.

## Option C — Build it locally from the command line

Only if the Android SDK is already installed and `ANDROID_HOME` is set:

```bash
npm install
npm run build:apk:local
```

## Updating the app's content

The app's UI lives entirely in `www/index.html` (identical to the standalone
web version). To change anything about what the app shows or does, edit
that file, then re-run `npx cap sync android` before rebuilding — Capacitor
copies `www/` into the native project on every sync.

## Turning this into a real production app (next steps)

- Point the app's fetch calls at the real backend (`../backend`) instead of
  the in-memory simulation, once the API is deployed and reachable.
- Replace the placeholder launcher icon/splash screen
  (`android/app/src/main/res/mipmap-*`, `drawable-*/splash.png`) with PTB's
  actual branding.
- Set up the `build-release-apk` job in the CI workflow (signing keystore as
  a GitHub Secret) before distributing outside a test group.
- Google Play requires an Android App Bundle (`.aab`) rather than a raw
  `.apk` for production listings — `./gradlew bundleRelease` produces that
  from the same project once signing is configured.
