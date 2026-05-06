# G Chase: GPS Tag

A real-time multiplayer GPS tag game played outdoors. One player is the **hare**, the rest are **hounds** — hounds chase the hare, and whoever captures it becomes the new hare.

## Play

Open in a mobile browser (HTTPS required for GPS):  
👉 https://haywardt.github.io/LocationGame

## How It Works

- The game world is a **20×20 toroidal grid** (edges wrap around)
- Your **GPS heading** (direction of travel) is your control — you must physically walk to move
- Capture happens when a hound gets within **1 grid unit** of the hare
- On capture, the hound becomes the new hare

## Controls

| Control | Action |
|---|---|
| ↺ / ↻ buttons | Manually adjust heading ±30° |
| GPS: ON/OFF | Toggle real GPS heading vs. simulated drift |
| Enable Geolocation | Grant location permission and map GPS to grid |
| Face North/East/South/West | Set a fixed simulated heading for testing |
| Join Game | Enter the current game session |
| Reset | Clear all game state |
| 🔊 Speech | Toggle audio announcements |

## Audio Navigation

When speech is enabled, the game announces other players every 15 seconds:

> *"You are the hound. Hare at 4.2 units, 2 o'clock."*

Bearings are given as clock positions relative to your current heading.

## Radar Display

The circular radar shows all players relative to your position. You are always at the center (green arrow). Other players appear as colored icons — **gold** for hare, **red** for hound — rotated to show their heading.

## Multiplayer Modes

**Firebase mode** (primary): Live sync via Firebase Cloud Functions. Requires a configured Firebase project — update `firebaseConfig` in `index.html` with your own credentials.

**localStorage mode** (fallback): Works across browser tabs on the same device. No server needed. Useful for local testing.

## Requirements

- Mobile browser with Geolocation API support (Chrome, Safari iOS, Firefox Mobile)
- HTTPS connection
- Location permission granted
- Must actually walk outdoors — GPS heading requires movement

## Setup (self-hosting)

1. Clone the repo
2. Replace the `firebaseConfig` placeholder values in `index.html` with your Firebase project credentials
3. Deploy your Firebase Cloud Function (`updateGame`)
4. Host `index.html` on any HTTPS server (GitHub Pages works out of the box)

## Tips

- Walk steadily in the direction you want to move — the GPS heading updates every 2 seconds
- Standing still or rotating in place won't change your position
- Use the manual heading buttons for indoor testing
- If GPS is unavailable, the game falls back to simulated heading drift
