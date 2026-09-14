# Tigor Drive

A free, personal CarPlay-inspired dashboard for using an iPhone (mounted landscape) in a Tata Tigor. It's a static PWA — no login, no subscription, no backend.

```
[ dock ] [        map        ] [ music ]
[ dock ] [        map        ] [weather]
[            status bar             ]
```

## Install on iPhone

1. Open the GitHub Pages URL in **Safari**.
2. Tap **Share → Add to Home Screen**.
3. Launch it from the Home Screen icon — it opens straight into the dashboard, full-screen, no browser chrome.
4. Mount the phone landscape. If it's ever opened in portrait, it shows a "rotate your iPhone" screen instead of a broken layout.

## What's on the dashboard

- **Left dock** — Google Maps, Spotify, Phone, and a "you're already here" Dashboard button. Large 44px+ touch targets, one active-state highlight at a time.
- **Center map** — a live OpenStreetMap view (via Leaflet, vendored locally, no API key) that follows your GPS position with a location marker and accuracy ring. A "Google Maps" card at the bottom launches turn-by-turn navigation to nowhere-in-particular by default, or centers on your live position — tapping it, or the dock icon, opens the native Google Maps app (`comgooglemaps://`) and falls back to the Google Maps website if the app isn't installed. If location is denied or unsupported, the map area is replaced by a clear "map needs location access" panel with the same Google Maps button, instead of a fake map.
- **Right column** — a Spotify panel (art glyph, prev/play/next, "Open Spotify") that launches the Spotify app (`spotify:`) and falls back to `open.spotify.com`; and a weather card using the free Open-Meteo API (no key, no account) once location is available. If weather can't load, it says so and the rest of the dashboard keeps working.
- **Status bar** — clock, live GPS speed in km/h (from `navigator.geolocation`, shows `--` rather than crashing when denied), battery percentage (from the Battery API where iOS/Safari exposes it, shows `—` otherwise), and a GPS state indicator.

## Honest limitations

This is a web app, not real Apple CarPlay, and it says so nowhere claims otherwise:

- It can't embed Google Maps' or Spotify's native app UI — it shows a live OpenStreetMap view for context and hands off to the real apps via deep links for actual navigation and playback.
- It can't read or control what's playing in Spotify (iOS doesn't allow that from a website). The play/prev/next buttons all just open the Spotify app.
- Speed, GPS, weather and battery all depend on the person granting the relevant browser permission, and each one fails gracefully (a dash, a status message) rather than crashing the dashboard if it's denied or unavailable.

## File structure

```
index.html              the entire dashboard (HTML + CSS + JS, no build step)
manifest.json            PWA manifest (standalone, landscape, icons)
icons/                   app icons (192, 512, maskable 512, apple-touch-icon)
vendor/leaflet/          Leaflet 1.9.4 (map library), vendored locally so the
                         dashboard doesn't depend on a CDN being reachable
                         with a weak signal in the car
README.md               this file
```

No backend, no build tools, no frameworks. Everything is plain HTML/CSS/vanilla JS and works directly from GitHub Pages.

## Notes on this rebuild

I didn't receive the two screenshots referenced in the brief (only the text came through), so this rebuild follows the detailed written spec directly: fixed left dock, large central map, stacked music/weather on the right, dark automotive styling, safe-area-aware full-bleed layout. If the actual visual reference differs meaningfully from this, tell me what to adjust and I'll iterate.
