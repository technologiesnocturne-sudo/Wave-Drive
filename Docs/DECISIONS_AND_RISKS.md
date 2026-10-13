# Decisions and Risk Register

## Decisions that must be made

| ID | Decision | Recommended default | Why it matters |
|---|---|---|---|
| D-01 | Primary audio path | Phone app -> car Bluetooth | Lowest-risk way to validate playback and app value. |
| D-02 | What does USB do? | Optional file storage/transfer; not assumed to be audio bridge | Car USB ports have different host roles and supported classes. |
| D-03 | Active electronics in first product? | No, unless user testing proves necessary | Reduces PCB, RF, thermal, firmware, and certification costs. |
| D-04 | NFC purpose | Universal link to setup/app | NFC cannot stream music; a safe link is simple to test. |
| D-05 | Cloud backend | None for MVP | Offline playback does not require cloud services. |
| D-06 | CarPlay/Android Auto | Separate later workstream | Requires platform-specific integration and eligibility review. |
| D-07 | Music source | User-owned local files for MVP | Avoids licensing, DRM, and streaming API complexity. |
| D-08 | Supported formats | Choose based on user research and OS decoder support | Avoid unsupported codec claims. |
| D-09 | Markets | Select launch countries before compliance design freeze | Radio, safety, labeling, import, and privacy requirements vary. |

## Risk register

| Risk | Probability / impact | Mitigation | Owner |
|---|---|---|---|
| USB port does not support intended device | High / High | Define USB role and test across actual target vehicles before PCB freeze. | Hardware lead |
| User expects automatic pairing on insertion | High / High | Clarify setup flow; test prototype; market only demonstrated behavior. | Product lead |
| CarPlay compatibility assumed but unavailable | Medium / High | Treat as separate entitlement/MFi review and platform project. | Platform lead |
| iOS background playback or NFC behavior differs from expectations | Medium / High | Prototype on physical iPhones early; use official APIs. | Mobile lead |
| Android device fragmentation | High / Medium | Maintain OS/device matrix and fallback setup. | QA lead |
| Vehicle compatibility varies by model/year | High / High | Publish tested matrix; avoid universal claims. | QA lead |
| Copyright or streaming-service restrictions | Medium / High | Use user-authorized files and obtain legal review for licensed content. | Legal/product |
| Active hardware overheats in parked vehicle | Medium / High | Component derating, thermal testing, no battery by default. | Hardware lead |
| Firmware compromise or malicious pairing | Medium / High | Secure pairing, signed updates, debug lockout, threat modeling. | Security lead |
| Supply-chain component EOL or substitution | Medium / Medium | AVL, lifecycle checks, second sources, change-control process. | Operations |
| Certification delays launch | Medium / High | Engage lab before design freeze and reserve budget/time. | Operations |
| Poor unit economics after returns/support | Medium / High | Pilot batch, realistic landed-cost model, support/return reserve. | Business lead |

## Go/no-go rules

Do not proceed to custom PCB or tooling until:
- A primary audio path is demonstrated on real target devices.
- The USB role and Bluetooth role are explicitly defined.
- Platform constraints are understood.
- At least a small group of target users completes setup successfully.
- Preliminary unit economics and compliance costs are plausible.

Do not ship until:
- Safety and required compliance reviews are complete.
- P0 requirements and release gates pass.
- Marketing claims match measured compatibility.
- Production traceability, warranty, returns, support, and update ownership are in place.
