# System Architecture

## 1. Architecture 


## 2. Logical components

- Mobile application: onboarding, local library, metadata index, playlists, playback UI, route state, diagnostics.
- Audio engine: OS-native media playback, audio focus/session handling, headset/car media controls.
- Optional accessory firmware: BLE configuration/control service, device identity, diagnostics, signed update mechanism.
- Optional storage subsystem: filesystem, file integrity checks, supported audio format policy.
- Optional backend: app configuration, opt-in crash reports, update metadata, support diagnostics. Core local playback must not depend on it.
- Manufacturing/test system: serial numbers, firmware versions, test results, production traceability.

## 3. Recommended stack

### Mobile
- React Native + TypeScript for shared screens and application logic.
- Native Swift/Kotlin modules for audio sessions, Bluetooth, NFC, background playback, media notifications, and car-platform integrations as needed.
- SQLite or equivalent local database for track metadata, playlists, play history, and schema migrations.
- OS-supported media frameworks rather than a custom decoder unless format requirements demand it.

### Backend (optional)
- Small REST API or managed service only for real requirements.
- PostgreSQL for account/device records if accounts are introduced.
- Object storage only for user-authorized uploads or update artifacts.
- Avoid storing audio files or user listening histories by default.

### Firmware (if active hardware)
- MCU/SoC selected based on exact USB class, Bluetooth profile, memory, power, and certification requirements.
- Vendor-supported SDK and RTOS where justified.
- BLE for low-bandwidth configuration/control; do not confuse BLE control with Bluetooth audio streaming.
- A2DP Classic or LE Audio path only if supported by the chosen chipset, peer devices, profiles, and product design. Validate phone/car role direction early.

## 4. Data flow — recommended MVP

1. NFC tag/QR code contains a non-secret universal link.
2. Link opens installed app; otherwise it opens a simple setup landing page.
3. App requests permissions in context.
4. User imports audio via OS-approved file picker or supported local library source.
5. App extracts metadata and writes normalized track records to local database.
6. User selects a track; native audio engine decodes and plays it.
7. OS routes audio to the chosen car output.
8. App listens to supported OS route/session events and displays truthful state.
9. Optional diagnostic events are redacted and uploaded only with consent.

## 5. Data model (local-first)

- `Track`: id, sourceUri, title, artist, album, albumArtist, durationMs, genre, artworkUri, format, fileSizeBytes, availability, createdAt, updatedAt.
- `Playlist`: id, name, createdAt, updatedAt, sortOrder.
- `PlaylistTrack`: playlistId, trackId, position, addedAt.
- `PlaybackState`: lastTrackId, positionMs, repeatMode, shuffleEnabled, updatedAt.
- `Device`: id, displayName, hardwareRevision, firmwareVersion, lastSeenAt, pairingState (only if active device exists).
- `DiagnosticEvent`: eventId, eventType, timestamp, appVersion, OSVersion, deviceModelClass, routeClass, redacted properties. Do not include filenames, raw location, contact data, or full listening history by default.

Use stable internal IDs; do not use file paths as primary keys. Handle deleted/moved files and stale artwork gracefully.

## 6. API design (only if backend exists)

Version all endpoints under `/v1`. Require TLS, authentication for private resources, schema validation, rate limits, request IDs, and consistent error bodies.

Possible endpoints:
- `POST /v1/devices/claim` — only if device ownership/activation is necessary.
- `GET /v1/firmware/manifest` — public signed manifest metadata; firmware download authorization rules defined separately.
- `POST /v1/diagnostics` — opt-in, schema-validated, size-limited redacted diagnostics.
- `DELETE /v1/account` — only if accounts exist; deletes or anonymizes associated personal data under the published policy.

Do not create an API just to proxy local playback or basic playlist functions.

## 7. Accessory protocol (only if custom active hardware)

Define and version a documented protocol before app implementation:
- Protocol version and capability negotiation.
- Device identity and firmware version.
- Pairing/ownership flow.
- Commands: `GET_STATUS`, `GET_CAPABILITIES`, `SET_NAME`, `GET_DIAGNOSTICS`, `BEGIN_UPDATE`, `VERIFY_UPDATE`, `REBOOT`.
- Bounded message sizes, explicit error codes, timeouts, retries, and idempotency rules.
- Authenticated control channel where supported; never trust an unpaired nearby device.
- Firmware images signed by a controlled release key.
- BLE control traffic is not a substitute for the audio transport design.

## 8. Reliability patterns

- Keep the core playback path offline-capable.
- Use database transactions for playlist edits and migrations.
- Make device commands idempotent where possible.
- Time out unavailable devices and show recovery steps.
- Avoid infinite Bluetooth reconnect loops; apply backoff and user-visible cancellation.
- Persist only state that is safe and useful to restore.
- Keep a last-known-good firmware image or recovery path if the hardware architecture supports it.
