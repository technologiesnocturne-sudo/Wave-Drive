# Roadmap, Manufacturing, and Launch Plan

## Phase 0 — feasibility and product definition (Weeks 1–2)

Deliverables:
- Interview 10–20 target users and document current music-in-car behavior.
- Select the primary promise: local USB playback, phone app + Bluetooth, or active dongle.
- Build a compatibility hypothesis and test matrix.
- Confirm ownership/licensing model for music.
- Identify target launch markets and compliance requirements.
- Obtain preliminary BOM, enclosure, and manufacturing quotes without committing to tooling.

Exit gate: one architecture selected; major platform/licensing unknowns assigned and researched.

## Phase 1 — proof of concept (Weeks 3–6)

Build:
- Mobile app prototype with local audio import, genres/playlists, and reliable playback.
- NFC tag/QR entry flow.
- Use existing phone-to-car Bluetooth where possible.
- If active hardware is essential, prototype on an evaluation board with the exact intended USB/Bluetooth role.

Validate:
- Actual audio path in 5–10 representative vehicles/head units.
- iOS and Android permission/background-playback behavior.
- First-use experience with target users.
- Device power, radio, and host compatibility risks.

Exit gate: repeatable playback and onboarding demonstrated on a documented minimum compatibility set.

## Phase 2 — engineering prototype (Weeks 7–12)

- Build a production-intent industrial design mockup.
- Freeze hardware architecture, interfaces, and protocol.
- Design PCB only after development-board evidence supports it.
- Implement app architecture, migrations, telemetry consent, diagnostics, and automated tests.
- Add signed firmware update path if active firmware exists.
- Create a preliminary BOM, assembly drawing, test points, and factory test plan.

Exit gate: EVT prototype passes core functionality and engineering review.

## Phase 3 — EVT (Engineering Validation Test)

- Build a small batch of engineering samples.
- Validate power, thermal, RF, ESD, USB, mechanical, audio, and firmware recovery.
- Conduct code review and threat-model review.
- Resolve high-risk defects before tooling.
- Work with compliance lab to identify applicable standards and test plan.

Exit gate: no unresolved design-level issue likely to require a major PCB/enclosure change.

## Phase 4 — DVT (Design Validation Test)

- Build samples using production-intent PCB, components, enclosure, and assembly process.
- Run full phone/vehicle matrix, drop/insertion/thermal/EMC pre-compliance tests.
- Verify firmware update, factory reset, serial traceability, and packaging.
- Validate labels, user manual, compatibility claims, warranty, and support workflows.

Exit gate: design is frozen; required formal compliance/certification work is complete or approved for production by the responsible parties.

## Phase 5 — PVT (Production Validation Test)

- Run a pilot batch on the intended manufacturing line.
- Measure yield, cycle time, defect categories, rework, and test coverage.
- Audit supplier traceability and component substitutions.
- Validate packaging/drop shipping and finished-goods quality checks.
- Test a random sample from the batch end-to-end.

Exit gate: manufacturing yield and reliability meet an agreed target; production test and supply chain are stable.

## Phase 6 — launch and ongoing operations

- Begin with limited launch and controlled inventory.
- Publish a live compatibility matrix and known issues.
- Monitor app crashes, returns, pairing/playback failures, and defect rates.
- Maintain security update process and supplier lifecycle tracking.
- Have a product recall/field action process appropriate to product risk.
- Review quality and customer support data monthly.

## Compliance workstream

Engage a qualified test lab and regulatory specialist early. The exact obligations depend on radio hardware, power design, USB implementation, markets, and final claims. Evaluate applicable requirements such as:
- Bluetooth qualification and trademark/brand requirements.
- Radio equipment authorization in each sales region.
- EMC and ESD.
- USB-IF specifications or logo/program requirements if making associated claims.
- RoHS/REACH or other material restrictions where applicable.
- WEEE/battery rules where applicable.
- Consumer electrical/product safety standards relevant to the final product.
- Apple MFi/CarPlay program requirements if licensed Apple technologies are used.
- Google Play/Android Auto eligibility and quality rules for app integration.
- Local import, labeling, warranty, and consumer-protection requirements.

This list is a planning checklist, not a legal determination. Obtain advice for the actual design and countries of sale.

## Manufacturing and supply chain

- Prefer established modules for early prototypes; custom RF design adds risk.
- Select parts based on lifecycle status, multiple-source options, SDK maintenance, certification evidence, and lead times.
- Require approved vendor list (AVL), controlled BOM revisions, and written rules for substitutions.
- Keep firmware signing keys and production credentials under company control, not solely with a contract manufacturer.
- Use per-unit traceability for serial, hardware revision, firmware, test results, and production lot.
- Define incoming inspection, in-process tests, final tests, packaging inspection, and failure analysis.
- Agree supplier quality terms, defect thresholds, warranty responsibilities, and end-of-life notice.

## Commercial planning

Build a unit economics model including:
- Electronics and storage.
- PCB fabrication and assembly.
- Enclosure, finishing, connector wear, and packaging.
- Tooling/NRE, certification, testing, and engineering amortization.
- Freight, import duties, taxes, warehousing, fulfillment, payment fees.
- App development/maintenance, support, warranty reserve, returns, and replacements.
- Channel margin, retailer margin, and marketing.
- Contingency for component substitutions and yield loss.

Do not set a retail price based only on the raw component cost. Establish a target landed cost and gross margin before tooling and purchase commitments.
