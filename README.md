# Rhythmhel for Android

A full-screen, landscape Android app that opens the Rhythmhel game. It is a thin shell around your web game
(`https://rhythmhel.imfone.dev/play.html` by default), so **the game updates itself: you only rebuild the app
when you want to change the app, not when you update the game.**

What the app adds over just using the browser:
- full screen, landscape, screen stays on, music starts without an extra tap
- picking files works (importing .osz / .osu / songs / backgrounds)
- exports work (.osz / .rhythmhel): you choose where to save them
- the game's confirm / prompt dialogs work
- Back button = Esc in the game; Back on the main menu leaves the app
- an offline screen with a "Try again" button
- your saved songs, settings and login stay on the phone between launches

---

## Get the APK (no Android software needed): GitHub builds it for you

1. Make a free account at https://github.com and create a **new private repository** (keep it private, see "Keep it private" below).
2. Upload everything from this folder into it:
   on the repository page choose **Add file > Upload files**, drag in the *contents* of this folder, and press **Commit changes**.
   - The workflow lives in a hidden folder called `.github`. On a Mac press Cmd+Shift+. in Finder to see it.
   - If `.github/workflows/build-apk.yml` did not upload: choose **Add file > Create new file**, type
     `.github/workflows/build-apk.yml` as the name, paste in the contents of the `build-apk.yml` file from this folder, and commit.
3. Open the **Actions** tab. A build starts by itself (or choose **Build Android APK > Run workflow**). It takes about 4-6 minutes.
4. When it shows a green tick, open the run and download **Rhythmhel-apk** from the *Artifacts* section at the bottom.
   Unzip it: inside is `Rhythmhel-1.0.N.apk`.
5. Copy the APK to your phone (cable, cloud drive, or open the artifact link in the phone's browser) and tap it.
   Android asks you to allow "Install unknown apps" for the app you opened it from. Allow it once, then install.

To make a new version later, just run the workflow again and install the new APK on top: it keeps your data
because every build is signed with the same key.

### If your game is not at `/play.html`
Run the workflow with the **start_url** box filled in, or change `START_URL` in `app/build.gradle`.

## Or build it yourself in Android Studio
1. Install Android Studio, choose **Open**, and pick this folder. Let it sync (it downloads Gradle and the SDK by itself).
2. **Build > Build Bundle(s) / APK(s) > Build APK(s)**, or in a terminal in this folder: `gradle assembleRelease`.
3. The APK is in `app/build/outputs/apk/release/app-release.apk`.

## Keep it private
`rhythmhel.keystore` (password `rhythmhel`) is the key that signs the app. Anyone who has it can publish an "update"
that Android will accept as yours. That is fine for a private repository. If you ever make the repository public, make a new key first:

    keytool -genkeypair -keystore rhythmhel.keystore -storetype pkcs12 -alias rhythmhel -keyalg RSA -keysize 2048 -validity 36500

and put its password in `app/build.gradle`. (A new key means the new APK will not install over an old one: uninstall the old app first.)

## Notes
- Needs Android 7.0 or newer.
- The Back button sends an Esc key to the game. If Back ever does nothing in your game, the app can be changed to call something else.
- Opening .osz files from the file manager straight into the app is not built in. Use the game's own Import button.
