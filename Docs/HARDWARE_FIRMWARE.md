# Hardware and Firmware Engineering Specification

## 1. Scope

This document applies only if the product proceeds beyond a passive NFC tag/USB storage MVP into custom active electronics. Do not order a custom PCB or injection mold until a development-board prototype proves the exact audio and USB architecture.

## 2. Product requirements

- Compact, pocketable enclosure with no sharp edges and strain-resistant USB connector.
- Status indicator that is visible in daylight but not distracting at night; provide a way to disable or dim it if appropriate.
- Device remains within component operating temperature limits in a parked vehicle environment; automotive cabin heat can be severe.
- No exposed conductive parts except designed connectors.
- Mechanical retention must tolerate repeated insertion/removal and normal vibration.
- Device must not obstruct adjacent ports, controls, gear selector, or driver sight lines.
- If powered from USB, define operating voltage/current, inrush, sleep current, and brownout behavior.
- No battery in the first revision unless it is required. Avoid unnecessary battery, charging, and thermal complexity.

## 3. Architecture decision required before schematic

Document all of the following:
1. Which side is USB host and which side is USB device?
2. Is the port used for power, mass storage, audio, data/control, or a defined combination?
3. Is the device an audio source, audio sink, or both?
4. Which Bluetooth profile(s) are required and which endpoint owns each profile?
5. Is NFC passive NDEF, an NFC controller, or a secure element-based interaction?
6. How is music stored, indexed, updated, and recovered after unsafe removal?
7. What happens if the car's port is power-only or has strict current limits?
8. Which phone and head-unit models are supported?

## 4. Prototype bill of materials categories

- USB connector or qualified plug assembly.
- MCU/SoC and supported Bluetooth radio if active.
- Flash storage and controller if onboard storage is required.
- NFC tag/controller if NFC behavior is required.
- Power regulation, protection, ESD components, filtering, and test points.
- LED and current-limiting/control circuitry.
- PCB, antenna keep-out, RF matching components, shielding where required.
- Enclosure, cap, labels, serial/lot marking.
- Optional secure element if justified by the threat model and platform design.

Choose exact parts only after requirements, availability, lifecycle status, SDK quality, and certification path are reviewed. A component shortlist is not a production BOM.

## 5. Firmware requirements

- Deterministic boot sequence and watchdog recovery.
- Brownout and unexpected power-loss handling.
- Versioned configuration storage with defaults and migration.
- Bounded memory use and no unbounded queue growth.
- Safe handling of malformed input and invalid state.
- Device identity that is unique but does not expose sensitive user information.
- Debug interfaces disabled or access-controlled in production.
- Release builds must not include default passwords, debug secrets, or unrestricted test commands.
- Firmware update package signature verification before installation.
- Power-loss-safe update strategy and documented recovery mode.
- Rollback prevention or version policy appropriate to the threat model.
- No firmware update initiated while the system is in an unsafe state.
- Diagnostic logs must be bounded and avoid personal data.

## 6. USB and storage

- Select a supported filesystem and maximum capacity policy based on target head units.
- Define supported audio codecs and metadata behavior.
- Handle unclean removal and filesystem corruption.
- Test filename encoding, Unicode, long paths, directory depth, duplicate names, and large libraries.
- Do not assume all head units sort files in the same order.
- Verify USB compliance and ESD robustness with a qualified lab.
- Ensure the device does not impersonate an unsupported class or rely on undocumented host behavior.

## 7. RF, antenna, and mechanical design

- Keep antenna clearances from metal enclosure features and the USB host chassis.
- Test performance in the final enclosure, not only on a bare development board.
- Evaluate interference from vehicle electronics and nearby phones.
- Perform coexistence tests if Bluetooth, Wi-Fi, and USB 3.x are present.
- Use a production-intent enclosure for thermal, drop, vibration, insertion-cycle, and RF validation.
- Document ingress protection only if tested to the relevant standard.

## 8. Manufacturing test plan

Each unit should have:
- Serial/lot identification.
- Firmware version and cryptographic build identifier.
- USB enumeration/storage test if applicable.
- NFC read test if applicable.
- Bluetooth pairing/control/audio test if applicable.
- Current consumption and standby-current test.
- LED/status test.
- Basic audio quality test if an audio path is part of the device.
- Final visual/mechanical inspection.
- Pass/fail record tied to serial number.

Create a factory fixture and golden reference unit. Define rework rules, failure codes, sampling levels, and final audit process before mass production.
