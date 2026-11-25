# Tabdeck

A single-file web page for practising guitar to Spotify. It shows what is playing, keeps your own tabs and chords for each song, and adds playback controls, a live spectrum with tempo and key estimates, and quick links to Ultimate Guitar.

Everything runs in your browser on your own computer. There is no server, no account, and no tracking.

## Features

- **Now playing:** large album art, title, artist and album details, updated from Spotify.
- **Your tabs:** paste the chords or tab for a song once, and Tabdeck shows it every time that song plays. If the top of the text mentions the capo, key, tempo or tuning, those are filled in for you.
- **Song info:** small fields for key, BPM, capo, tuning and notes, saved per song.
- **Ultimate Guitar links:** save the page address of the version you like, then open it in one click. "Open all versions" runs an Ultimate Guitar search for the song. Tabdeck only opens links. It does not fetch anything from the site.
- **Playback controls:** previous, back 10 seconds, play/pause, forward 10 seconds, next, and a seek bar. Keyboard: `Space` plays and pauses, `Left` and `Right` jump 10 seconds.
- **Recently played:** your last five songs and their artists. Click a song to play it. The guitar icon on each song opens its Ultimate Guitar page.
- **Live spectrum:** a colourful frequency display from your microphone, with the kick and snare ranges emphasised, an estimated BPM, and an approximate musical key. One button saves the estimates into the song info.

## Easiest way: use a hosted copy

If someone has put Tabdeck online (for example on GitHub Pages) and added your Spotify account to their Spotify app, you do not need to install anything. Open the link they give you and press **Log in with Spotify**.

If the owner shares a link ending in `?cid=` followed by their Spotify Client ID, Tabdeck fills the Client ID in for you.

Spotify limits this. A Spotify app in development mode can only be used by its owner and up to five people the owner adds by name and email under **Users Management**. See the notes below.

## What you need

- A Spotify account. Click-to-play and the playback buttons need **Spotify Premium**, and Spotify must be open on a device.
- A Spotify developer app (instructions below). Under Spotify's 2026 rules, the account that owns the app needs **Spotify Premium**, each developer can have a limited number of apps, and each app can have only **five** authorized users. People you add do not need Premium themselves.
- To run it on your own computer: Python 3, or any other way of serving a folder over HTTP. Not needed if you use a hosted copy.
- A recent browser. It was built and used in Chrome on macOS. It should also work in Chrome and Edge on Windows, but that has not been tested, and Firefox and Safari are untested too.

## Setup

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and create an app.
2. In the app's settings, add this **Redirect URI** exactly, including the trailing slash, then save:

   ```
   http://127.0.0.1:8888/
   ```

   Spotify only accepts the numeric loopback address here, not `localhost`.
3. Tick **Web API** as the API the app uses.
4. Copy the app's **Client ID**. You do not need the client secret, and it should never go into this project.
5. In a terminal, from the folder that holds `index.html`:

   ```
   python3 -m http.server 8888
   ```

   On Windows the command is usually `python -m http.server 8888` or `py -m http.server 8888`. Any tool that serves the folder at `http://127.0.0.1:8888/` works, such as the Live Server extension in VS Code. Opening `index.html` by double-clicking will not work, because Spotify's login needs that web address.

6. Open <http://127.0.0.1:8888> in your browser, paste the Client ID, and press **Log in with Spotify**.

If you use a different port, change it in the command and in the Redirect URI to match.

### Hosting it yourself (for example GitHub Pages)

1. Put `index.html` in a public GitHub repository and turn on **Settings, Pages**, deploying from the `main` branch.
2. In your Spotify app's settings, add your Pages address as a Redirect URI, for example `https://your-name.github.io/your-repo/` (with the trailing slash). The login screen shows the exact address to use.
3. Under **Users Management**, add each person who will use it (up to five).
4. Share `https://your-name.github.io/your-repo/?cid=YOUR_CLIENT_ID`. A Client ID is a public identifier, not a secret, and it is never stored in the project files. Never share the client secret.

Everyone's tabs, links and notes stay in their own browser. Nothing is stored on a server.

## Using it

- Play a song in Spotify. Tabdeck picks it up within a few seconds.
- Press **Paste a tab**, paste your chords or tab, and save. It is stored for next time.
- Press **Open all versions** to browse versions on Ultimate Guitar. Copy the address of the one you like into the link box on the left and press **Save**. After that, **Open saved version** goes straight to it.
- Turn on **Spectrum** and allow the microphone. The tempo needs several seconds of music to settle. If the estimate is half or double the real tempo, press **½** or **×2**, then use **Save key & BPM to song info**.

## Privacy

- Your tabs, links, notes, Spotify login tokens and settings are stored in your browser's local storage for `127.0.0.1:8888`. They never leave your computer, apart from the requests Tabdeck makes to Spotify.
- Microphone audio is analysed live in the browser. It is not recorded or sent anywhere.
- To start fresh, clear the site data for `127.0.0.1:8888` in your browser.

## Limits and known issues

- **Key and BPM are estimates.** Spotify no longer gives key and tempo data to newly created apps, so Tabdeck listens through your microphone instead. It hears the room, so it works best with clear drums and chords and no other noise. Treat the numbers as a starting point.
- **Recent plays can lag.** Spotify records a play only after you have listened for a while, so a song may appear in Spotify's own history late. Tabdeck also adds songs it sees playing, but it can only do that while it is open.
- **Ultimate Guitar** does not allow its pages to be shown inside other sites, and it has no public API. Tabdeck therefore opens its pages in a normal browser tab, and you copy tab text across yourself. Please respect Ultimate Guitar's terms of use.
- Spotify's rules for developer apps change over time. If login or playback stops working, check the current [Spotify developer terms](https://developer.spotify.com/terms) and your app's settings.

## Troubleshooting

| Problem | What to try |
|---|---|
| "Invalid redirect URI" when logging in | The Redirect URI in your Spotify app must be exactly `http://127.0.0.1:8888/`. |
| Login works for you but not a friend | Add their Spotify name and email under Users Management in your Spotify app. Each app allows five people. |
| Recent plays say Spotify needs permission | Press **Allow recent history** and approve the extra permission. |
| Clicking a song does nothing | Playback control needs Premium and an active device. Press play once in Spotify, then try again. |
| The page looks out of date after an update | Hard refresh with Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows). |
| Spectrum is empty | Allow the microphone for the page in your browser, and for the browser in your system settings (macOS: Privacy & Security, Microphone. Windows: Privacy & security, Microphone). |
| `python3` is not recognised on Windows | Use `python` or `py` instead, or install Python from python.org. |

## Project layout

```
index.html   the whole app (HTML, CSS and JavaScript)
README.md    this file
```

## Disclaimer

Tabdeck is an independent project. It is not affiliated with, endorsed by, or sponsored by Spotify or Ultimate Guitar. Spotify and Ultimate Guitar are trademarks of their respective owners.

## Licence

<!-- Add the licence you choose here, and a LICENSE file alongside this README. -->
