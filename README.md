# CatJacker

CatJacker is the hardware bridge and appliance platform for the CatJack
ecosystem. It exists to connect radios, phones, tablets, laptops, and future
embedded CatJack Server deployments through practical field transports.

CatJacker does not own radio safety policy. CatJack owns the radio abstraction
layer, CAT command rules, APRS service boundaries, and write/transmit safety.
CatJacker provides the transport and appliance surface that makes those services
usable in the field.

## Ecosystem Role

```text
CatJack Mobile / browser / desktop client
          |
          v
      CatJacker
 USB radio bridge, Wi-Fi/BLE/LAN transport,
 optional embedded CatJack Server
          |
          v
  Radio USB CAT / audio / TNC / KISS
```

Relationship definitions:

- **CatJack** is the core radio platform and server.
- **CatJack Mobile** is the native iOS/iPadOS operator client.
- **CatJacker** is the bridge/appliance that exposes radio transports and may
  host CatJack Server on embedded hardware.

## Target Modes

- **USB bridge:** expose radio CAT/audio/TNC paths to CatJack clients.
- **Portable appliance:** field box with Wi-Fi/Bluetooth/local network access.
- **Remote radio host:** network path to a station radio.
- **Embedded CatJack platform:** optional local CatJack Server on appliance
  hardware.

## Documentation

- [Project Vision](docs/PROJECT_VISION.md)
- [Transport Architecture](docs/TRANSPORT_ARCHITECTURE.md)

For ecosystem-level architecture, see the CatJack repository:

- `docs/ECOSYSTEM_OVERVIEW.md`
- `docs/RADIO_ABSTRACTION_LAYER.md`
- `docs/CATJACK_SERVER.md`

## Responsibility Matrix

| Responsibility | CatJacker role | CatJack role | Mobile role |
| --- | --- | --- | --- |
| USB radio access | Bridge/appliance owner | Radio policy/API owner | Operator UX |
| CAT command safety | Transport only | Owner | Presents safe controls |
| Audio transport | Bridge path | DSP/server processing | Mobile UX/future decode |
| KISS/TNC transport | Bridge path | APRS service owner | APRS UX |
| Embedded server | Host option | Server implementation | Client |
| Remote operation | Network appliance | API/service behavior | Client |

## Safety Direction

CatJacker should fail closed:

- Do not invent radio capabilities.
- Report connected transports and device identity.
- Keep write/transmit policy in CatJack.
- Make transport state visible.
- Prefer explicit pairing/session control for field use.
