# CatJacker Bridge Daemon

Raspberry Pi bridge daemon for CatJacker. Handles USB radio attachment, CAT
serial bridging, and network transport for CatJack clients.

## Target Hardware

- **Primary:** Raspberry Pi Zero 2 W.
- **Alternative:** Any Pi with USB host and Wi-Fi (Pi 4, Pi CM4 for Pro builds).

## Scope (Phase 0)

- USB serial: open FTX-1 CAT port (`/dev/ttyUSB0` or similar).
- Wi-Fi: expose CAT bridge to CatJack Mobile over local network.
- Read/set frequency, mode, S-meter via CAT commands.
- mDNS advertisement (`_catjacker._tcp`) for client discovery.

## Setup

> Daemon scaffold will be initialized here (CJ-005). Language TBD: Python or Go.

## Related

- `../firmware/` — ESP32-S3 firmware (alternative hardware path)
- `../docs/TRANSPORT_ARCHITECTURE.md` — transport layer design
- `../docs/UX_SURFACE.md` — operator-facing states and flows
