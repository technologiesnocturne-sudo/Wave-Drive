# Smart Music Drive — Project Documentation

**Document status:** Engineering baseline / pre-feasibility  
**Version:** 0.1  
**Date:** 2026-10-12  
**Product class:** Consumer electronics + mobile software  
**Working name:** Smart Music Drive (name subject to trademark review)

## Purpose

This package defines a production-oriented path for a compact music device inspired by the supplied concept image. The intended experience is to make a curated music library easy to access from a phone and play through a vehicle's supported audio system, with optional NFC tap-to-open, playlist/genre browsing, and automatic reconnection.

This is a starting specification, not a claim that the concept is already technically validated or certified. Hardware design, platform permissions, vehicle compatibility, certification, manufacturing costs, and commercial rights must be verified before production.

## The critical product decision

A USB port in a car is not a universal audio input. Depending on the vehicle, it may accept USB mass storage, support wired CarPlay/Android Auto, charge devices, or expose a proprietary interface. A USB flash drive cannot automatically pair with a phone merely because it is inserted. NFC is a short-range trigger/identifier technology, not an audio transport.

Choose and validate one primary product architecture before committing to custom tooling:

1. **Recommended MVP — app + Bluetooth audio:** the phone app plays authorized local audio and sends it to the car's existing Bluetooth audio system. The physical product uses NFC for setup/launch and optionally USB storage as a library transfer mechanism. This is the lowest-risk way to validate the consumer experience.
2. **USB storage edition:** a normal USB mass-storage device containing compatible audio files for vehicles that support USB media playback. The app may help prepare files, but cannot assume it can read arbitrary USB drives on every phone or vehicle.
3. **Active Bluetooth receiver/transmitter dongle:** custom electronics receive or transmit audio over Bluetooth and may present USB storage or audio interfaces. This is a more complex product and requires detailed host-compatibility, power, RF, thermal, and certification work.
4. **CarPlay/Android Auto integration:** treat as a separate platform workstream. CarPlay app support requires the relevant Apple entitlements and eligible app category; certain accessory technologies require MFi. Android Auto media apps must follow Google's car app rules and quality requirements.

Do not market “works with every car,” “automatic CarPlay,” or “universal USB audio” until supported vehicle models and configurations have passed documented tests.

## Package contents

- `PRD.md` — product requirements and acceptance criteria
- `ARCHITECTURE.md` — system architecture, components, data flows, and interfaces
- `HARDWARE_FIRMWARE.md` — hardware/firmware engineering requirements
- `MOBILE_APP.md` — mobile app requirements and implementation plan
- `SECURITY_PRIVACY.md` — threat model, privacy, and security requirements
- `QA_VALIDATION.md` — software, hardware, vehicle, and release validation
- `ROADMAP_MANUFACTURING.md` — staged execution plan, sourcing, compliance, and launch gates
- `DECISIONS_AND_RISKS.md` — unresolved decisions and risk register

## Suggested initial technology choices

- Mobile: React Native + TypeScript for shared UI, with native Swift/Kotlin modules where Bluetooth, NFC, background audio, media controls, and OS integrations require them. Native-first Swift/Kotlin is also valid if budget permits.
- Backend: none required for the first offline MVP. Add a small API only if accounts, cloud sync, device activation, analytics consent, or remote firmware update coordination genuinely need it.
- Hardware prototype: development board with a supported Bluetooth audio profile and a separately validated USB/storage path. Do not custom-design a PCB before proving the chosen architecture.
- Firmware: vendor-supported SDK, reproducible builds, secure boot and signed updates where supported.
- Music: user-owned or properly licensed files/content. Do not scrape or redistribute commercial streaming-service catalogs.

## Source references

- Apple accessory development and MFi overview: https://developer.apple.com/accessories/
- Apple MFi program process: https://mfi.apple.com/en/how-it-works.html
- Apple CarPlay entitlements: https://developer.apple.com/documentation/carplay/requesting-carplay-entitlements
- Android for Cars overview: https://developer.android.com/training/cars/platforms/android-auto
- Android for Cars media apps: https://developers.google.com/cars/design/create-apps/app-types/media
- Bluetooth LE Audio Basic Audio Profile: https://www.bluetooth.com/wp-content/uploads/Files/Specification/HTML/16212-BAP-html5/out/en/index-en.html

Requirements and policies can change. Recheck official documentation at the time of implementation and certification.
