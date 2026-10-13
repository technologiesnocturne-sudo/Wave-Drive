# Mobile App Specification

## 1. App structure

- **Home:** current playback, last playlist, connection/output status, quick resume.
- **Browse:** genres, artists, albums, tracks, playlists.
- **Now Playing:** artwork, title/artist, seek, play/pause, next/previous, shuffle/repeat, favorite.
- **Library:** import audio, scan/re-index, unavailable files, storage information.
- **Device & Audio:** setup, NFC instructions, supported routes, pairing guidance, firmware info if relevant.
- **Settings:** appearance, audio behavior, permissions help, privacy, diagnostics consent, app version.
- **Help & Compatibility:** supported file formats, phone OS support, vehicle compatibility matrix, troubleshooting.

## 2. Suggested implementation

- React Native + TypeScript for shared application UI.
- Use native modules for platform-specific audio/background behavior, NFC, Bluetooth, media session, and platform automotive integration.
- Use a typed navigation layer and a single source of truth for playback state.
- Separate domain logic from screens: `library`, `playback`, `playlists`, `device`, `diagnostics`, `settings`.
- Use SQLite with migration scripts for durable local metadata.
- Use a tested audio library that supports required formats and OS background playback. Prototype this early; do not assume one cross-platform package handles every edge case.

## 3. Permission strategy

Request permissions just in time and explain why:
- File/media access through the platform's current user-approved picker/library APIs.
- Bluetooth permissions only for actual device discovery/connection features.
- NFC availability only where supported.
- Notifications only if needed for media controls or status.
- Microphone, contacts, precise location, and camera are not required for core music playback. Do not request them unless a separately justified feature needs them.

Permission denial must not crash the app. Offer settings guidance and preserve any functionality that can safely continue.

## 4. Playback behavior

- Use OS-native audio focus/session APIs.
- Support background playback only under platform rules and with appropriate media controls/notifications.
- Respect calls, navigation prompts, headphones removal, route changes, and other audio interruptions.
- Persist last track and position at safe intervals.
- Distinguish “phone connected to accessory,” “phone connected to car,” and “audio currently routed to car.” These are not equivalent.
- Never claim that music is coming from the car solely because the accessory is nearby.
- Provide explicit handling for unsupported codecs and missing files.
- Do not attempt to control third-party streaming apps through undocumented APIs.

## 5. NFC behavior

Recommended first version: encode a universal link with a stable, non-secret product/setup identifier. The link should:
- Open the app if installed, otherwise a helpful web landing page.
- Contain no credentials, access tokens, user identity, or private library data.
- Be revocable or redirectable if the landing experience changes.
- Have a QR/manual fallback.
- Never silently install an app or imply that NFC automatically starts Bluetooth audio on every phone.

If the tag is writable, lock it after programming if the tag and deployment process support it. Validate NDEF compatibility on target iOS and Android devices.

## 6. UI and accessibility

- Dark interface may match the industrial design, but include adequate contrast.
- Touch targets and typography must remain usable in normal phone use.
- Car-specific interfaces must follow the relevant platform's driver-distraction and app-category rules.
- Do not encourage browsing or setup while driving; show a clear “configure while parked” instruction.
- Support screen readers, dynamic text, reduced motion, and non-color-only status indicators.
- Keep animations short and nonessential.

## 7. App states

Define and test: first launch, no library, importing, indexing, ready, playing, paused, audio route unavailable, Bluetooth disconnected, permission denied, unsupported file, file missing, device not found, NFC unsupported, offline, update available, update failed, and unrecoverable error.

## 8. App store and release

- Bundle IDs, signing keys, app privacy declarations, privacy policy, support URL, and review notes must be managed securely.
- Keep development, staging, and production configurations separate.
- Use CI for lint, typecheck, unit tests, integration tests, and signed release builds.
- Use staged rollout where available and monitor crash rates.
- CarPlay access is not guaranteed: request the appropriate entitlement and follow Apple's eligible-category and review requirements. Android Auto requires a supported car-app category and compliance with Google's current quality rules.
