# Kick Companion

**An Android app for watching Kick live streams, videos and chat.**

Kick Companion is a fork of [Stream Companion](https://github.com/raymon-io/streamcompanionCrossPlatform) by Raymon (raymon-io). It keeps the original idea (a simple Kick client) and turns it into an Android-only app with a redesigned interface and themes, picture-in-picture, background audio, saved videos, live alerts for the streamers you follow, and a lot of fixes.

> **Built with free Claude, on an Android phone.**
> Every change in this fork was made with the free version of [Claude](https://claude.ai), working entirely from an Android phone with [Termux](https://termux.dev) and [Expo](https://expo.dev) (EAS Build compiles the APK in Expo's cloud).

> **Unofficial.** Kick Companion is not made, endorsed or sponsored by Kick or by the original author. It uses the same web endpoints that Kick's own website uses, and those can change without notice. "Kick" belongs to its owners.

Current version: **0.9.0** (versionCode 10) · Android only (64-bit ARM) · MIT licence · [Changelog](CHANGELOG.md)

---

## What changed from the original

The original app stopped receiving updates in August 2024 (its last commit is `ad887b2`). This fork starts from that code. Everything below was added or changed since then; the [changelog](CHANGELOG.md) lists it version by version, from 0.2.0 to 0.9.0.

### Player (live streams and videos)
- **Quality picker** (Auto, 1080p, 720p, ...). It uses the player's own track selection, so changing quality no longer restarts the video.
- **Playback speed** (live up to 1.25x, videos up to 30x), **skip buttons** with a skip interval you choose, or **double-tap gestures**, a **sleep timer**, and an adjustable **chat width** in landscape. Your choices are remembered. The settings panel was redesigned in 0.6.0.
- **Picture-in-picture** when you leave the app, with a **headphone button inside the PiP window** that switches to background audio.
- **Audio-only mode** (headphone icon in the player): hides the picture and keeps the sound going in the background.
- **Media card in the notification shade** (title, streamer, play/pause). It can be switched off in Settings.
- The phone's **volume keys**, and the volume buttons on a headset, control the media volume.
- Background audio is kept alive with a wake lock, so it should no longer cut out a few seconds after the app leaves the screen.
- Live streams no longer pause themselves after a short buffering stall, and the buffering is tuned so chat lines up better with the picture.
- Video durations were shown about 1000 times too long. Fixed.
- **Resume** a video where you stopped, from the videos list or from Saved videos. Since 0.9.0 it starts straight at your saved time (video and chat) instead of showing the beginning first and then jumping.

### Chat
- The live chat connection was rewritten after live chat stopped working: it reconnects by itself, tries alternative connection keys, and shows a short notice with the reason if chat can't connect.
- Emotes: the original's 7TV emotes, plus **BetterTTV and FrankerFaceZ global emotes**.

### Browsing
- **Home** has a **Following** tab (the streamers you follow, loaded in parallel, no more duplicates, refreshed when you come back) and a **Live** tab (Kick categories with viewer counts and tags; tap one to see its live streams).
- **Streamer cards** show a green ring when live and an eye icon with the viewer count. Tapping the avatar opens the live stream, or the videos if they are offline. Follow / Unfollow updates instantly.
- **Saved videos**: bookmark any video and find it again later on the Saved screen.
- **Search** was redesigned in 0.8.0: recent searches, and clear loading, empty and error states.

### Live alerts (new in 0.9.0)
- A notification when a streamer you follow goes live. Tap it to open the stream.
- The phone does the checking, with no server: about every 10 minutes in the background and every 5 minutes while the app is open. Android decides the exact timing, so an alert can arrive a little late (see [Good to know](#good-to-know)).
- **Settings > Notifications**: on/off switch, a test notification button, and a "Check now" button that says what it found.
- Turning it on never floods you with alerts for streams that were already running.

### Look and feel
- Dark interface with a theme setting: **AMOLED Black** (default), **Dark**, **Light** and **Material You** (Android 12 and newer, with a safety fallback if it ever fails to start).
- A short launch animation, and a new app icon (a green ring with a play mark on black) that matches it.
- A **Settings** screen (gear icon on Home), and an About screen that credits the original author and Claude.

### Android only, and smaller
- The iOS and tvOS configuration, the iOS / TV images and the TV run script were removed. The app is built and tested for Android phones only (the TV navigation code from the original is still in the project, because the player, chat and profile screens use it).
- Built for **arm64-v8a only**, which took the APK from about 75 MB to about 30 MB. It will not install on a 32-bit-only phone or an x86 emulator.
- Its own app id, `com.kickcompanion.appv2`, so it installs next to the original app instead of replacing it.

### Build changes (for developers)
- `react-native-video` is pinned to 6.19.2 with Media3 1.4.1.
- A set of small Expo config plugins in `plugins/` patch the generated Android project: picture-in-picture, the headphone action, background keep-alive, media volume stream, notification permission, display cutout, black window background, arm64-only packaging, and Flipper removal (needed by the `react-native-tvos` base).
- `.npmrc` sets `legacy-peer-deps=true`, and `eas.json` has a `preview` profile that produces an installable APK.

---

## Install

1. Download the latest APK from this repository's **Releases** page.
2. Open it and allow "Install unknown apps" if Android asks.
3. It needs a phone with a 64-bit ARM CPU (almost every phone made in the last several years).

For the best experience, in Android's settings for Kick Companion:
- **Notifications**: allow them (Android 13 and newer asks when you open the app). Live alerts and the media card need this.
- **Picture-in-picture**: allow it, if you want the floating window.
- **Battery**: choose **Unrestricted** if you want live alerts to arrive on time.

The Settings screen has an "Android app settings" shortcut for all of these.

## Build it yourself

You need Node.js and a free [Expo](https://expo.dev) account. The `android/` folder is not stored in the repository: EAS generates it from `app.json` and the plugins in `plugins/`.

```bash
npm install
npm install -g eas-cli      # once
eas login                   # once
eas build -p android --profile preview
```

When the cloud build finishes, EAS gives you a link to the APK.

This is how the fork is built on a phone: install Node.js in [Termux](https://termux.dev) (for example `pkg install nodejs`), then run the same commands. The other commands from the original README (`npx expo run:android`, `npx eas-cli build --local -p android --profile preview`) need the Android SDK on a computer. They have not been tried with this fork.

## Good to know

- **Live alerts are best effort.** Android decides when background checks run. Battery saver, Doze and some phone makers' battery managers can delay or skip them, so an alert can be late. Setting Battery to Unrestricted helps. Force-stopping the app stops the checks until you open it again. There is no push server, so an alert can never be instant.
- **Kick can change its endpoints at any time.** The app relies on the same web endpoints Kick's own site uses. If Kick changes them, parts of the app can stop working until it is updated.
- **Tested on one phone.** The app is developed and tested on a single Android phone. Other phones and Android versions can behave differently. Please open an issue if something doesn't work.
- **Your data stays on the phone.** There is no account or login. The list of streamers you follow, saved videos, resume positions and settings are stored only on your phone. The app talks to Kick and, for emotes, to 7TV, BetterTTV and FrankerFaceZ.

## Project layout

```
App.tsx                 navigation, About screen, launch and exit handling
theme.ts                the colour themes
kick/                   screens: home, profile, search, settings, live and video players
kick/services/          Kick API, chat socket, emotes, saved videos, live alerts, settings store
kick/components/        shared pieces: player settings panel, slider, launch animation
plugins/                Expo config plugins that patch the generated Android project
assets/                 icons
```

## Credits and licence

- **Stream Companion**, the original app, is by Raymon LLC ([raymon-io](https://github.com/raymon-io)): <https://github.com/raymon-io/streamcompanionCrossPlatform>. This fork would not exist without it.
- Modified with **Claude** (Anthropic), on an Android phone with Termux and Expo.
- Emotes come from [7TV](https://7tv.app), [BetterTTV](https://betterttv.com) and [FrankerFaceZ](https://www.frankerfacez.com).
- Released under the **MIT licence**, the same as the original. See [LICENSE](LICENSE), which keeps the original copyright notice.


[VirusTotal Check]:(https://www.virustotal.com/gui/file/e914cdec0855b8e1f4d5cdcc8005a74f325ac7fcd9d4cd3a6c2738e5a6b6d9d2/details)
