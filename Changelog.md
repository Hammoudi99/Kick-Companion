# Changelog

All versions of this fork, newest first. The original project (Stream Companion 1.0.12, last commit `ad887b2`, August 2024) is the starting point and is not listed here. The first release of this fork was 0.2.0.

Each version has its own `versionCode` so a new APK installs over the previous one.

## 0.9.0 (versionCode 10)

### Added
- **Live alerts.** A notification when a streamer you follow goes live; tapping it opens the stream (also when the app was closed). About every 10 minutes in the background and every 5 minutes while the app is open, with no server. Only an offline-to-live change is announced, the first look after switching it on is silent, the same streamer is not announced again within 20 minutes, and a check shows at most five separate alerts (any others are grouped into one "N more streamers are live" alert).
- **Settings > Notifications**: a switch (on by default), "Send a test notification" and "Check now", which reports what it found (no follows, nobody live, alerts sent, nothing new, or that Kick could not be reached).
- New dependencies: `expo-notifications`, `expo-task-manager`, `expo-background-fetch`.
- A new app icon (a green ring with a play mark on black) that matches the launch animation, and a matching notification icon.
- This README (rewritten for the fork) and this changelog.

### Fixed
- Resuming a video no longer shows its first seconds and then jumps to your saved time. The player now starts at the saved position, and the chat starts from the same point instead of replaying the beginning of the video's chat.

## 0.8.1 (versionCode 9)

### Fixed
- The 0.8.0 build failed with `Could not find com.facebook.react:flipper-integration`. Removing the TV config plugin in 0.8.0 also removed the part of it that took Flipper (a debug-only tool) out of the generated Android project. A small new plugin, `plugins/withRemoveFlipper.js`, does that one job on its own. Nothing else changed. Use this version instead of 0.8.0.

## 0.8.0 (versionCode 8) - did not build, use 0.8.1

### Added
- **Theme setting** (Settings > Appearance): AMOLED Black (default), Dark, Light and Material You (Android 12+). If Material You ever fails to start, the app goes back to AMOLED Black on the next launch.
- **Search page redesigned**: recent searches with a Clear button, a clear (x) button in the box, clear loading, no-results and error states with Retry, and an older search can never overwrite a newer one.
- **Launch animation**: a short black screen with a green play mark and a soft ring that fades out when the app is ready.
- The supplied Claude PNG is now the icon next to "Modified by Claude" in About.
- Volume keys (and a headset's volume buttons) control the media volume.

### Fixed
- Audio stopping 5 to 10 seconds after using the PiP headphone button: the player now holds a network wake lock while playing, so the phone no longer puts the CPU and Wi-Fi to sleep.
- Chat names are darker on the light themes so they stay readable.

### Changed
- Android only: the iOS configuration, iOS / TV images, the TV run script and the TV config plugin were removed.
- The APK is built for arm64-v8a only (about 75 MB down to about 30 MB). `.npmrc` sets `legacy-peer-deps=true`.

## 0.7.0 (versionCode 7)

### Added
- A **headphone button inside the PiP window** that switches to background audio (it was being overwritten by the player library's own button list, so it never showed).
- A **media card in the notification shade** (title, streamer, play/pause), with a switch in Settings. The notification permission is declared and asked for when the app opens (Android 13 and newer), because the card is a notification.
- A **Settings screen**, opened with a gear icon next to the info icon on Home.
- The Claude icon next to "Modified by Claude" on the About screen.
- An eye icon with the viewer count on streamer cards, instead of the word "Viewers".

### Fixed
- Live chat not working: the connection was rewritten (`ChatSocket`). It reconnects by itself, tries alternative connection keys, and shows a short notice with the reason if chat can't connect.

## 0.6.0 (versionCode 6)

### Changed
- The player settings panel was redesigned. One panel is shared by live streams and videos: Quality, Playback speed, Sleep timer, Skip interval, Player controls and Chat width.

### Added
- Saved videos you have already started show a **Resume** button.

### Fixed
- Dragging a slider in the settings panel no longer scrolls the page behind it.

## 0.5.0 (versionCode 5)

### Added
- **BetterTTV and FrankerFaceZ global emotes** in chat, next to the original's 7TV emotes.

### Fixed
- One broken 7TV entry could drop a streamer's whole 7TV emote set. It is now skipped on its own.

## 0.4.0 (versionCode 4)

### Added
- **Playback speed for live streams** (0.5x to 1.25x).

### Fixed
- The skip interval and the Controls / Gestures choice were forgotten after every video. They are now remembered.

## 0.3.0 (versionCode 3)

### Added
- **Saved videos**: a bookmark button on every video, and a Saved screen opened from the top bar on Home.
- **Recent searches** in Search, shown as chips that run the search again in one tap.

## 0.2.0 (versionCode 2) - first release of the fork

### Changed
- Renamed to **Kick Companion**, with its own app id (`com.kickcompanion.appv2`) so it installs next to the original app.
- Redesigned dark interface with shared colours (`theme.ts`), a themed exit confirmation, and a redesigned About screen.
- **Home** now has Following and Live tabs. The Live tab shows Kick categories (icon, viewer count, tags) and opens a category's live streams. Followed streamers load in parallel, no longer duplicate, and refresh when you return to the screen.
- **Streamer cards**: Follow / Unfollow updates instantly, a green ring shows when a streamer is live, and tapping the avatar opens the live stream, or the videos if they are offline.
- Search has a small back icon instead of a full-width button.

### Added
- **Player**: quality picker (using the player's track selection, so switching doesn't restart the video), playback speed for videos, a skip interval you choose, Controls or Gestures mode, a sleep timer, and a chat width slider for landscape.
- **Picture-in-picture** when you leave the app, and an **audio-only mode** (headphone icon).
- Chat reconnects by itself with a growing delay when the connection drops.
- Plugins for picture-in-picture, display cutout, Media3 version, Flipper version, black window background and the PiP background-audio action; `react-native-video` pinned to 6.19.2; `expo-screen-orientation`; a Gradle command for the preview APK in `eas.json`.

### Fixed
- Video durations were shown about 1000 times too long (Kick sends milliseconds).
- A short buffering stall could leave a live stream paused until you pressed play.
