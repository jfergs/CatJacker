# CatJacker Backlog

Hardware bridge and appliance platform for the CatJack ecosystem. ESP32-S3 and
Raspberry Pi based CAT, PTT, audio, and telemetry transport over Wi-Fi and
Bluetooth.

- Repository: https://github.com/jfergs/CatJacker
- Card convention: `CJ-XXX`

## Milestone Map

| Milestone | Theme |
|---|---|
| Phase 0 | FTX-1 minimal bridge proof of concept |
| Phase 1 | Transport protocol + network audio |
| Phase 2 | CatJacker Pro embedded server |
| Phase 3 | Remote operation hardening |

## Open Cards

### Phase 0 — Minimal FTX-1 Bridge POC

- `CJ-001` Phase 0: Minimal FTX-1 bridge proof of concept.
  Connect FTX-1 via USB to Raspberry Pi Zero 2 W or ESP32-S3. Read/set
  frequency, mode, and S-meter. Control from iPhone via CatJack Mobile. No
  APRS, no audio, no digital modes. Validates the transport architecture.
  _GitHub: #3_

- `CJ-002` ESP32-S3 USB host radio bridge research spike.
  Evaluate USB host capability on ESP32-S3 for direct radio attachment. Defines
  which hardware path (ESP32-S3 vs Pi Zero 2 W) is viable for Phase 0.
  _GitHub: #4_

- `CJ-003` Define CatJacker UX surface for Phase 0.
  Discovery and pairing flow from CatJack Mobile to CatJacker bridge. Transport
  status screen (frequency, mode, S-meter, session health). Error and disconnect
  state handling. Scoped to FTX-1 CAT-only, no audio or APRS.
  _See: docs/UX_SURFACE.md_

- `CJ-004` Initialize firmware project structure.
  PlatformIO scaffold for ESP32-S3 target. Board config, build, and flash
  validation. No radio logic yet.

- `CJ-005` Initialize bridge daemon project structure.
  Python or Go skeleton for Raspberry Pi bridge daemon. Serial port enumeration,
  basic HTTP or WebSocket server stub.

### Phase 1 — Transport Protocol and Network Audio

- `CJ-010` Design CatJacker transport protocol for USB, Wi-Fi, and Bluetooth.
  CAT, telemetry, audio streams, APRS transport, discovery, authentication, and
  multi-client access. Shared protocol for ESP32-S3 and Raspberry Pi builds.
  _GitHub: #1_

- `CJ-011` Implement CAT serial bridge over Wi-Fi.
  Expose radio CAT path to CatJack Server or direct client over local Wi-Fi.
  CatJack still performs identity preflight and write verification.

- `CJ-012` Implement local discovery and pairing.
  mDNS/Bonjour advertisement for CatJacker on local network. Explicit pairing
  handshake for client session control.

- `CJ-013` Implement network audio streaming service.
  Expose USB audio input/output to network clients. Receive-only default. No
  transmit path without CatJack policy gate.
  _GitHub: #2_

- `CJ-014` Implement KISS/TNC bridge.
  Expose KISS TCP path for APRS ingestion by CatJack Server. No APRS transmit
  without safety gates in CatJack.

- `CJ-015` Transport observability.
  Log disconnects, errors, device identity, and session events. Surface state
  to connected clients.

### Phase 2 — CatJacker Pro Embedded Server

- `CJ-020` Integrate optional embedded CatJack Server.
  Package CatJack Server onto appliance hardware (Pi CM4 or similar). CatJack
  Mobile connects to one local field box rather than a laptop.
  _GitHub: #5_

- `CJ-021` Design CatJacker Pro appliance web dashboard.
  Local web UI for bridge status, device attachment, client sessions, and
  network configuration. Field-operable without a laptop.

- `CJ-022` Field power and network resilience.
  Battery/UPS attach, Wi-Fi AP fallback, hotspot mode for no-infrastructure
  environments.

### Phase 3 — Remote Operation Hardening

- `CJ-030` Remote radio operation over WAN.
  Authenticated tunnel for operating a station radio from a remote client.
  Transport state visible; write policy stays in CatJack.

- `CJ-031` Security audit and hardening.
  Verify pairing, session, and transmit gates. Default-deny receive-only
  posture. Logging for disconnects and errors.

## Priority Distribution

| Priority | Cards | Notes |
|---|---|---|
| P0 | CJ-001, CJ-002, CJ-003 | Phase 0 critical path |
| P1 | CJ-004, CJ-005, CJ-010, CJ-011, CJ-012 | Transport foundation |
| P2 | CJ-013–CJ-031 | Audio, APRS, Pro, remote |

## Active Work

- Phase 0 critical path: `CJ-001` / `CJ-002` / `CJ-003`.
- Next action: complete UX surface doc, choose hardware target (ESP32-S3 vs Pi
  Zero 2 W), initialize firmware and bridge scaffolds.
