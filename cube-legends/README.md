# Cube Legends – Space 🚀

A retro space cube runner PWA by **Yusuf Eren Orhan (YUSF)**

---

## Files

```
cube-legends/
├── index.html          ← Main game (all bugs fixed)
├── manifest.json       ← PWA manifest (Play Store ready)
├── sw.js               ← Service Worker (offline support)
├── icons/
│   ├── icon.svg            ← Scalable icon
│   ├── icon-192.png        ← PWA / Android icon
│   ├── icon-512.png        ← Play Store icon
│   └── icon-512-maskable.png  ← Adaptive icon
└── README.md
```

---

## Bugs Fixed

| # | Bug | Fix |
|---|-----|-----|
| 1 | `drawCube()` had an orphan JS syntax error line (`gravity: RPG...`) | Removed |
| 2 | `applyUpgrades()` gravity values were inverted (0.5 low, 0.10 high) | Corrected to 0.18 / 0.32 |
| 3 | `consumeQueuedUpgrades()` called but never defined | Removed call |
| 4 | No save system – progress lost on refresh | localStorage save/load added |
| 5 | `coinMult` not recalculated after page reload | `recalcCoinMult()` runs on boot |
| 6 | Canvas resize ran after `initGame()` – stars used wrong W/H | `resizeCanvas()` called first in `initGame()` |
| 7 | Mobile: page could scroll during gameplay | `touchmove` preventDefault added |
| 8 | No `user-scalable=no` – pinch-zoom broke game | Added to viewport meta |
| 9 | `groundOff` could go negative (visual glitch) | Fixed with modulo + offset |
| 10 | `sw.js` referenced but missing | Created with proper cache strategy |
| 11 | Manifest had SVG-only icon – Play Store requires PNG | PNG icons (192, 512) added |
| 12 | No auto-save – data only saved on death | Auto-save every 30 s during play |
| 13 | `updatePlayOverlay()` race condition on boot | Overlay state driven by `gameState` flag |

---

## Play Store (TWA / Bubblewrap)

To publish to Google Play Store as a **Trusted Web Activity**:

1. **Host the game** on HTTPS (GitHub Pages, Netlify, Vercel, etc.)
2. **Install Bubblewrap CLI**
   ```bash
   npm i -g @bubblewrap/cli
   bubblewrap init --manifest https://your-domain.com/manifest.json
   bubblewrap build
   ```
3. **Sign the APK** with your keystore and upload to Play Console
4. **Add Digital Asset Links** – place this at `/.well-known/assetlinks.json` on your server (Bubblewrap generates it):
   ```json
   [{ "relation":["delegate_permission/common.handle_all_urls"],
      "target":{"namespace":"android_app","package_name":"com.yourname.cubelegends",
                "sha256_cert_fingerprints":["YOUR_FINGERPRINT"]} }]
   ```

> ⚠️ Play Store requires `icon-512.png` (exactly 512×512, no transparency for the store listing screenshot).

---

## Hosting (free options)

| Provider | Command |
|----------|---------|
| **GitHub Pages** | Push folder to repo → Settings → Pages |
| **Netlify** | `npx netlify-cli deploy --dir=.` |
| **Vercel** | `npx vercel` |

---

## Save System

Progress is stored in **localStorage** under the key `cubelegends_v2`.  
A 💾 SAVED indicator flashes in the bottom-right corner after each save.

Saves happen:
- On death
- On level-up
- On upgrade purchase
- On skin change
- Every 30 seconds while playing
