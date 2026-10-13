# Security and Privacy Requirements

## 1. Principles

- Local-first: core playback works without an account or network.
- Data minimization: do not collect personal data unless a defined feature requires it.
- Least privilege: request only permissions needed for enabled features.
- Secure defaults: production builds have debug interfaces disabled.
- Transparency: explain what is stored locally and what leaves the device.
- No unsupported security claims: NFC is not authentication by itself, and Bluetooth pairing does not automatically make every custom protocol secure.

## 2. Threat model

Assets:
- User's local library metadata and playlists.
- Accessory identity and configuration.
- Firmware integrity.
- Optional account/device-claim data.
- Diagnostic logs and release-signing keys.

Threats:
- Nearby attacker attempts unauthorized pairing/control.
- Malicious or malformed metadata/file crashes the app.
- Lost device exposes stored content or debug interfaces.
- Fake firmware update or rollback attack.
- Diagnostic endpoint abuse or personal data leakage.
- Compromised developer credentials or build pipeline.
- NFC tag replaced or rewritten to point to a malicious URL.

## 3. Required controls

### App
- Validate imported metadata and file types; treat media metadata as untrusted input.
- Keep secrets out of source control and mobile bundles.
- Use OS-protected storage for any tokens that become necessary.
- Use TLS for all network traffic.
- Do not log filenames, tokens, raw device addresses, or full listening history by default.
- Pinning is optional and must have a rotation/recovery plan if used.
- Use dependency scanning, secret scanning, and signed release artifacts.
- Apply database migrations safely and avoid storing credentials in SQLite.

### Accessory
- Pair only through a deliberate user action or documented secure setup flow.
- Do not use a static shared password across all units.
- Authenticate privileged control commands where feasible.
- Rate-limit pairing attempts and reset operations.
- Verify signed firmware before install.
- Protect debug UART/JTAG/test pads in production.
- Use unique identity material per unit where the design requires it.
- Ensure factory reset clears user/device bindings without deleting immutable safety information.
- Use secure boot and anti-rollback where supported and proportionate to risk.

### Backend, if used
- TLS, authentication, authorization, input validation, rate limits, and audit logs for administrative actions.
- Separate development/staging/production projects and credentials.
- Least-privilege service identities.
- Encrypt sensitive data at rest where applicable.
- Backups must be access-controlled and tested for restoration.
- Define retention and deletion periods before collecting diagnostics.
- Never put firmware signing private keys in application servers or source control.

## 4. Privacy requirements

- Publish a plain-language privacy policy before release.
- Default analytics to off or obtain valid consent where required.
- Offer deletion/export mechanisms if accounts or cloud data are added.
- Provide a data inventory and retention schedule.
- No precise location, contacts, microphone, or advertising ID for core playback.
- If support logs are uploaded, preview or clearly describe fields and redact identifiers.
- Determine applicable privacy obligations for launch markets, including Ghana and any other target jurisdictions, with qualified legal counsel.

## 5. Security testing gates

- Threat-model review before hardware architecture freeze.
- SAST, dependency vulnerability scanning, secret scanning, and SBOM generation in CI.
- Mobile permission and local-data review.
- Fuzz or robustness testing for custom accessory protocol and media metadata parsing where feasible.
- Firmware signature, interrupted-update, rollback, reset, and recovery tests.
- Penetration test before launch if a backend or custom wireless control protocol is present.
- No unresolved critical/high findings at release without a formally documented risk acceptance by accountable leadership.
