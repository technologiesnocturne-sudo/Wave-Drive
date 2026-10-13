# QA and Validation Plan

## 1. Test strategy

Testing must cover the full system: mobile app, audio route, optional NFC, optional USB storage, active accessory firmware if present, and real vehicles. Emulator-only validation is insufficient for a hardware-connected product.

## 2. Mobile software tests

- Unit tests: metadata parsing, playlist ordering, state transitions, file availability, database migrations, route-state logic.
- Integration tests: import -> index -> browse -> play -> seek -> pause -> resume -> playlist edit.
- UI tests: onboarding, denied permissions, empty library, long titles, missing artwork, accessibility, large text.
- Recovery tests: app force quit, phone restart, storage permission revoked, audio interruption, Bluetooth disabled, car ignition cycle.
- Performance tests: large library indexing, memory use, startup time, long playback sessions.
- Security tests: malformed files/metadata, local storage, logs, permissions, network traffic if any.
- Regression suite runs on every pull request; release candidate receives full suite.

## 3. Hardware/firmware tests (if applicable)

- USB enumeration across target hosts and power-only ports.
- Power-on, brownout, repeated ignition cycles, standby consumption, and inrush current.
- NFC read/write/lock and tap reliability across target phone models and cases.
- Bluetooth pairing, reconnection, coexistence, range, and RF performance in final enclosure.
- Audio quality, latency, sample-rate behavior, dropouts, and volume behavior for chosen profile.
- Firmware update interrupted at every stage, invalid signature, wrong hardware revision, rollback, recovery.
- ESD, thermal, mechanical insertion-cycle, drop, vibration, and EMC tests with qualified test providers.

## 4. Vehicle compatibility matrix

For every supported combination record:
- Vehicle make/model/model year and head-unit version.
- Phone model, OS version, app version.
- Connection path: Bluetooth, USB mass storage, wired CarPlay, wireless CarPlay, Android Auto, or other.
- Setup time and steps.
- Cold start and warm reconnect results.
- Playback, controls, track metadata, and steering-wheel controls.
- Sleep/ignition cycle behavior.
- Known limitations and reproduction steps.

Test a representative matrix across older/newer head units, different manufacturers, iOS and Android, and different Bluetooth implementations. Do not infer universal compatibility from one car.

## 5. Acceptance test examples

- Given a supported local audio file, when the user selects Play, then audio begins on the selected output route and the UI shows the actual playback state.
- Given Bluetooth is disconnected, when playback cannot reach the car, then the app presents a recovery action and does not falsely show “car connected.”
- Given an unsupported file, when the user tries to play it, then the app explains the issue without crashing.
- Given the user denies optional NFC/Bluetooth permissions, when the app opens, then unaffected library features remain available.
- Given an accessory firmware update is interrupted, when power returns, then the device either boots a valid image or enters documented recovery.
- Given a USB host does not support the chosen device class, then the device fails safely and the manual explains the limitation.

## 6. Release quality gates

- All P0 tests pass on the release candidate.
- No open critical/high crash, data-loss, security, thermal, electrical, or compatibility defects.
- Two-hour playback soak test passes on each primary supported route.
- At least 100 repeated connection/ignition cycles per representative reference setup, with results recorded; expand sample size based on risk.
- Production-intent hardware has passed pre-compliance tests before formal certification submission.
- Manuals, compatibility page, support process, and known-issues list are ready.
- Signed artifacts, rollback plan, and incident response owner are identified.
