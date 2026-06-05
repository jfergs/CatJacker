# CatJacker UX Surface

Operator-facing states, flows, and transitions for CatJacker. This document
drives both the transport protocol design and the CatJack Mobile connection
screen. It is intentionally scoped to **Phase 0** (FTX-1 CAT-only, no audio,
no APRS) and expands with each milestone.

---

## Design Principles

These are transport-layer UX principles, not app aesthetics:

1. **State is always visible.** The operator must never wonder whether the
   bridge is connected, whether the radio is attached, or whether a command
   landed. Every state has a clear, named representation.
2. **Failures are explicit.** Transport errors, radio disconnect, and session
   loss each have a distinct state. The app never silently degrades.
3. **Transmit paths require explicit action.** Nothing with a write or transmit
   consequence is passive or automatic. Operator intent is required.
4. **Field-first.** Interactions are designed for one-handed glanceable use.
   Status is readable at arm's length. Critical actions have high tap targets.

---

## Phase 0 Scope

The following is the complete Phase 0 UX surface:

- FTX-1 connected to CatJacker via USB.
- CatJack Mobile connecting to CatJacker over local Wi-Fi or Bluetooth.
- Operator controls: read frequency, set frequency, read mode, set mode, read
  S-meter.
- No audio. No APRS. No digital modes. No remote (WAN) operation.

---

## Discovery and Pairing Flow

### State Machine

```
Idle
  │
  ▼
Scanning ──── timeout ──── No Bridge Found
  │
  ▼
Bridge Discovered
  │
  ▼
Pairing Request Sent
  │
  ├── rejected / timeout ──── Pairing Failed
  │
  ▼
Paired (session active)
  │
  ├── radio not attached ──── Connected / Radio Missing
  │
  ▼
Connected + Radio Ready
```

### States

| State | What the operator sees | Action available |
|---|---|---|
| Idle | "No bridge" empty state | Tap to scan |
| Scanning | Progress indicator, bridge name if found | Cancel |
| No Bridge Found | "No CatJacker found on this network" | Retry / Manual IP |
| Bridge Discovered | Bridge name, signal type (Wi-Fi / BT), firmware version | Connect |
| Pairing Request Sent | Spinner with bridge name | Cancel |
| Pairing Failed | Error with reason (rejected / timeout / already paired) | Retry / Dismiss |
| Paired — Radio Missing | Bridge name, "Radio not attached" badge | Disconnect |
| Connected + Radio Ready | Bridge name, radio model, USB path | Active controls |

### Pairing Rules

- CatJacker advertises via mDNS (`_catjacker._tcp`) on local Wi-Fi and via BLE
  advertisement on Bluetooth.
- CatJack Mobile initiates pairing. CatJacker may require a local confirmation
  (button press or PIN) on the appliance — this is a Phase 1 hardening detail;
  Phase 0 may use auto-accept on LAN.
- One active session at a time in Phase 0. A second client sees "Bridge in use
  by another session."
- Session token is held for the app lifetime. Reconnect after backgrounding
  uses the existing token without re-pairing.

---

## Transport Status Screen

This is the persistent operator view once paired and radio-ready. It is the
home screen for field operation.

### Layout (top to bottom)

```
┌────────────────────────────────────────┐
│  CatJacker ● Connected                 │  ← Bridge status bar
│  FTX-1  •  USB /dev/ttyUSB0           │     (radio model, transport path)
├────────────────────────────────────────┤
│                                        │
│         146.520.000 MHz                │  ← Frequency (large, glanceable)
│              FM                        │  ← Mode
│                                        │
├────────────────────────────────────────┤
│  S-Meter   ▓▓▓▓▓░░░░░  S7             │  ← S-meter (live poll)
├────────────────────────────────────────┤
│  [ Set Frequency ]  [ Set Mode ]       │  ← Primary CAT actions
└────────────────────────────────────────┘
```

### Status Bar States

| State | Indicator | Color token |
|---|---|---|
| Connected + Radio Ready | ● Connected | Green |
| Connected + Radio Missing | ● Radio Missing | Amber |
| Bridge Unreachable | ● Disconnected | Red |
| Reconnecting | ◌ Reconnecting… | Amber (pulsing) |

### Polling Behavior

- Frequency, mode, and S-meter are polled on a fixed interval (Phase 0: 1 Hz
  suggested). The interval is a bridge-side configuration parameter.
- If a poll response is not received within 3× the interval, the status bar
  transitions to Reconnecting.
- S-meter display is suppressed when the radio is in transmit state (Phase 1).

---

## Set Frequency Flow

1. Operator taps **Set Frequency**.
2. A numeric entry sheet appears with current frequency pre-filled.
3. Operator edits the frequency. No confirmation of intent to transmit — this
   is a VFO CAT write, not a transmit action.
4. Operator taps **Set**.
5. CatJack Mobile sends the CAT write command via CatJacker transport.
6. The transport status screen immediately polls frequency to confirm. If the
   readback matches within tolerance, the display updates. If not, a brief
   inline error is shown: "Frequency not confirmed — radio may not have
   accepted the value."

---

## Set Mode Flow

1. Operator taps **Set Mode**.
2. A picker sheet shows modes supported by the connected radio (read from CAT
   capabilities). Phase 0 modes: FM, AM, USB, LSB, CW.
3. Operator selects a mode. No additional confirmation.
4. CatJack Mobile sends the CAT write. Transport status screen polls mode to
   confirm readback.

---

## Error and Disconnect States

These states must each be named and surfaced — not silenced, not logged only.

### Bridge Unreachable

Triggered when: Wi-Fi drops, bridge loses power, or session times out.

- Status bar transitions to ● Disconnected (red).
- Frequency and mode display freezes with a "Last known" label.
- S-meter is hidden.
- A persistent banner appears: "CatJacker unreachable — trying to reconnect."
- Auto-reconnect runs in background using existing session token.
- If reconnect succeeds, banner dismisses and polling resumes from live state.
- If reconnect fails after N seconds, banner changes to "Reconnect failed" with
  a manual **Retry** button.

### Radio Disconnected from Bridge

Triggered when: USB cable pulled, radio powered off, serial path lost.

- Status bar transitions to ● Radio Missing (amber).
- Frequency, mode, and S-meter display is replaced with "Radio not attached."
- Set Frequency and Set Mode are disabled (greyed, not hidden).
- No alarm or alert — this is a field-normal event (operator powers radio down).
- When radio reattaches, bridge notifies client and display reverts to
  Connected + Radio Ready.

### Another Client Joined

Phase 0 allows one session. If a second client attempts to pair:

- New client sees: "CatJacker is in use — another operator has an active
  session."
- Existing session is not interrupted.

### CAT Command Failure

Triggered when: a set-frequency or set-mode command is rejected by the radio
or times out at the transport layer.

- Inline error below the control: "Command failed — [reason if available]."
- No modal. No block on further commands.
- The operator can retry immediately.

---

## Phase 1 UX Additions (out of scope now, noted for continuity)

- BLE pairing confirmation on bridge hardware.
- Multi-client session with read-only observer role.
- Audio stream status (receive path).
- Transmit-capable state with explicit PTT gate.
- APRS session status.
- Bridge appliance web dashboard.
