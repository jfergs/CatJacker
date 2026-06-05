# CatJacker Firmware

ESP32-S3 firmware for the CatJacker bridge. Handles USB host radio attachment,
CAT serial bridge, Wi-Fi transport, and Bluetooth connectivity.

## Target Hardware

- **Primary:** ESP32-S3 with USB OTG host capability.
- **Secondary:** Evaluate against Raspberry Pi Zero 2 W (see `../bridge/`).

## Scope (Phase 0)

- USB host: enumerate and open FTX-1 CAT serial device.
- Wi-Fi: expose CAT bridge to CatJack Mobile on local network.
- Read/set frequency, mode, S-meter via CAT commands.
- mDNS advertisement (`_catjacker._tcp`) for client discovery.

## Setup

> PlatformIO project will be initialized here (CJ-004).

```
platformio init --board esp32-s3-devkitc-1
```

## Related

- `../bridge/` — Raspberry Pi bridge daemon (alternative hardware path)
- `../docs/TRANSPORT_ARCHITECTURE.md` — transport layer design
- `../docs/UX_SURFACE.md` — operator-facing states and flows
