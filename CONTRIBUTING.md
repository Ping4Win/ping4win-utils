# Contributing to Ping4Win Utils

Thanks for the interest! This project aims to stay small, native and readable.

🌐 **Languages / Языки:** **English** | [Русский](docs/CONTRIBUTING.ru.md) | [Українська](docs/CONTRIBUTING.uk.md)

---

## Getting Started

```bash
git clone https://github.com/Ping4Win/ping4win-utils.git
cd ping4win-utils
./build.sh                         # build + assemble the bundle
./build/Ping4WinUtils --selftest   # quick health check (SELFTEST OK)
./build.sh --install               # install into /Applications and launch
```

Requirements: macOS 14+, Apple Silicon, Xcode Command Line Tools. The build is a plain `swiftc` invocation (see `build.sh`) — no Xcode project, no external dependencies, reproducible by design. `Package.swift` exists so SwiftPM-aware editors can index the code.

### Stable Signing (optional)

By default `build.sh` signs ad-hoc, whose code hash changes every build — so macOS re-prompts for Accessibility/Screen Recording after each rebuild. Run

```bash
./Tools/setup-signing.sh
```

once to create a free, self-signed identity (`Ping4Win Utils Signing`) in a dedicated keychain. `build.sh` then signs local builds with it, giving them a constant designated requirement so granted permissions persist across rebuilds. It is a local convenience only and is never shown outside the keychain.

Official releases are different: CI signs them with an Apple **Developer ID** and notarizes and staples them automatically, so downloads open with no Gatekeeper warning.

---

## Project Layout

| Folder | Role |
|---|---|
| `Sources/Ping4WinUtils/App` | App lifecycle and the menu bar status item |
| `Sources/Ping4WinUtils/Core` | Localisation, permissions, UserDefaults keys |
| `Sources/Ping4WinUtils/Services` | All behavior: energy, monitor, scroll, switcher |
| `Sources/Ping4WinUtils/UI` | SwiftUI views only — no business logic |
| `Sources/Ping4WinUtils/Support` | `--selftest` and `--sensors` diagnostics |
| `Tools` | Icon generator and DMG packaging |

**Conventions:**

- **UI observes services; services never import SwiftUI.** Keep that boundary.
- Singletons are exposed as `Type.shared` and publish state with Combine (`ObservableObject` — no Observation macros; the project builds with the Command Line Tools).
- Comments explain *why*, not *what*. Keep them rare and useful.
- No new dependencies without prior discussion in an issue.

---

## Strings & Translations

Every user-facing string lives in `Core/Localization.swift` as a field of the `Strings` struct. Adding a field forces every language to provide it — the compiler is the completeness check. To add a language: add a case to `AppLanguage` and a `static let` extension of `Strings`.

Currently supported languages: **en-US**, **ru**.

---

## Sensors on New Chips

Temperature mapping lives in `SystemMonitor.prepareSensorsIfNeeded()`:  
CPU = `Tp…`/`Te…`, GPU = `Tg…`, battery = `TB0T…TB2T`.  
If a new Apple Silicon generation renames keys, run:

```bash
./build/Ping4WinUtils --sensors
```

and open a PR with the dump and the adjusted prefixes.

---

## Pull Requests

1. One topic per PR, with a clear description of behavior before/after.
2. `./build.sh` must finish without warnings and `--selftest` must pass.
3. New user-facing text must land in **both** languages (en-US and ru).
4. Match the style of the file you are editing.

---

## Releases (Maintainers)

```bash
git tag v2.12.0 && git push origin v2.12.0
```

The `release` workflow builds the app, packages the DMG and attaches it to a GitHub Release automatically.

---

## Relationship to Upstream

This fork tracks [vorssaint/vorssaint-utils](https://github.com/vorssaint/vorssaint-utils). When upstream releases a new version:

1. Merge `upstream/main` into `main`
2. Resolve any conflicts in `Core/Localization.swift` (the Russian strings block)
3. Update `CHANGELOG.md` with a `[Ping4Win]` section for fork-specific notes
4. Tag and release
