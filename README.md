```markdown
# GPS Course Navigator

A real-time navigation game that uses your **GPS course over ground** (`position.coords.course`) as the control mechanism. You must physically walk to move through the game world. Find treasure, avoid bandits, and explore using only your direction of travel and audio cues.

## 🎮 Game Overview

You control a player in a **20×20 toroidal world** (edges wrap around). Your movement direction is determined by your actual walking direction (GPS course). The game speaks navigation instructions to guide you.

**Objectives:**
- 🏆 Find the treasure – it respawns in a new location each time you collect it
- 🚫 Avoid bandits – they chase you when you enter their detection range

## 🧭 Core Mechanics

| Mechanic | Description |
|----------|-------------|
| **Control method** | GPS course over ground (`coords.course`) |
| **Movement required** | You must be physically walking |
| **Update interval** | Every 15 seconds |
| **World size** | 20×20 units (toroidal) |
| **Bandits** | 2 bandits, 5-unit chase range |
| **Feedback** | Speech synthesis (clock-face directions) |

### How Movement Works

1. Walk east → GPS course ≈ 90° → player moves east in-game
2. Turn and walk north → GPS course ≈ 0° → player moves north
3. Stand still → `course = null` → player stops moving

**Simply rotating in place does nothing.** You must change your actual walking direction.

## 🔊 Audio Navigation System

The game speaks cues in this format:

```

"[target] at [clock] o'clock [distance] clicks"

```

- **Clock position** – Direction relative to your current course  
  (12 o'clock = straight ahead, 3 o'clock = directly to your right)
- **Clicks** – Rounded distance to target (lower = closer)

### Examples

| Audio Cue | Meaning |
|-----------|---------|
| `"treasure at 12 o'clock 3 clicks"` | Treasure is straight ahead, 3 units away |
| `"bandit at 4 o'clock 1 click"` | Bandit is close, to your right-rear |
| `"treasure at 9 o'clock 7 clicks"` | Treasure is to your left, far away |

## 📱 Requirements

### Hardware
- Device with **GPS receiver** (smartphones, tablets, many laptops)
- **No compass required** – uses GPS course only

### Movement
- You **must be walking** for course data to update
- Stationary = no movement in game
- Speed doesn't matter – only direction counts

### Browser & Environment
- HTTPS connection (required for Geolocation API)
- User permission for location access
- Clear sky view (GPS works best outdoors)

### Browser Support
| Browser | Support |
|---------|---------|
| Chrome (Android) | ✅ Full |
| Edge (Android) | ✅ Full |
| Safari (iOS) | ✅ Requires permission |
| Firefox (Mobile) | ✅ Good |

## 🚀 How to Play

1. **Open the game** in a mobile browser
2. **Allow location access** when prompted
3. **Go outside** – GPS needs sky view
4. **Start walking** in any direction
5. **Listen to audio cues** – they tell you where to go
6. **Change direction** by physically turning and walking a different way
7. **Collect treasure** to make it respawn elsewhere
8. **Avoid bandits** – if they get too close, change course!

## 📊 Display

The screen shows one thing: your current GPS course in degrees.

| Display | Meaning |
|---------|---------|
| `342°` | Walking northwest |
| `---` | No GPS course (stationary or no lock) |
| `ERR` | Geolocation error |

## 🛠️ Technical Details

### The Core Code

```javascript
navigator.geolocation.watchPosition(
  (position) => {
    if (position.coords.course !== null) {
      player.heading = position.coords.course;
    }
  },
  (error) => { /* handle error */ },
  { enableHighAccuracy: true }
);
```

What is coords.course?

Property Value
Definition GPS course over ground (direction of travel relative to true north)
Source Calculated from successive GPS position fixes
Range 0–360 degrees
Null when Stationary or insufficient GPS signal
Requires Physical movement

Game Loop (15-second interval)

1. Move player in direction of current GPS course
2. Check if treasure collected (distance < 1 unit)
3. Move bandits (chase player if within detection range)
4. Speak navigation updates via speech synthesis
5. Repeat

⚠️ Troubleshooting

No course data (shows "---")

Fix Explanation
Start walking Course only updates when moving
Go outside GPS needs clear sky view
Check permissions Location must be allowed
Wait 10-30 seconds GPS needs time to lock

Course is jittery

Fix Explanation
Walk in straight lines Turns confuse GPS averaging
Avoid tree cover Trees block GPS signals
Stay away from buildings Urban canyons cause multipath errors

Speech not working

Fix Explanation
Check volume Obvious but easy to miss
Tap the screen Some browsers require user interaction first
Check media permissions Speech needs audio permission

iOS specific issues

1. Settings → Privacy → Location Services → On
2. Settings → Privacy → Motion & Orientation → Allow for browser
3. Reload the page

🧪 Strategy Tips

Tip Why it works
Walk in open areas GPS accuracy is best with clear sky
Listen for clock positions "12 o'clock" = walk straight ahead
Check your six "Bandit at 6 o'clock" means it's behind you
Change course when bandits approach They chase your last known position
Treasure respawns randomly After collecting, listen for new direction

🔧 Development

Simulate course for testing (no GPS)

```javascript
// Add this for desktop testing
let testCourse = 0;
setInterval(() => {
  testCourse = (testCourse + 10) % 360;
  player.heading = testCourse;
  updateDisplay();
}, 1000);
```

Local HTTPS server

```bash
# Python 3
python -m http.server 8080

# For HTTPS (required for Geolocation on some browsers)
# Use ngrok or localhost with self-signed cert
```

Configuration constants

```javascript
const size = 20;              // World size (units)
const detectionRange = 5;     // Bandit detection range
// Update interval: 15000ms (15 seconds)
// Movement speed: 1 unit per update
```

🎯 Design Philosophy

Why GPS course instead of compass?

Aspect GPS Course Compass
Requires walking ✅ Yes ❌ No
Works without compass hardware ✅ Yes ❌ No
Rewards physical exploration ✅ Yes ❌ No
Mirrors real navigation ✅ Yes ❌ No
Works indoors ❌ Poor ✅ Yes

The game is designed to get you moving. Your body becomes the controller – walk toward treasure, walk away from bandits. No cheating by spinning your phone!

📝 License

MIT License – free to modify and distribute.

🤝 Contributing

Ideas for improvements:

· Score tracking and high scores
· Visual radar/minimap
· Speed-based movement (walk faster = move faster in game)
· Multiple difficulty levels
· Power-ups (speed boost, temporary invisibility)
· Vibration feedback on treasure collection
· Bandit count scaling with score

---

Get outside, start walking, and trust your GPS course! 🚶‍♂️🗺️

Your direction of travel is your only controller. Walk well.

```