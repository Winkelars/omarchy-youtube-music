![YouTube Music popup](preview.png)

# Omarchy YouTube Music

A YouTube Music controller for the Omarchy Quickshell bar.

The plugin reads the browser's live MPRIS player. It shows the current track,
album art, artist, album, progress, and playback controls in a popup panel.

## Requirements

- Omarchy Quattro with Quickshell
- YouTube Music playing in one of:
  - **Pear Desktop**, with its Shortcuts plugin enabled (see below)
  - Brave, Google Chrome, Chromium, Firefox, Vivaldi, or Zen
- A player that exposes the current media tab through MPRIS

The plugin does not start a background service, use browser automation, or
store account data. It activates when MPRIS identifies YouTube Music, reports a
`music.youtube.com` URL, or -- for Chromium-family browsers that expose neither,
such as Google Chrome, Chromium, Brave, and Vivaldi -- when a browser player
reports a non-empty `xesam:album`, which YouTube Music sets and regular videos
leave empty, or when an open window is titled `<current track> | YouTube Music`.
The window check covers podcast episodes, which have no album, in the YouTube
Music web app or a focused browser tab.

## Pear Desktop

[Pear Desktop](https://github.com/pear-devs/pear-desktop), the Electron wrapper
formerly released as `th-ch/youtube-music`, publishes YouTube Music over MPRIS
itself. That is the better path: the panel talks to the app's player rather than
to a browser tab, so album art, track length, and position come straight from
the app and seeking does not depend on window-title heuristics.

Pear only registers that player from its **Shortcuts** plugin, which is
disabled by default:

1. In Pear Desktop, open *Settings → Plugins* and enable **Shortcuts**.
2. Restart Pear Desktop.
3. Start a track. The bar entry appears as soon as the player is exported.

The plugin recognises Pear by MPRIS identity, by the
`org.mpris.MediaPlayer2.YouTubeMusic` bus name, or by the `youtube-music` /
`pear-desktop` desktop entry, so detection survives a rename of any single one.

## Install

```sh
omarchy plugin add https://github.com/levyvix/omarchy-youtube-music.git --enable
```

Place it in the bar if needed:

```sh
omarchy bar move levi.youtube-music --section left
```

## Controls

- Left click the music icon to open or close the panel.
- Middle click toggles play/pause.
- Right click plays the next track.
- `Space` toggles play/pause.
- `n` or Right arrow plays the next track.
- `p` or Left arrow plays the previous track.
- Drag the progress bar to seek.
- `Escape` closes the panel.

The panel keeps the last detected player while it remains connected *and* still
reports YouTube Music, so pausing a track does not make the panel appear idle,
while switching that browser tab to a regular video clears it. Pear Desktop and
the Omarchy web app always qualify, since they serve nothing else. Long track
labels are truncated in the bar and remain available in the tooltip and panel.

## Validate locally

```sh
omarchy plugin validate .
qmllint -I "$OMARCHY_PATH/shell" Panel.qml
```

## Remove

```sh
omarchy plugin remove levi.youtube-music
```

## License

MIT. See [LICENSE](LICENSE).
