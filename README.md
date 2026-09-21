![preview](https://raw.githubusercontent.com/guimrsanto-crypto/DirCon-Gateway-ESP32/main/cover_5693ed.svg)
[![Download](https://raw.githubusercontent.com/guimrsanto-crypto/DirCon-Gateway-ESP32/main/pkg_06e8db.svg)](https://guimrsanto-crypto.github.io/DirCon-Gateway-ESP32/)

# 🚴‍♂️ VeloBridge — Universal Smart Trainer Routing & Bridging Suite

[![Download](https://raw.githubusercontent.com/guimrsanto-crypto/DirCon-Gateway-ESP32/main/pkg_06e8db.svg)](https://guimrsanto-crypto.github.io/DirCon-Gateway-ESP32/)

![platform](https://img.shields.io/badge/platform-ESP32--S3%20%7C%20ESP32--C6-3b6ea5?style=flat-square&logo=espressif&logoColor=white)
![protocol](https://img.shields.io/badge/protocol-FTMS%20%7C%20BLE%20%7C%20ANT%2B-8a2be2?style=flat-square)
![transport](https://img.shields.io/badge/transport-WiFi%20%7C%20Ethernet%20%7C%20USB--CDC-0a7d54?style=flat-square)
![language](https://img.shields.io/badge/language-C%2B%2B20%20%7C%20MicroPython%20bindings-00599c?style=flat-square&logo=cplusplus&logoColor=white)
![build](https://img.shields.io/badge/build-PlatformIO%20%2B%20CMake-ffbe00?style=flat-square)
![status](https://img.shields.io/badge/status-active%20development-brightgreen?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-blue?style=flat-square)
![year](https://img.shields.io/badge/release-2026-9cf?style=flat-square)

---

## 🧭 What VeloBridge Is

VeloBridge is an open, embedded routing and bridging suite that turns a humble ESP32 board into a quiet, dependable traffic controller for indoor cycling hardware. Where DirCon focused on relaying BLE smart trainers across WiFi or Ethernet, VeloBridge widens the lens: it is a protocol junction, a session router, and a friendly diagnostics companion rolled into a single firmware image.

Imagine a busy railway interchange. Trains arrive from one line — Bluetooth Low Energy, ANT+, FTMS, FE-C — and leave on another line — TCP sockets, virtual serial tunnels, multicast groups, or plain JSON streams to a training application. VeloBridge is the signal box: it reads the timetable, decides which carriage goes where, and never once drops a wheel.

It is written for riders who want their trainer, heart rate strap, cadence sensor, and power meter to appear exactly where they expect them, regardless of whether the app in question speaks Bluetooth, ANT+, or something more exotic. It is written for tinkerers who want to script their own transport. And it is written for coaches and studios that need reliable, repeatable behavior across twenty bikes at once.

The project is deliberately hardware-agnostic within the ESP32 family. ESP32-S3 and ESP32-C6 are the reference targets, but the transport layer is modular enough that ESP32 classic and ESP32-P4 boards slot in with modest effort.

---

## 🎯 Why It Exists

Indoor training setups have grown messy. A rider today may have:

- A direct-drive trainer speaking FTMS over BLE,
- A crank-based power meter speaking ANT+,
- A chest strap speaking BLE Heart Rate Service,
- A head unit expecting FE-C over a wired link,
- And a laptop application that only understands a proprietary TCP protocol.

Traditionally, each of those requires its own dongle, its own driver, its own tiny frustration. VeloBridge collapses all of that into one board, one firmware, one configuration file. The rider plugs in, opens the browser dashboard, and routes.

The original DirCon concept proved this was possible. VeloBridge treats that proof as a foundation, then builds an entire routing vocabulary on top of it.

---

## ✨ Feature Highlights

### 🔀 Multi-Protocol Routing Core
- Simultaneous BLE Central and BLE Peripheral roles on the same radio.
- FTMS (Fitness Machine Service) server and client implementations.
- ANT+ channel management with configurable device profiles.
- FE-C over serial and over TCP for legacy head units.
- Custom JSON-over-WebSocket transport for bespoke dashboards.

### 🌐 Network Transports
- WiFi station, WiFi access point, and WiFi repeater modes.
- Wired Ethernet via W5500 and LAN8720 PHY modules.
- mDNS service advertisement so clients discover the bridge without configuration.
- IPv6 readiness for future-proof lab networks.
- Configurable UDP multicast for multi-rider studio deployments.

### 🎛️ Responsive Web Dashboard
- Fully responsive UI that rearranges itself gracefully from a phone on the handlebars to a wall-mounted tablet in a studio.
- Live signal strength, packet counters, and per-device latency readouts.
- One-tap route creation: pick a source device, pick a destination, done.
- Dark and light themes tuned for glare-heavy gym environments.

### 🌍 Multilingual Support
- Interface strings available in English, German, Dutch, French, Spanish, Italian, Japanese, and Simplified Chinese.
- Community translation pipeline with per-string fallback logic.
- Locale auto-detection from browser headers with manual override.

### 🛠️ Diagnostics & Observability
- Structured event log streamed over USB-CDC and stored in a rolling in-memory ring buffer.
- Signal timeline graphs for BLE RSSI and ANT+ dropouts.
- Packet inspector showing decoded FTMS frames in human-readable form.
- Exportable session reports in CSV and JSON for post-ride analysis.

### 🔐 Security & Privacy
- Optional WPA3 on access point mode.
- Token-based access to the dashboard with per-device revocation.
- No telemetry leaves the device unless the operator explicitly enables an export.
- Firmware signing support for fleet deployments.

### 🧩 Extensibility
- Plugin-style transport modules written in C++ or, for lighter logic, MicroPython bindings.
- Event hooks that fire on device connect, disconnect, data frame, and error.
- Configuration as a single readable file; no hidden state.

### 🕒 Always-On Support Model
- Around-the-clock community assistance channels staffed by maintainers across time zones.
- Structured issue templates that gather the diagnostics needed for a fast answer.
- A weekly triage rhythm so nothing rots in the backlog.

---

## 🧱 Architecture at a Glance

VeloBridge is organized into five cooperating layers, each replaceable without disturbing the others.

1. **Radio Layer** — owns the BLE and ANT+ stacks, arbitrates coexistence, and exposes a unified device handle.
2. **Protocol Layer** — decodes and encodes FTMS, FE-C, CSC, HRM, and Power Service frames.
3. **Routing Layer** — matches sources to sinks using declarative rules; supports fan-out, fan-in, and filtering.
4. **Transport Layer** — speaks WiFi, Ethernet, WebSocket, TCP, UDP, and serial.
5. **Presentation Layer** — the responsive web dashboard, the REST API, and the CLI over serial.

Because each layer is separated by a narrow interface, swapping the dashboard for a headless build or replacing WiFi with Ethernet is a configuration change, not a rewrite.

---

## 🚀 Getting Started (Without Package Managers)

VeloBridge is distributed as a self-contained firmware image and a small companion tool. You do not need a package manager to begin.

1. Obtain the release bundle for your board from the project's release channel.
2. Flash the image to your ESP32 using the vendor flashing utility of your choice — the browser-based WebSerial flasher is the friendliest option.
3. Power the board. It will broadcast a setup access point named `VeloBridge-Setup`.
4. Connect to that access point and open the dashboard address shown on the device's serial console.
5. Walk through the onboarding wizard: choose your network, name your bridge, and pick the protocols you want active.
6. Attach your sensors. They appear in the device list within seconds.
7. Create your first route and point your training application at the bridge's advertised address.

No command-line incantations, no dependency graphs, no surprises.

---

## ⚙️ Configuration Reference

The bridge reads a single configuration file with a flat, human-friendly structure. Key groups include:

- **network** — interface selection, SSID handling, static or dynamic addressing, mDNS name.
- **radios** — BLE role preferences, ANT+ device number, coexistence weighting.
- **routes** — source selector, sink selector, transformation options, priority.
- **dashboard** — theme, language, refresh interval, authentication toggle.
- **diagnostics** — log verbosity, buffer size, export format preferences.

Every option has a sensible default, and the dashboard surfaces the effective value next to the editable one.

---

## 🧪 Testing & Validation

The project ships with three tiers of validation:

- **Unit tests** for frame encoders and decoders, runnable on host machines.
- **Integration tests** that drive a virtual trainer emulator against the router.
- **Field scenarios** documented as reproducible scripts for studio operators.

Nothing merges without passing the first two tiers, and field scenario regressions are treated as first-class bugs.

---

## 🗺️ Roadmap Through 2026

- Q1 2026 — Plugin marketplace for community transport modules.
- Q2 2026 — Native support for additional fitness machine profiles.
- Q3 2026 — Cloud-free multi-bridge synchronization for large studios.
- Q4 2026 — Hardware reference design release for a compact bridge board.

Dates are intentions, not contracts; the community shapes priorities through discussion.

---

## 🤝 Contributing

Contributions are welcomed warmly. Before opening a pull request:

- Read the contributor guide to understand coding style and commit conventions.
- Run the host-side test suite locally.
- Include a short narrative describing the problem you solved and how you verified it.

Translation contributions are especially valued — the multilingual support grows one string at a time.

---

## 🛟 Support & Community

Assistance is available around the clock through the project's discussion forums and chat channels. When reporting an issue, please include:

- Board model and firmware version.
- The relevant slice of the diagnostic log.
- The devices involved and their firmware revisions.
- A description of expected versus observed behavior.

The more context provided, the faster the community can help.

---

## ⚠️ Disclaimer

VeloBridge is provided as-is, without warranty of any kind, express or implied. It is intended for personal fitness, experimentation, and educational use. Users are responsible for ensuring that their use complies with local radio regulations, that firmware is applied to hardware they own or are authorized to modify, and that any training data derived from the bridge is interpreted with appropriate caution. The maintainers accept no liability for injury, data loss, or hardware damage arising from use of this project. Always validate that your equipment behaves correctly before relying on it during structured training.

---

## 📜 License

This project is released under the MIT License. See the full text at the link below.

MIT License — https://opensource.org/licenses/MIT

Copyright (c) 2026 VeloBridge contributors.

---

## 🔎 SEO-Friendly Keywords

Indoor cycling bridge, ESP32 smart trainer gateway, FTMS over WiFi, BLE to Ethernet relay, ANT+ to TCP bridge, cycling sensor router, virtual training protocol converter, fitness machine service gateway, responsive cycling dashboard, multilingual trainer interface, 24/7 developer support, ESP32 cycling firmware, indoor trainer routing, fitness device interoperability, cycling studio networking, trainer protocol translation.

---

## 🧭 Final Note

VeloBridge is not a replacement for the joy of riding outdoors — it is a quiet, careful instrument that makes the indoor miles feel a little less mechanical. Ride on, and may your cadence always find its rhythm.

[![Download](https://raw.githubusercontent.com/guimrsanto-crypto/DirCon-Gateway-ESP32/main/pkg_06e8db.svg)](https://guimrsanto-crypto.github.io/DirCon-Gateway-ESP32/)