# Product Requirements Document (PRD)

## 1. Product summary

Smart Music Drive is a hardware-assisted music experience for drivers. It combines a compact USB-shaped accessory or storage device with a mobile application that organizes music into genres and playlists and routes playback through a vehicle's supported audio system.

The product should prioritize reliable playback, simple setup, honest compatibility claims, low distraction, and offline operation.

## 2. Problem statement

Drivers may have music spread across files, apps, USB drives, and vehicle interfaces. The product aims to reduce friction when finding and playing a preferred personal music collection in a car.

## 3. Goals

- Make first-time setup understandable in under five minutes in a supported configuration.
- Enable browsing and playback of a user's authorized library.
- Support genre, playlist, favorites, and recently played organization.
- Provide clear device and audio-route status.
- Recover gracefully after phone restart, Bluetooth disconnection, car ignition cycles, and app suspension, within OS restrictions.
- Work offline for locally stored content.
- Offer an NFC tap flow where supported.
- Establish a tested compatibility matrix rather than broad unsupported promises.

## 4. Non-goals for MVP

- Universal compatibility with all vehicles, USB ports, or head units.
- Bypassing Apple CarPlay, Android Auto, DRM, or app-store restrictions.
- Replacing Spotify, Apple Music, YouTube Music, or other streaming apps.
- Downloading or redistributing copyrighted music without permission.
- Cloud accounts, social sharing, recommendation AI, subscriptions, and remote device management unless validated as necessary.
- A custom CarPlay-capable accessory before the program/entitlement and commercial requirements are understood.

## 5. Target users

- Drivers with a local library of legally acquired music.
- Users who prefer genre-based playlists and a consistent interface.
- Owners of vehicles with Bluetooth audio, USB media playback, or a supported infotainment system.
- Users who want a physical tap point to open the companion app or initiate setup.

## 6. MVP product definition

**MVP architecture recommendation:** mobile app is the primary music player; the phone routes audio to the car's existing Bluetooth audio system. NFC is used to open the app or start setup. The USB-shaped hardware may initially be a passive NFC tag plus optional storage, or a prototype enclosure with development electronics. Confirm whether a custom active dongle is needed only after usability tests.

### Functional requirements

| ID | Requirement | Priority | Acceptance criterion |
|---|---|---:|---|
| FR-01 | Onboarding | P0 | User can complete setup with clear permission explanations and no account requirement. |
| FR-02 | Device identification | P0 | App shows the selected accessory/device identity and setup status. |
| FR-03 | Library import | P0 | User can import supported, user-owned audio files through OS-approved mechanisms. |
| FR-04 | Library indexing | P0 | Tracks display title, artist, album, duration, artwork when available, and file availability. |
| FR-05 | Playback | P0 | Play, pause, seek, next, previous, shuffle, repeat work reliably for supported formats. |
| FR-06 | Playlist management | P0 | User can create, rename, reorder, and delete playlists. |
| FR-07 | Genre browsing | P1 | Genres are derived from metadata or assigned by the user; missing metadata is handled. |
| FR-08 | Audio route status | P0 | App identifies the OS-reported output route where available and does not falsely claim the car is connected. |
| FR-09 | Reconnection | P1 | App restores a valid session after transient disconnects when permitted by OS and accessory design. |
| FR-10 | NFC tap | P1 | Compatible NFC phones open the app or a safe landing/setup page; unsupported devices have a QR/manual fallback. |
| FR-11 | Offline mode | P0 | Local library and playlists remain usable without internet. |
| FR-12 | Error handling | P0 | Unsupported format, missing file, denied permission, device unavailable, and route failure have actionable messages. |
| FR-13 | Safe driving UI | P0 | Large controls, minimal steps, no video, and no distracting configuration flows while moving. |
| FR-14 | Diagnostics export | P2 | User can voluntarily export redacted logs for support. |
| FR-15 | Firmware update | P2 | If active firmware exists, updates are signed, integrity checked, recoverable, and documented. |

## 7. Non-functional requirements

- **Reliability:** target at least 99% successful playback session starts in the validated test matrix; define measurement and sample size before release.
- **Startup:** app cold start target under 3 seconds on reference mid-range devices, excluding OS permission prompts and slow library imports.
- **Playback:** no audible gaps caused by app bugs during a 2-hour soak test on each reference configuration; separately document OS/audio-route interruptions.
- **Data integrity:** playlist and metadata changes are transactional and recoverable after a crash.
- **Privacy:** no account or cloud upload required for core local playback.
- **Accessibility:** screen-reader labels, scalable text, sufficient contrast, and touch targets aligned with platform guidance.
- **Localization:** English MVP; externalize all user-visible strings for future locales.
- **Maintainability:** automated tests for core logic, CI builds, dependency scanning, release signing, and versioned migrations.
- **Compatibility:** publish tested phone OS versions, file formats, audio routes, and vehicle/head-unit combinations.
- **Supportability:** device serial/batch identity, app version, firmware version (if applicable), and redacted diagnostics.

## 8. User journeys

### Journey A — first use
1. User installs the app.
2. App explains that playback uses the phone and a supported car audio route.
3. User grants only required permissions.
4. User taps the NFC tag or scans a QR code, if present.
5. App helps the user select the car's Bluetooth audio output or another supported route.
6. User imports local audio or uses an existing local library.
7. User plays a test track and confirms sound from the car.

### Journey B — daily use
1. User starts the car and connects the phone to its audio system.
2. User opens the app or uses supported system media controls.
3. App restores the last playlist/session when appropriate.
4. User chooses a playlist and listens.
5. On disconnection, app pauses or continues according to explicit user settings and OS policy; status is truthful.

### Journey C — USB media vehicle
1. User checks the compatibility list.
2. User copies supported files to the storage device using the documented folder and file-naming scheme.
3. User inserts it into a vehicle USB port that supports USB media.
4. Vehicle-specific playback and sorting behavior is explained; companion-app control is not promised unless validated.

## 9. Product success metrics

- First-session setup completion rate.
- Time from install to first successful playback.
- Playback start success rate by phone OS/audio route/head-unit combination.
- Weekly active listeners and repeat usage (only if analytics consent and privacy review approve).
- Crash-free sessions and playback error rate.
- Return/defect rate and support tickets per 100 units.
- Compatibility failures by vehicle model and model year.

## 10. Launch criteria

- All P0 acceptance criteria pass.
- No open critical/high security defects.
- Hardware safety, RF, EMC, USB, and applicable regional compliance path reviewed by qualified labs.
- Written platform and licensing review completed for any CarPlay/MFi-dependent feature.
- Compatibility matrix and user manual are published.
- Production test fixture, traceability, returns process, and incident-response process are ready.
- Marketing claims are evidence-backed.
