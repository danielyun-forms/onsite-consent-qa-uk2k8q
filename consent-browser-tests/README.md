# Consent browser testing: Uk2K8Q / WeAFEE

The report is `../qa/report.html`. It maps every one of the 69 source-sheet cases to an observed result, partial check, blocker, or explicit non-execution, and links 27 screenshots.

## What was set up

- Read the proxy README, browser-kit README, AGENT_HANDOFF, VALIDATION, fixture notes, and existing consent scenario sources.
- Verified the local proxy checkout matches GitHub main: `8e1890cd7b2227afa551eb6a7807ba682e32cd9e`.
- Installed/enabled its Manifest V3 extension in the existing dedicated Chrome at `http://127.0.0.1:9333`. Chrome: 154.0.8037.58. Playwright: 1.49.0. Node: 26.4.0.
- Verified 19 core kit checks: document transformation, verification boundaries, stale evidence handling, and browser response/2–0–1 slot checks through the existing Chrome. The standard headless launcher was unavailable in this sandbox, so the two browser checks attached through CDP. The unrelated JWT session test could not load `typescript`; its widget test passed separately.
- Captured the current form read-only on 2026-09-29. The original account, form, sheet, browser-kit configuration, and captured fixture were not modified.
- Started an isolated eager frontend on port **4101** using the existing Fender checkout `d496af53e3d462f0e8277ef992b4cd46bdc0646f`. Existing servers on 4001/4005/4006/4007 were preserved. No product code was edited.

## Scope and safeguards

These are **controlled renderer tests**, not a completed storefront/designer E2E setup. The real local renderer executes against a fresh snapshot of the actual form. A captured production bootstrap is rewritten only to use the local asset manifest and selected consent hotsetting. The document is a minimal local test page. Fixture changes enable globally disabled forms, select consent models, and exercise malformed copy, alternate layout, dark styling, and duplicate embed instances. These changes exist only inside each isolated browser context.

Every non-GET/HEAD/OPTIONS request is aborted before dispatch. GET tracking/identify endpoints are also blocked, and service workers are disabled. The final 19 completed scenarios recorded **47 blocked POST attempts**: 42 analytics and 5 DataDome. No subscription/profile/publish request was sent. Required-email validation used an empty field. Valid submissions, subscription persistence, backend publishing, and authentication were not tested.

Mobile checks use Chrome touch/viewport emulation at 390×844 and 320×844, not physical devices or Safari. Axe found zero violations and no incomplete checks in the recorded desktop/mobile form-subtree audits. Consent text contrast measured at least 18.61:1 in the authored light theme and 17.76:1 in the controlled dark theme. Screen-reader speech output remains unverified.

The multi-instance exploration mounted two copies of the same version after changing it to EMBED in memory. Consent checkbox, legend, disclosure IDs and group names were unique. Email and rich-text IDs duplicated. This is useful evidence, but it is **not** the sheet's exact authored A/B-version precondition; OA-12 remains partial.

## Remaining setup blockers

1. The test Shopify store requires its storefront password. `ONSITE_STORE_PASSWORD` was not set. An unlock page was opened in the dedicated test Chrome; no password was guessed or stored.
2. Django on 8080 cannot start because MySQL on 3308 is unavailable. Docker did not reach a running state. The local designer at `https://localhost:4000/forms/Uk2K8Q` displays “Something went wrong.” All 25 designer cases remain blocked, not failed.
3. The default 4001 development server emits lazy-compilation placeholders that try to call the compiler at the storefront origin. The isolated 4101 wrapper disables lazy compilation and watchers for this frozen test run.

After Docker/MySQL and app8080 are restored, unlock the storefront and sign in to the local designer on account WeAFEE. Then complete the designer cases and normal browser-kit storefront verification. Do not treat its failed setup evidence as `ready`. Keep mutation blocking enabled before any designer edits because autosave is also an API write.


The executable test setup remains in the original local workspace.
