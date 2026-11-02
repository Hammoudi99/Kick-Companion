# Kick Companion

**An Android app for watching Kick live streams, videos and chat.**

Kick Companion is a fork of [Stream Companion](https://github.com/raymon-io/streamcompanionCrossPlatform) by Raymon (raymon-io). The original stopped getting updates in August 2024 (last commit `ad887b2`). This fork starts from that code and turns it into an Android-only app with a new look, picture-in-picture, background audio, live alerts and more.

> **Built with free Claude, on an Android phone.** Every change in this fork was made with the free version of [Claude](https://claude.ai), working only from an Android phone with [Termux](https://termux.dev) and [Expo](https://expo.dev). Expo's cloud (EAS Build) compiles the APK.

> **Unofficial.** Not made, endorsed or sponsored by Kick or by the original author. It uses the same web endpoints as Kick's own website, and those can change without notice. "Kick" belongs to its owners.

Current version: **0.10.1** (versionCode 13) · Android only (64-bit ARM) · MIT licence · [Changelog](CHANGELOG.md)

---

## What it does

### Watching
- **Live streams and videos**, with a quality picker (Auto, 1080p, 720p...). Changing quality doesn't restart the video.
- **Picture-in-picture** when you leave the app, with a headphone button in the window for background audio.
- **Audio-only mode** (headphone icon in the player): the picture hides and the sound keeps playing in the background.
- **Notification card** with the title, the streamer and play/pause. You can switch it off in Settings.
- **Playback speed** (live up to 1.25x, videos up to 30x), **skip buttons** or **double-tap gestures**, a **sleep timer**, and an adjustable **chat width** in landscape. Your choices are remembered.
- **Resume** a video where you stopped, from the video list or Saved videos. Video and chat start at your saved time.
- The phone's volume keys and your headset's volume buttons control the volume.

### Chat
- Live chat reconnects by itself, and shows the reason if it can't connect.
- Emotes from 7TV, BetterTTV and FrankerFaceZ.

### Browsing
- **Following**: the streamers you follow. Order them **Live first** (default), **A-Z** or **Added**, and **pin** favourites to the top.
- **Continue watching**: a row of videos you started but didn't finish, with a progress bar and the time left.
- **Live**: Kick categories with viewer counts and tags. Tap one to see its live streams.
- **Saved videos**: bookmark a video and find it later. Videos you started show a progress bar.
- **Search**, with recent searches.
- Streamer cards show a green ring when live. Tap the avatar to open the live stream, or the videos if they are offline.

### Live alerts
- A notification when a streamer you follow goes live, with the title and the game. Tap it to open the stream.
- Choose which streamers can alert you, and set **quiet hours** (alerts still arrive, but without sound or vibration).
- The phone does the checking, with no server: about every 10 minutes in the background and every 5 minutes while the app is open.
- Settings > Notifications has a test button and a "Check now" button.

### Backup and updates
- **Back up your data** (Settings > Backup) turns your follows, pins, saved videos, resume spots and settings into text you can keep in Notes, an email or a chat. **Restore** adds back whatever the phone is missing and never replaces or removes anything. Restart the app afterwards.
- **Check for updates** (Settings > Updates) looks on GitHub for a newer version when you tap it. It never downloads or installs anything by itself.

### Look
- Themes: **AMOLED Black** (default), **Dark**, **Light** and **Material You** (Android 12 and newer).
- A short launch animation, a new icon, a Settings screen (gear icon on Home) and an About screen that credits the original author and Claude.

### Smaller, Android only
- The iOS and TV files were removed. The APK is built for 64-bit ARM only: about 30 MB instead of 75 MB. It won't install on a 32-bit-only phone or an x86 emulator.
- It has its own app id (`com.kickcompanion.appv2`), so it installs next to the original app.

---

## Install

1. Download the latest APK from the [Releases](https://github.com/Hammoudi99/Kick-Companion/releases) page.
2. Open it, and allow "Install unknown apps" if Android asks.
3. You need a phone with a 64-bit ARM processor (almost every phone from the last several years).

To update later, tap **Settings > Updates > Check for updates**, then install the new APK over the old one.

For the best experience, open Android's settings for Kick Companion (Settings > Android app settings in the app takes you there) and:
- allow **Notifications**: live alerts and the notification card need them,
- allow **Picture-in-picture**, if you want the floating window,
- set **Battery** to **Unrestricted**, if you want live alerts on time.

## Build it yourself

You need Node.js and a free [Expo](https://expo.dev) account. The `android/` folder is not stored here: EAS creates it from `app.json` and the plugins in `plugins/`.

```bash
npm install
npm install -g eas-cli      # once
eas login                   # once
eas build -p android --profile preview
```

When the cloud build finishes, EAS gives you a link to the APK.

On a phone, install Node.js in [Termux](https://termux.dev) (`pkg install nodejs`) and run the same commands. The original's other commands (`npx expo run:android`, local EAS builds) need the Android SDK on a computer. They have not been tried with this fork.

## Good to know

- **Live alerts are best effort.** Android decides when background checks run. Battery saver, Doze and some phone makers' battery managers can delay or skip them. Setting Battery to Unrestricted helps. Force-stopping the app stops the checks until you open it again. There is no push server, so an alert is never instant.
- **Kick can change its endpoints at any time.** If it does, parts of the app can stop working until it is updated.
- **Tested on one phone.** Other phones and Android versions can behave differently. Please open an issue if something doesn't work.
- **Your data stays on your phone.** There is no account or login. Your follows, saved videos, resume spots and settings are stored only on the phone, and a backup is text you keep wherever you choose.
- **Who the app talks to.** Kick; 7TV, BetterTTV and FrankerFaceZ (for emotes); and GitHub, only when you tap Check for updates. That request sends no account or personal data, but, like any web request, it lets GitHub see your IP address.
- **Uninstalling the app removes its data.** Make a backup first if you want to keep your follows, saved videos and settings.

## For developers

- `react-native-video` is pinned to 6.19.2 with Media3 1.4.1.
- Small Expo config plugins in `plugins/` patch the generated Android project: picture-in-picture, the headphone action, background keep-alive, media volume stream, notification permission, display cutout, black window background, arm64-only packaging, and Flipper removal (needed by the `react-native-tvos` base).
- `.npmrc` sets `legacy-peer-deps=true`. `eas.json` has a `preview` profile that makes an installable APK.
- The original's TV navigation code is still in the project, because the player, chat and profile screens use it.
- **Check for updates** reads the releases of this repository. Tag each release with its version, like `v0.10.1`, and attach the APK.

```
App.tsx                 navigation, About screen, launch and exit handling
theme.ts                the colour themes
kick/                   screens: home, profile, search, settings, live and video players
kick/services/          Kick API, chat, emotes, saved videos, Continue watching, following order and pins,
                        live alerts, backup, update check, settings store
kick/components/        shared pieces: player settings panel, slider, launch animation
plugins/                Expo config plugins that patch the generated Android project
assets/                 icons
```

## Credits and licence

- **Stream Companion**, the original app, is by Raymon LLC ([raymon-io](https://github.com/raymon-io)): <https://github.com/raymon-io/streamcompanionCrossPlatform>. This fork would not exist without it.
- Modified with **Claude** (Anthropic), on an Android phone with Termux and Expo.
- Emotes come from [7TV](https://7tv.app), [BetterTTV](https://betterttv.com) and [FrankerFaceZ](https://www.frankerfacez.com).
- Released under the **MIT licence**, the same as the original. See [LICENSE](LICENSE), which keeps the original copyright notice.
