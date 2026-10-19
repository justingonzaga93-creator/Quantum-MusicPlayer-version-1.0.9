# Quantum Music Player v1.0.7 — Refresh + YouTube Online

## What's included
- Refresh + YouTube Onlineed dark neon interface with Home, Playing, Library, YouTube Online, Studio, and More pages.
- Four selectable themes: Quantum Purple, Ocean Blue, Solar Ember, and Midnight Mono.
- Animated page transitions, album-art transitions, button feedback, live visualizer, and reduced-motion setting.
- Custom artwork per song and embedded MP3 ID3 artwork where recognized. Custom artwork takes priority; otherwise embedded artwork is used, then the built-in Quantum fallback cover.
- Local audio library stored in IndexedDB, favorites, playlists, search, playback controls, sleep timer, bass/treble effects, and Media Session controls where supported.
- YouTube Online: search videos using YouTube Data API v3 and play selected videos in YouTube’s embedded player. Does not download or extract YouTube audio.
- API key is entered in the app and stored in this browser only. For safety, restrict the key to YouTube Data API v3 and authorized websites when hosted; never publish an unrestricted key.
- Library backup export and restore for song records, artwork, favorites, playlists, and theme preference. Keep your original audio files separately; browser storage limits vary.
- About & Legal page with privacy/storage explanations and the requested creator credit.
- Service-worker cache name updated so existing installs can receive the refreshed app shell.

## Run locally
1. Extract this ZIP.
2. Serve this folder with a local static web server (for example, `python -m http.server 8000`).
3. Open `http://localhost:8000` in a modern browser.
4. Add audio files, test playback, then check favorites, playlists, artwork, backup/restore, themes, and settings.

Opening `index.html` directly supports basic testing, but service workers and PWA behavior require localhost or HTTPS. Keep using the same browser and app origin if you want to retain the existing local library.

## Important storage notes
- This is a plain HTML/CSS/JavaScript web app, not an APK.
- Audio and artwork are stored in the browser's IndexedDB under this app origin. Clearing site data, switching browsers/profiles, private browsing, or opening the app from another origin can make that library unavailable.
- Export a backup before major updates. Backups can be large. Keep original music files outside the app too.
- The backup restore asks for confirmation and updates records by song ID; do not delete your backup until you have checked the restored library.
- Embedded artwork parsing currently targets common ID3v2 APIC artwork in MP3 files. Other audio formats may use the default cover unless custom artwork is selected.

## Android / background playback
This repository is a web app and does not include an Android Gradle project or APK. Background playback and lock-screen controls depend on the browser/WebView and Android lifecycle. Reliable native background playback requires an Android wrapper and device testing.


## v1.0.8 update
- Moved YouTube Data API key configuration from the Online page into a dedicated Secure Settings page accessible from More.
- Online search now reads the saved key automatically, so users do not see or paste the key on the Online page.
- Existing saved keys use the same browser storage key and should remain available on the same app origin.
- Note: hiding the key field is a UI change, not true secret storage. Browser JavaScript API keys can still be extracted; restrict the key in Google Cloud Console or use a backend for public distribution.


## v1.0.9 update — Cloudflare search backend
- YouTube search now calls the Cloudflare Worker endpoint `https://quantum-music-search.larnqua43.workers.dev/search` instead of calling Google directly from the app.
- The app no longer asks users to enter or store a YouTube API key. The key belongs in the Cloudflare Worker secret named `YOUTUBE_API_KEY`.
- Existing local library, playback, playlists, themes, and embedded YouTube playback code are preserved.
- The Worker must be deployed with the `/search?q=...` route code and the `YOUTUBE_API_KEY` secret configured. If the Worker URL or route changes, update `SEARCH_ENDPOINT` in `script.js`.
- This backend hides the API key from the app, but the endpoint is public and can consume YouTube API quota. Add abuse protection/rate limiting before broad public release. This app searches and embeds YouTube videos; it does not download or extract YouTube audio.
