# CatJacker Transport Architecture

CatJacker is a transport bridge and appliance. Its architecture should keep
radio/device transport separate from CatJack's radio policy and CatJack Mobile's
operator UX.

## Transport Diagram

```text
Clients
  |
  +--> CatJack Mobile
  +--> CatJack browser/desktop UI
  +--> Other approved clients
          |
          v
CatJacker session layer
  |
  +--> Wi-Fi / LAN
  +--> Bluetooth
  +--> USB client link
  +--> Local loopback for embedded server
          |
          v
Device bridge layer
  |
  +--> CAT serial
  +--> USB audio
  +--> KISS TCP/serial
  +--> Future GPIO/PTT control
          |
          v
Radio / audio interface / TNC
```

## Transport Responsibilities

| Layer | Responsibility |
| --- | --- |
| Discovery | Identify attached radios, serial ports, audio devices, TNCs. |
| Session | Pair clients, expose active transports, track connection state. |
| Bridge | Move bytes/audio frames between client/server and device. |
| Observability | Report errors, disconnects, rates, and device identity. |
| Embedded Host | Optionally run CatJack Server locally. |

## Non-Responsibilities

CatJacker should not:

- Decide whether a CAT write is safe.
- Invent a radio capability.
- Send APRS beacons or messages without CatJack policy.
- Own memory-plan persistence.
- Own POTA/SOTA logging workflows.

Those belong to CatJack and CatJack Mobile.

## Bridge Modes

### Transparent CAT Bridge

Expose a serial CAT device to CatJack Server or a trusted direct client. CatJack
still performs identity preflight, command generation, and write verification.

### KISS/TNC Bridge

Expose a KISS TCP or serial TNC path so CatJack can ingest APRS packets and later
control transmit paths only after safety gates are designed.

### Audio Bridge

Expose input/output audio paths for receive monitoring, waterfall, and future
digital modes. Audio transport does not imply transmit permission.

### Embedded CatJack Server

Run CatJack Server on the appliance so CatJack Mobile can connect to one local
field box rather than a laptop.

## Security And Safety

- Require explicit local pairing for clients.
- Prefer receive-only/default-deny behavior.
- Surface device identity and transport state.
- Keep logs for disconnects and transport errors.
- Let CatJack enforce radio write/transmit policy.

## Ecosystem Contracts

CatJacker should align with CatJack docs:

- `docs/ECOSYSTEM_OVERVIEW.md`
- `docs/RADIO_ABSTRACTION_LAYER.md`
- `docs/CATJACK_SERVER.md`
- `docs/aprs-roadmap.md`
- `docs/cat-command-mapping.md`
