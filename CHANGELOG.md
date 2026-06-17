# Changelog

All notable changes to this project are documented here.  
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses [Semantic Versioning](https://semver.org/).

Entries marked **[Ping4Win]** are specific to this fork.  
All other entries originate from the upstream project [vorssaint/vorssaint-utils](https://github.com/vorssaint/vorssaint-utils).

---

## [2.12.0-p4w.1] - 2026-06-17

### Added [Ping4Win]

- **Russian localisation (`ru`)** — full translation of every user-facing string in `Core/Localization.swift`
- `docs/README.ru.md` — complete Russian README
- `docs/CONTRIBUTING.ru.md` — complete Russian contributing guide
- Bilingual language selector in onboarding now includes **Русский** as an option alongside English

### Changed [Ping4Win]

- Project renamed to **Ping4Win Utils** (`com.ping4win.utils`)
- Bundle identifier, app name and all internal references updated accordingly
- README rewritten as bilingual (EN / RU) with Ping4Win branding and upstream attribution

---

## [2.12.0] - 2026-06-15 (upstream)

### Added

- **Support the project.** A new Support tab in Settings, and a brief one-time note when you update, let you back the project with a coffee if you'd like. It stays free, with no subscription, always.

### Fixed

- Battery health matches macOS — the health percentage now lines up with "Maximum Capacity" in System Information.
- The menu bar icon is recoverable — reopening the app from Applications brings the icon back; a "Show menu bar icon" button in Settings rebuilds it.
- Fixed the Support tab hiding the rest of the Settings sidebar.

---

## [2.11.0] - 2026-06-15 (upstream)

### Added

- **Cleaning Mode** — locks the keyboard so you can wipe it down without typing anything by accident. Unlock by pressing the same key five times in a row, by clicking Unlock, or after 60 seconds automatically.

### Fixed

- Battery health now matches macOS.
- Removing the menu bar icon no longer locks you out — it always comes back on launch.
- The icon's right-click menu now opens reliably even when the panel is already open.

---

## [2.10.0] - 2026-06-15 (upstream)

### Added

- **System monitor, expanded**: live network speed (download/upload) with session totals; power draw breakdown; battery health, charge and cycle count; history graphs for CPU, GPU, memory, network, power and battery; system uptime.
- **Metrics in the menu bar**: pin any of CPU, GPU, RAM, Network or Power next to the icon, updated live.
- **Internet speed test** on demand from the Network block.
- **Configurable panel blocks**: choose which blocks appear in the panel and which items appear inside each block.
- **Update notifications**: when a new version is available the menu bar icon turns blue.

### Fixed

- Fixed two mach port leaks in the CPU and memory sampling.

---

## [2.9.1] - 2026-06-14 (upstream)

### Changed

- The switcher's grouping option now shows **one entry per app**, collapsing all windows of an app into a single entry.

---

## [2.9.0] - 2026-06-14 (upstream)

### Added

- **Switcher option to merge an app's tabs** — treats the tabs of one window as a single entry.

---

## [2.8.0] - 2026-06-14 (upstream)

### Added

- **Volume boost in the mixer** — each app's volume now goes up to 200%. Above 100% the slider and percentage turn amber. A one-tap reset returns the app to 100%.

---

## [2.7.0] - 2026-06-14 (upstream)

### Added

- **Advanced settings page** with two clean-up tools: Clear all permissions and Uninstall completely.

### Fixed

- **Quit on last window close** no longer quits an app when you leave full screen with the green button.

---

## [2.6.0] - 2026-06-14 (upstream)

### Changed

- Signed with an Apple Developer ID and notarized — first-launch security warning is gone.

### Migration

- You will grant permissions once on this update due to the new signing certificate.

---

## [2.5.0] - 2026-06-13 (upstream)

### Changed

- The app is now **Vorssaint** everywhere the system shows it (upstream name). In this fork: **Ping4Win Utils**.

---

## [2.4.0] - 2026-06-12 (upstream)

### Added

- **Cut & paste files in Finder** with ⌘X / ⌘V
- **Quit on last window close** with per-app exception list
- **Complete app uninstaller**
- **Temporary shelf** (⌃⌥⌘D or mouse shake)
- Settings moved to a System-Settings-style sidebar

---

## [2.3.0] - 2026-06-12 (upstream)

### Added

- **Per-app volume mixer** in the panel (CoreAudio process taps, macOS 14.4+)

---

## [2.0.0] - 2026-06-12 (upstream)

Initial open-source release.

### Added

- System monitor (CPU/GPU/battery temperatures, usage, memory pressure)
- Inverted mouse scrolling (mouse wheel only)
- Window switcher (⌘Tab with real thumbnails, ScreenCaptureKit)
- 7-step onboarding
- Bilingual interface (pt-BR / en-US in original; ru / en-US in this fork)
- `--sensors` diagnostic flag
- `--uninstall` flag and `Tools/uninstall.sh`
- CI build workflow and automated DMG releases
