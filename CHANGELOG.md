# Changelog

Every version of this fork, newest first. The starting point is the original app, Stream Companion 1.0.12 (last commit `ad887b2`, August 2024). The first version of this fork was 0.2.0.

Each version has its own `versionCode`, so a new APK installs over the old one.

## 0.10.1 (versionCode 13)

**New**
- **Check for updates** (Settings > Updates). Tap it to see if a newer version is on GitHub. If there is one, "Open the download page" opens it in your browser. It only runs when you tap it, and it never downloads or installs anything by itself.

**Good to know**
- It reads release tags that look like a version number, such as `v0.10.1`. Other tags (such as `New`), drafts and pre-releases are ignored.
- It tells you plainly when you already have the latest version, when your version is newer than the newest release, when nothing is published yet, when there is no connection (it gives up after 12 seconds), and when GitHub is limiting requests.
- The player, chat, `plugins/` and `package.json` are untouched: no new libraries, no native changes.

## 0.10.0 (versionCode 12)

**New**
- **Continue watching** (Home > Following): up to 10 videos you started and didn't finish, newest first, with a progress bar and the time left. Tap one to carry on, like the Resume button. The x removes it, and it comes back only if you watch more.
- **Progress bars** on the pictures in a streamer's video list.
- **Following order**: Live first (most viewers on top, then A-Z), A-Z, or Added (the order you followed them). Your choice is remembered. It shows when you follow two or more streamers.
- **Pinned streamers**: tap the pin next to a name to keep that streamer at the top in every order.
- **Live alerts > Streamers** (Settings > Notifications): choose which of the streamers you follow can alert you, with All on and All off buttons.
- **Live alerts > Quiet hours**: pick a From and an Until hour (it can run past midnight). Alerts still arrive, but without sound or vibration. The test notification tells you when it was sent silently.
- **Backup and restore** (Settings > Backup). "Back up your data" opens Android's share menu with your follows, pins, saved videos, resume spots, the Continue watching row, recent searches and settings, as text. "Restore from a backup" takes that text back. It only adds what the phone is missing, and skips anything that doesn't look right. Restart the app afterwards.

**Changed**
- Following now starts in Live first order. Choose Added to get the old order back.

**Good to know**
- A video counts as started after 10 seconds, and as finished within 30 seconds of the end.
- Videos you watched before this version appear in Continue watching after you open that streamer's video list once.
- A switched-off streamer stays in your Following list. Switching one back on doesn't announce a stream that was already running.
- Quiet-hours alerts use their own notification channel, "Live streams (quiet hours)".
- The player, chat, `plugins/` and `package.json` are untouched: no new libraries, no native changes.

## 0.9.1 (versionCode 11)

**Fixed**
- The About screen no longer says the app is for TV platforms, no longer links to the original's website, and no longer repeats the original's plan for other streaming platforms. It now says Kick Companion is an Android fork of Stream Companion, and "Open Original Project" opens the original's GitHub page.

Nothing else changed from 0.9.0.

## 0.9.0 (versionCode 10)

**New**
- **Live alerts**: a notification, with the stream title and the game, when a streamer you follow goes live. Tap it to open the stream, even if the app was closed.
- The phone does the checking, with no server: about every 10 minutes in the background and every 5 minutes while the app is open.
- Only a streamer who just went live is announced. The first look after you switch alerts on is silent, the same streamer isn't announced again within 20 minutes, and one check shows at most five alerts (any more are grouped into one "N more streamers are live").
- **Settings > Notifications**: a switch (on by default), "Send a test notification", and "Check now", which says what it found (no follows, nobody live, alerts sent, nothing new, or that Kick could not be reached).
- A new app icon (a green ring with a play mark on black) that matches the launch animation, and a matching notification icon.
- This README and this changelog.

**Fixed**
- Resuming a video no longer shows its first seconds and then jumps to your saved time. It starts at the saved position, and chat starts from the same point instead of replaying the beginning.

**For developers**
- New dependencies: `expo-notifications`, `expo-task-manager`, `expo-background-fetch`.

## 0.8.1 (versionCode 9)

**Fixed**
- The 0.8.0 build failed with `Could not find com.facebook.react:flipper-integration`. Removing the TV plugin in 0.8.0 had also removed the step that takes Flipper (a debug-only tool) out of the generated Android project. A small new plugin, `plugins/withRemoveFlipper.js`, does that one job. Nothing else changed. Use this version instead of 0.8.0.

## 0.8.0 (versionCode 8) - did not build, use 0.8.1

**New**
- **Themes** (Settings > Appearance): AMOLED Black (default), Dark, Light and Material You (Android 12+). If Material You ever fails to start, the app goes back to AMOLED Black on the next launch.
- **Search page redesigned**: recent searches with a Clear button, an x in the box, and clear loading, no-results and error states with Retry. An older search can never overwrite a newer one.
- **Launch animation**: a short black screen with a green play mark and a soft ring that fades out when the app is ready.
- Volume keys, and a headset's volume buttons, control the media volume.
- The Claude icon next to "Modified by Claude" in About.

**Fixed**
- Audio stopping 5 to 10 seconds after using the PiP headphone button. The player now keeps the phone's CPU and Wi-Fi awake while it plays.
- Chat names are darker on the light themes, so they stay readable.

**Changed**
- Android only: the iOS configuration, the iOS and TV images, the TV run script and the TV plugin were removed.
- The APK is built for arm64-v8a only, which took it from about 75 MB to about 30 MB.
- `.npmrc` sets `legacy-peer-deps=true`.

## 0.7.0 (versionCode 7)

**New**
- A **headphone button in the PiP window** that switches to background audio. (It never showed before, because the player library replaced the button list.)
- A **media card in the notification shade** (title, streamer, play/pause), with a switch in Settings. Android 13 and newer asks for notification permission when the app opens, because the card is a notification.
- A **Settings screen**: the gear icon next to the info icon on Home.
- The Claude icon next to "Modified by Claude" on the About screen.
- An eye icon with the viewer count on streamer cards, instead of the word "Viewers".

**Fixed**
- Live chat not working. The connection was rewritten: it reconnects by itself, tries alternative connection keys, and shows a short notice with the reason if chat can't connect.

## 0.6.0 (versionCode 6)

**Changed**
- The player settings panel was redesigned. Live streams and videos share one panel: Quality, Playback speed, Sleep timer, Skip interval, Player controls and Chat width.

**New**
- A **Resume** button on saved videos you have already started.

**Fixed**
- Dragging a slider in the settings panel no longer scrolls the page behind it.

## 0.5.0 (versionCode 5)

**New**
- **BetterTTV and FrankerFaceZ global emotes** in chat, next to the original's 7TV emotes.

**Fixed**
- One broken 7TV entry could drop a streamer's whole set of 7TV emotes. It is now skipped on its own.

## 0.4.0 (versionCode 4)

**New**
- **Playback speed for live streams** (0.5x to 1.25x).

**Fixed**
- The skip interval and the Controls / Gestures choice were forgotten after every video. They are now remembered.

## 0.3.0 (versionCode 3)

**New**
- **Saved videos**: a bookmark button on every video, and a Saved screen opened from the top bar on Home.
- **Recent searches** in Search, as chips that run the search again in one tap.

## 0.2.0 (versionCode 2) - first version of the fork

**Changed**
- Renamed to **Kick Companion**, with its own app id (`com.kickcompanion.appv2`), so it installs next to the original app.
- A new dark look, a themed exit confirmation and a redesigned About screen.
- **Home** has Following and Live tabs. Live shows Kick categories (icon, viewer count, tags). Tap one to see its live streams.
- Followed streamers load in parallel, no longer repeat, and refresh when you come back to the screen.
- **Streamer cards**: Follow / Unfollow updates at once, a green ring shows when a streamer is live, and tapping the avatar opens the live stream (or the videos, if they are offline).
- Search has a small back icon instead of a full-width button.

**New**
- **Player**: a quality picker (switching doesn't restart the video), playback speed for videos, a skip interval you choose, Controls or Gestures mode, a sleep timer, and a chat width slider for landscape.
- **Picture-in-picture** when you leave the app, and an **audio-only mode** (headphone icon).
- Chat reconnects by itself, with a growing delay, when the connection drops.

**Fixed**
- Video durations were shown about 1000 times too long (Kick sends milliseconds).
- A short buffering stall could leave a live stream paused until you pressed play.

**For developers**
- Plugins for picture-in-picture, display cutout, Media3 version, Flipper version, black window background and the PiP background-audio action. `react-native-video` pinned to 6.19.2, `expo-screen-orientation` added, and a Gradle command for the preview APK in `eas.json`.
