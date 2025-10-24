# 🎲 Toy & Game Picker PWA

A playful **Progressive Web App (PWA)** designed to help kids (and parents) decide what to play with!  
You take photos of your toys or games, then spin a colorful **roulette wheel** that picks one at random — followed by a **15-minute timer** for focused playtime.

---

## 🧸 Why This App?

This app was made to help children make decisions in a fun and visual way.  
Instead of endless “What should we play?”, your child can:
1. Take photos of their favorite toys or activities.
2. Spin the wheel.
3. Let chance decide the next 15-minute adventure!

It’s great for:
- Encouraging independent play choices  
- Limiting indecision and arguments  
- Making cleanup rotations more fun  
- Supporting routines for kids with decision fatigue

---

## ✨ Features

| Feature | Description |
|----------|-------------|
| 📸 **Take Photos** | Capture each toy or game with the device camera |
| 🧩 **Save Items** | Images are stored locally in the browser via IndexedDB |
| 🎡 **Roulette Wheel** | Tap “Spin” to randomly select one toy |
| ⏱️ **15-Minute Timer** | Starts automatically when the wheel stops |
| 🧠 **Offline-Ready** | Works without internet and can be installed on any device |

---

## 🧠 Tech Stack

- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES Modules)
- **Storage:** IndexedDB (for offline persistence)
- **APIs:**
  - `navigator.mediaDevices.getUserMedia()` — camera access  
  - `IndexedDB` — image storage  
  - `Service Worker` — offline caching  
  - `<canvas>` — photo capture and processing
- **Installable:** Uses Web App Manifest to work as a home-screen app

---

## 📱 App Flow

```text
[Home Screen]
   ├── [📸 Add Toy] → Opens camera → Capture → Save locally
   ├── Gallery grid → Shows all saved toys
   └── [🎲 Spin Wheel] → Random selection → Show winner + start timer
During the spin:

Photos appear around a circular wheel.

Wheel rotates for a few seconds.

Random toy image is chosen.

During the timer:

Big image of the chosen toy is shown.

Countdown starts from 15:00 → 0:00.

Options: [⏹ Stop] [🔁 Spin Again].

💾 Data Model

Each toy/game is saved like this:
{
  id: 'uuid',
  imageBlob: Blob,      // Photo of the toy
  createdAt: Date.now()
}
Stored in an IndexedDB object store named "toys-store".

⚙️ Setup
1. Clone the repository
git clone https://github.com/your-username/toy-picker-pwa.git
cd toy-picker-pwa

2. Run a local server

You need a local server to access the camera (HTTPS required on the web):
npx serve .
# or
python3 -m http.server 8080

3. Install the PWA

Once loaded in Chrome or Safari, use “Add to Home Screen” to install it like a native app.
#Project Structure
toy-picker-pwa/
│
├── index.html          # Main layout and buttons
├── style.css           # Simple, kid-friendly styles
├── app.js              # Core app logic (camera, storage, spin, timer)
├── manifest.json       # PWA metadata and icons
└── service-worker.js   # Offline caching

# Core logic examples
##Photo Capture
const stream = await navigator.mediaDevices.getUserMedia({ video: true });
video.srcObject = stream;

const canvas = document.createElement('canvas');
canvas.width = video.videoWidth;
canvas.height = video.videoHeight;
canvas.getContext('2d').drawImage(video, 0, 0);
canvas.toBlob(blob => saveToIndexedDB(blob));


Roulette Selection

function spinWheel() {
  const toys = getAllToys();
  const winner = toys[Math.floor(Math.random() * toys.length)];
  animateRoulette(winner);
  startTimer(15 * 60);
}


Timer

function startTimer(seconds) {
  let remaining = seconds;
  const interval = setInterval(() => {
    updateDisplay(remaining);
    if (--remaining <= 0) clearInterval(interval);
  }, 1000);
}


🎨 Design Notes

Bright colors and rounded shapes for a friendly, child-safe feel.

Large buttons for easy tapping.

Offline-first approach ensures it works even on an old tablet with no Wi-Fi.

🚀 Future Ideas

🗑️ Delete or rename toys

🔊 Add sound effects for spin and timer end

💾 Backup toys to cloud or export list

🎨 Themes (e.g. “Space”, “Rainbow”, “Woodland”)

🧘 “Calm Timer” mode for quiet play sessions
