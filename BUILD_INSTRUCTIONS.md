# Dust2 FPS – Capacitor APK Build Guide

## What's in this folder

```
csgo-capacitor/
├── www/
│   ├── index.html              ← Modified game (with server connect screen)
│   ├── de_dust2_-_cs_map.glb  ← Map file (bundled in APK)
│   └── map.json
├── capacitor.config.json       ← Capacitor settings
├── package.json                ← Dependencies
└── BUILD_INSTRUCTIONS.md       ← This file
```

---

## Prerequisites (install these once)

| Tool | Download |
|------|----------|
| **Node.js 18+** | https://nodejs.org |
| **Android Studio** | https://developer.android.com/studio |
| **Java JDK 17** | bundled with Android Studio, or https://adoptium.net |

After installing Android Studio:
1. Open Android Studio → SDK Manager
2. Install **Android SDK Platform 34**
3. Install **Android SDK Build-Tools 34**

---

## Build Steps (do these once)

### 1. Install dependencies
Open a terminal in this folder and run:
```bash
npm install
```

### 2. Add Android platform
```bash
npx cap add android
```

### 3. Copy web files into Android project
```bash
npx cap sync android
```

### 4. Build the APK

**Option A – Debug APK (easiest, no signing needed):**
```bash
cd android
./gradlew assembleDebug
```
Your APK will be at:
`android/app/build/outputs/apk/debug/app-debug.apk`

**Option B – Open in Android Studio (to run on emulator or device):**
```bash
npx cap open android
```
Then press the ▶ Run button in Android Studio.

---

## How to use the APK

1. Install `app-debug.apk` on your Android device
   - Enable "Install from unknown sources" in Settings if prompted
2. Start your game server on your PC:
   ```bash
   node server.js
   ```
3. Open the app — enter your PC's **local IP** and port
   - Example: `http://192.168.1.42:3000`
   - Find your PC's IP: run `ipconfig` (Windows) or `ifconfig` (Mac/Linux)
4. Tap **CONNECT** and play!

> **Important:** Your phone and PC must be on the **same Wi-Fi network**.

---

## Tips

- For a release APK (for sharing), follow Google's signing guide:
  https://developer.android.com/build/building-cmdline#sign_cmdline
- If you get CORS errors, make sure your `server.js` allows all origins.
  Add this near the top of `server.js`:
  ```js
  const io = require('socket.io')(server, { cors: { origin: '*' } });
  ```
- The game loads Three.js and the map from CDN/local file.
  Make sure your device has internet access for Three.js to load.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "Cannot reach server" | Check IP address, ensure same Wi-Fi, check firewall |
| Black screen | Wait ~10 seconds for Three.js to load from CDN |
| App crashes on launch | Ensure Android SDK 34 and Build-Tools 34 are installed |
| `gradlew` permission denied | Run `chmod +x android/gradlew` first |
