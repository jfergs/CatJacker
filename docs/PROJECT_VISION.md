# CatJacker Project Vision

CatJacker is the hardware bridge and appliance path for the CatJack ecosystem.
It should make radio USB, audio, serial, and TNC access practical for phones,
tablets, laptops, and remote clients.

## Product Goal

Provide a portable bridge/appliance that can:

- Connect to USB radios and expose CAT/audio/TNC paths.
- Provide local Wi-Fi, LAN, Bluetooth, or USB client access.
- Support CatJack Mobile field operation.
- Support desktop/browser CatJack operation.
- Optionally host CatJack Server on embedded hardware.
- Enable remote radio operation without moving radio safety policy out of
  CatJack.

## Ecosystem Relationship

CatJacker sits between clients and radios:

```text
CatJack Mobile / CatJack UI
        |
        v
CatJacker transport/appliance
        |
        v
USB radio, audio interface, TNC, KISS device
```

CatJack defines:

- Radio abstraction and capabilities.
- CAT command behavior.
- Memory write safety.
- APRS service boundaries.
- Server APIs.

CatJacker defines:

- Device attachment.
- Transport reliability.
- Local network/pairing.
- Appliance hosting.
- Bridge observability.

## Use Cases

- iPad in the field controlling a radio through a small CatJacker bridge.
- Remote operator connecting to a station radio through a CatJacker appliance.
- Portable APRS/TNC bridge exposing KISS to CatJack Server.
- Embedded CatJack Server on appliance hardware for no-laptop operation.
- Audio transport for future waterfall and digital-mode work.

## Principles

- Transport first, policy second.
- Do not duplicate CatJack radio safety logic.
- Make transport state inspectable.
- Prefer local/offline field operation.
- Support direct client mode and embedded-server mode.
- Keep transmit-capable paths gated by CatJack policy.

## Milestone Themes

1. Device inventory and USB path discovery.
2. CAT serial bridge.
3. CatJack Server discovery/client pairing.
4. KISS/TNC bridge.
5. Audio bridge.
6. Embedded CatJack Server packaging.
7. Remote operation hardening.
8. Field power/network resilience.
