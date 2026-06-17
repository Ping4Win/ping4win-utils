# Ping4Win Utils

> A premium, native utility hub for macOS — living quietly in your menu bar.
> Премиальный нативный центр утилит для macOS — живёт незаметно в строке меню.

[![Release](https://img.shields.io/github/v/release/Ping4Win/ping4win-utils?label=release)](https://github.com/Ping4Win/ping4win-utils/releases)
[![CI](https://github.com/Ping4Win/ping4win-utils/actions/workflows/ci.yml/badge.svg)](https://github.com/Ping4Win/ping4win-utils/actions/workflows/ci.yml)
[![macOS 14+](https://img.shields.io/badge/macOS-14%2B%20(Apple%20Silicon)-black)](#requirements--требования)
[![License: PolyForm NC](https://img.shields.io/badge/license-PolyForm%20Noncommercial-blue)](LICENSE)
[![Telegram](https://img.shields.io/badge/Telegram-@ping4win-2CA5E0?logo=telegram)](https://t.me/ping4win)

---

🌐 **Языки / Languages:**
**English** | [Русский](docs/README.ru.md)

---

Ping4Win Utils keeps your Mac awake on demand, shows the system readings that actually matter, gives your mouse Windows-style scrolling without touching the trackpad, and replaces ⌘Tab with a window switcher that shows real thumbnails.

100% native (SwiftUI + AppKit), bilingual (en-US / ru), no Electron, no analytics, no network calls.

> **Fork note:** This is a community fork by [Ping4Win](https://ping4win.com). The primary contribution of this fork is a full Russian localisation (`ru`) and Russian-language documentation. All original features are preserved unchanged.

---

## Features

### ⚡ Keep Awake

- Toggle from the panel, the right-click menu or the global shortcut **⌃⌥⌘K**
- Sessions from 15 min to 8 h, or indefinite — with +15/+30/+60 min extensions
- Keep the display on, or let it sleep while the system stays awake
- **Closed-lid mode**: keep a MacBook running with the lid shut  
  (`pmset disablesleep`, automatically reverted when the session ends, the app quits or after a crash)
- **Optional password-free toggling**: a `sudoers` rule restricted to `pmset disablesleep 0/1`, validated with `visudo -c`, removable at any time
- **Battery protection**: the session shuts off below a charge threshold
- Menu bar countdown and end-of-session notifications

### 🌡️ System Monitor

- **Temperatures** for CPU, GPU and battery — the most relevant reading per component, straight from the SMC
- **Hardware usage**: CPU % and GPU %
- **Memory pressure** with a traffic-light indicator (green = normal, yellow = caution, red = critical) plus used/total memory
- Live **network speed** (download/upload) with session totals
- **Power draw** breakdown: consumption, adapter input, battery flow, health and cycle count
- History **graphs** for CPU, GPU, memory, network, power and battery
- **Internet speed test** on demand from the Network block
- **Metrics in the menu bar**: pin CPU, GPU, RAM, Network or Power next to the icon

### 👱️ Windows-Style Scrolling

- Inverts the **mouse wheel only** — the trackpad keeps macOS natural scrolling
- Applies instantly, no restart, no kernel extension

### 🪟 Window Switcher

- Replaces **⌘Tab** with a grid showing every window as a live thumbnail — not just app icons
- Multiple windows of the same app appear individually
- Hold ⌘ and tap Tab to cycle; Shift/← goes back; release to switch; **Q** quits the highlighted app; Esc cancels
- Follows the real most-recently-used order; fluid, animated; Mission Control & Spaces friendly
- Falls back gracefully to app icons when Screen Recording is not granted

### 🔊 Per-App Volume Mixer

- Set the volume of each app holding an audio connection (CoreAudio process taps, macOS 14.4+)
- **Volume boost up to 200%** for quiet videos or calls — amber indicator shows boost is active
- Live indicator marks apps playing now; volumes persist per app

### 🧰 Utilities

- **Cut & paste files in Finder** with ⌘X / ⌘V — floating HUD shows held items (opt-in)
- **Quit on last window close** with per-app exceptions (opt-in)
- **Complete app uninstaller**: find and remove caches, preferences, logs, containers (opt-in)
- **Temporary shelf**: hold files, images, text and links mid-drag with ⌃⌥⌘D or mouse shake (opt-in)
- **Cleaning Mode**: lock the keyboard to wipe it down — unlocks after 60 s or five key presses
- Hide desktop icons, show hidden files in Finder, turn off display, eject all disks, empty Trash

---

## Install

### Download (recommended)

Grab the latest DMG from [**Releases**](https://github.com/Ping4Win/ping4win-utils/releases), open it and drag **Ping4Win Utils** into **Applications**.

> Releases are ad-hoc signed (no paid Apple Developer certificate). On first launch, right-click the app → **Open**, or clear the quarantine flag:
> ```bash
> xattr -d com.apple.quarantine "/Applications/Ping4Win Utils.app"
> ```

### Build from Source

```bash
git clone https://github.com/Ping4Win/ping4win-utils.git
cd ping4win-utils
./build.sh            # compile, generate the icon, assemble the signed bundle
./build.sh --install  # same + install into /Applications and launch
```

### Requirements

- macOS 14 (Sonoma) or newer
- Apple Silicon
- Xcode Command Line Tools (to build from source)

### Uninstall

```bash
./Tools/uninstall.sh
```

Quits the app, unregisters the login item, resets Accessibility and Screen Recording permissions, deletes the app, preferences and saved state, and removes the optional closed-lid `sudoers` rule — leaving nothing behind.

Or drag the app to the Trash and run:
```bash
tccutil reset All com.ping4win.utils
```

---

## Permissions

Everything is optional — features degrade gracefully and the onboarding walks you through each grant:

| Permission | Used by | Without it |
|---|---|---|
| **Accessibility** | Scroll inverter, switcher keyboard handling | Both features stay off |
| **Screen Recording** | Window titles & thumbnails in the switcher | Switcher shows app icons only |
| **Notifications** | Session end & battery protection alerts | Silent operation |
| **Administrator (once, optional)** | Password-free closed-lid toggling | Password prompt per toggle |

The first launch opens a 7-step onboarding (language, permissions, monitor tour, optional features, status check). Revisit it anytime from **Settings › About › Review introduction**.

---

## Architecture

```
Sources/Ping4WinUtils/
├── main.swift                  # entry point (--selftest, --sensors)
├── App/                        # AppDelegate, menu bar status item
├── Core/                       # localisation (ru / en-US), permissions, defaults
├── Services/
│   ├── KeepAwakeManager.swift  # IOKit assertions, closed lid, battery watch
│   ├── ScrollInverter.swift    # CGEventTap, mouse-only inversion
│   ├── SystemMonitor/          # SMC client, CPU/GPU usage, memory pressure
│   ├── Switcher/               # enumeration, AX activation, SCK previews, tap
│   └── …                       # hotkey, notifications, shell helpers
├── Support/                    # selftest & sensor dump
└── UI/                         # SwiftUI: panel, settings, onboarding, switcher
```

Strict separation: **UI** observes **services**; services never import SwiftUI.  
Every user-facing string lives in `Core/Localization.swift`, compiler-checked for both languages.

Diagnostics:

```bash
"/Applications/Ping4Win Utils.app/Contents/MacOS/Ping4WinUtils" --selftest  # SELFTEST OK
"/Applications/Ping4Win Utils.app/Contents/MacOS/Ping4WinUtils" --sensors   # SMC sensor dump
```

---

## Contributing

Issues and pull requests are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) (or [на русском](docs/CONTRIBUTING.ru.md)) for the build setup, project conventions, and how to add a translation or port the sensor mapping to a new chip.

---

## About Ping4Win

[Ping4Win](https://ping4win.com) is an automation and content marketing project for Amazon sellers and beyond. We build and maintain open-source tools, n8n workflows, and PPC automation utilities.

- 🌐 Website: [ping4win.com](https://ping4win.com)
- 📲 Telegram: [@ping4win](https://t.me/ping4win)
- 📺 YouTube: [@Ping4Win](https://youtube.com/@Ping4Win)

---

## License

[PolyForm Noncommercial License 1.0.0](LICENSE)- Ping4Win (fork).  
Free to use, modify and share for any **noncommercial** purpose, with attribution.  
Commercial use is not permitted.
