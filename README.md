# Natsukashi: a 90s twin-deck mixtape

A single-page, 90s British mini hi-fi with two cassette decks, a Ceefax-style teletext tracklist and a CRT telly that plays your own video files. No build step, no dependencies, one HTML file.

- **Deck A** holds the **Natsukashi** tape.
- **Deck B** holds the **Recommendations** tape.
- Each tape has one song per video.

## Project layout

```
.
├── index.html
├── README.md
└── videos/
    ├── wonderwall.mp4
    ├── wannabe.mp4
    └── ...
```

`index.html` must keep that name so GitHub Pages serves it as the home page.

## Add your videos

1. Put your video files in the `videos/` folder.
2. Open `index.html` and find the `T` array near the top of the `<script>`:

   ```js
   const T=[
    {name:'Natsukashi', sub:'なつかしい · the good old days', stripe:'#e4352b', fs:31, tracks:[
     {src:'videos/wonderwall.mp4', t:'Wonderwall', a:'Oasis'},
     ...
    ]},
    {name:'Recommendations', ... }
   ];
   ```

3. For each track set:
   - `src`: path to the video file, relative to `index.html`
   - `t`: song title (shown on the teletext)
   - `a`: artist
   - `r`: reason (optional), a line about why you picked the song. It scrolls across the deck's display

Add or remove tracks freely. The teletext page numbers and the tape-reel animation adjust automatically. You can also change a tape's `name`, the handwritten subtitle `sub`, the label `stripe` colour, and `fs` (the label font size, which you can lower if a long name doesn't fit).

### Video format

Use H.264 video with AAC audio in an `.mp4` container, which plays in every browser. `.webm` works in most.

To shrink a video for the web with [ffmpeg](https://ffmpeg.org/):

```sh
ffmpeg -i input.mov -c:v libx264 -crf 26 -preset slow -vf scale=-2:480 \
       -c:a aac -b:a 128k -movflags +faststart videos/song.mp4
```

`+faststart` lets the video start playing before it has fully downloaded.

### Size limits

GitHub rejects files over **100 MB**, and a Pages site should stay under about **1 GB** in total. A 480p video of a few minutes usually comes out at 10 to 30 MB.

## Run it locally

Because the videos are local files, you can just double-click `index.html`. Or serve the folder if you prefer:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## Publish on GitHub Pages

1. Create a repository and push `index.html`, `README.md` and the `videos/` folder.
2. On GitHub, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick your main branch and the `/ (root)` folder, then save.
4. After a minute or two the site is live at `https://<your-username>.github.io/<repository-name>/`.

## How to use it

| Control | What it does |
| --- | --- |
| Previous / next keys | Previous / next track on that tape |
| `◀◀` / `▶▶` | Rewind / fast forward (sound is muted while winding). Press again to resume playing |
| `▶` | Play |
| `❚❚` | Pause. Press again to resume |
| `■` | Stop and rewind to the start of the track |
| Teletext track list | Click a track to play it on the tape shown |
| Red / green teletext keys | Switch the teletext to the Natsukashi / Recommendations tape |
| Yellow teletext key, or **Hide video / Show video** | Hide or show the telly. Audio keeps playing while hidden |
| Cyan teletext key, or **Full screen** | Full-screen the video. Press `Esc` to leave |
| **Super Bass** | Boosts the low end of the light show. It's cosmetic and doesn't change the sound |
| Volume | Output volume |

Only one deck plays at a time. Starting a deck pauses the other where it was, and it picks up from that spot when you press play on it again. When a track ends the tape moves to the next one, and at the end of the tape it stops and rewinds.

## Extra features

- **Seek bar:** each deck has a tape-position bar with elapsed and remaining time. Click it to jump.
- **Repeat:** cycles off, repeat the tape, or repeat one track.
- **Shuffle:** plays tracks in random order.
- **Chain decks:** when one tape ends, the other deck starts.
- **Mute** and a **Pitch** slider (±10%). Pitch behaves like a real tape deck, so speed and pitch move together. Double-click the slider to reset it.
- **Add videos:** pick video or audio files from your computer to add to the tape shown on the teletext. They last until you reload the page, and they aren't uploaded anywhere.
- **On-video controls:** move the mouse over the video (or tap it) to get transport buttons, a seek bar, mute and full screen on the picture itself, including in full screen. Click the picture to play or pause, and double-click it for full screen.
- **Edit tapes:** the **Edit tapes** button (or `E`) opens a form where you can change each song's name, artist and reason. Changes show instantly and are remembered in your browser only. To make them permanent for everyone, press **Copy tapes code** and paste it over the `const T=[...]` block in `index.html`, then commit and push. **Reset my edits** clears what your browser saved.
- **Reason marquee:** each deck has a scrolling display showing the reason for the current song (or its name and artist if there isn't one).
- **Pop out:** floats the video in a small window (Picture-in-Picture) where the browser supports it.
- **Lock-screen and media-key controls:** the current title and artist show up in your OS or phone media controls.

## Keyboard shortcuts

Space play/pause · ←/→ seek 5s (Shift for 30s) · N / P next / previous · S stop · M mute · F full screen · V show/hide video · 1 / 2 switch tape · ↑/↓ volume · R repeat · E edit tapes

## Troubleshooting

- **"FILE NOT FOUND - SKIPPING"**: the `src` path is wrong, or the file isn't in the repo. Paths are case-sensitive on GitHub Pages, so `Song.mp4` and `song.mp4` are different files.
- **Black screen but you can hear sound**: the video codec isn't supported. Re-encode it as H.264/AAC `.mp4` using the ffmpeg command above.
- **Video won't start on the first press**: browsers only allow playback after you interact with the page. Press play once and it works from then on.
- **Full screen doesn't work on iPhone**: iOS doesn't allow full screen on a page element, so the button falls back to the phone's own video player.
- **The spectrum lights aren't reacting to the music**: they're animated for show and don't analyse the audio.

## Credits

Built as a nostalgia piece: teletext display, hi-fi stickers, CRT telly. All brand names in the artwork are made up.
