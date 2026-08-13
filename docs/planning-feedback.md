# Tiny Observer planning feedback

Reviewed against the [Companion Focus App Project Hub](https://www.notion.so/3bbee37f1d1381faa63ee3f8fcf8c6d5) and its product, architecture, art, and launch plans.

## What is already strong

- The product has a memorable wedge: a real focus session becomes an in-world timelapse through the Viewing Relic, then accumulates in a personal Gallery.
- The recording-first positioning is coherent across the loop, Viewing Relic, Timelapse Gallery, room progression, widgets, desktop presence, and shareable recaps.
- The privacy stance is unusually clear for a camera-adjacent product: local-first media, optional Focus Sense, and full no-camera functionality.
- The plans correctly defer social systems, Linux support, and aggressive monetization until individual retention is demonstrated.
- The technical plan calls out difficult areas early: timer recovery, idempotent rewards, real camera capture, local rendering, Gallery persistence, media storage, widgets, and desktop packaging.

## Highest-priority decisions

### 1. Define the smallest proof of value

The current plan still contains enough surface area for several products. Freeze the prototype around one companion, one room, one project chain, one Viewing Relic, one camera mode, and one reward path. The success criterion should be repeat behavior, not feature completeness.

### 2. Keep the recording loop primary and the no-camera path secondary

Camera composition is the product, but it adds permission, storage, battery, rendering, and privacy risk. Validate the camera/timelapse path first, with a no-camera fallback for privacy or permission denial. The fallback should preserve focus, quests, XP, and main currency without making the product drift into a generic focus timer.

### 3. Turn the architecture into a risk order

The proposed stack is reasonable, but the implementation phases should explicitly prioritize the highest-uncertainty proofs: timer correctness under interruption, real iOS camera capture, local frame sampling/rendering, Gallery persistence, companion animation performance, and reward reconciliation after offline completion. Delay backend breadth and multi-platform polish until these work on one named baseline device.

### 4. Make the two reward lanes measurable

The main currency should be earned from active, unpaused focus minutes and remain available to every user, including no-camera users. Focus Sense can later award a separate, capped Focus Marks currency when the user opts into the model. Keep Focus Marks out of the first proof-of-value build until the model passes privacy, accuracy, battery, and thermal checks; it should never replace or reduce the main progression path.

### 5. Replace directional targets with decision thresholds

The GTM plan has useful targets, but each should specify what decision it unlocks. For example: if recording start is weak, test permission messaging and setup length; if capture succeeds but render completion is weak, fix reliability before adding content; if Gallery saves are strong but second-recording return is weak, test the first unlock and quest framing; if camera opt-in is weak, revisit the recording experience rather than redefining the product around no-camera use.

### 6. Establish a single source of truth for content

Viewing Relics, project stages, rewards, room items, and animation states should have stable IDs and versioned definitions. Keep these definitions data-driven so art and balance changes do not require rewriting client logic or invalidating user inventory.

## Recommended first build sequence

1. Clickable prototype of the recording setup, live Viewing Relic scene, session completion, Gallery card, and recap playback.
2. Required account sign-in followed by real camera capture with interruption/relaunch recovery, local persistence, and queued sync.
3. One Rive companion state machine with idle, working, sleepy, and celebrate states.
4. One polished Viewing Relic with live compositing and a deterministic 9:16 local render.
5. Timelapse Gallery playback, metadata, local deletion, favorite/pin, and explicit export/share.
6. No-camera fallback with time-based currency from active minutes only.
7. Small cohort test with capture/render/Gallery instrumentation and explicit privacy messaging.

The first release candidate is not “a timer with a camera.” It is the vertical slice: account → recording → Viewing Relic → recap render → Gallery → replay/export → progression.

## Open questions to resolve before production

- Minimum supported OS is now **iOS 17.0 or later**. The **Phase 0 performance baseline is iPhone 13 (128 GB)**. The intended initial launch floor is **iPhone 12 and newer**; test iPhone 11 later before considering broader support. Do not broaden device support until capture/render budgets pass.
- What exactly counts as a meaningful session for rewards, and how are long, abandoned, and resumed sessions handled?
- How should account recovery, second-device sign-in, and queued offline mutations behave when a user changes devices?
- What is the minimum acceptable battery/storage cost for a camera session and a recap render?
- The initial recording contract is portrait 9:16, target 1080 × 1920, 5–20 second recap, interval-based local sampling, and a 10-minute recording render target of 30 seconds or less on the iPhone 13 baseline. Measure the same contract on iPhone 12 before launch-floor approval.
- For the MVP, keep Gallery records and media device-local. Sync progression and session summaries; design Gallery sync/cloud backup as a later feature rather than showing unavailable Gallery entries on other devices.
- Define the sampling strategy: interval-based frames by default, with temporary source retention only when needed for retry/export or user retention settings.
- Set release budgets for battery, thermal state, storage, render time, and render failure rate for 10-, 25-, and 90-minute recordings.
- The desktop decision is a combined gate: iOS retention, Android validation, explicit desktop demand, and team capacity—not one magic metric.
- What store-policy, privacy, and consent requirements apply to camera capture, face blur, analytics, and the chosen account providers?

## Recommended services

- **Supabase** for Auth, Postgres, Row Level Security, and server-side reward mutations. The relational model is a better fit for append-only reward ledgers, wallet balances, projects, inventory, and cross-device reconciliation than a document-first backend.
- **SQLite on-device** for the offline session database and mutation queue. Supabase should be the online authority, not the offline database itself.
- **PostHog** for privacy-filtered product analytics and feature flags; do not enable session replay or capture camera data, raw video, task text, or Focus Sense source frames.
- **Sentry** for crash and error monitoring, with sensitive payloads filtered before upload.
- **RevenueCat later**, only when paid purchases or subscriptions exist.

## Suggested near-term repository issues

- Prototype session state machine and interruption recovery.
- Build a one-relic real iOS camera compositor spike using AVFoundation, not only sample media.
- Use iOS 17.0 or later as the minimum supported OS; use iPhone 13 (128 GB) as the Phase 0 performance baseline, iPhone 12 and newer as the initial launch floor, and iPhone 11 as a later compatibility test.
- Build a deterministic local 9:16 recap renderer with a 10-minute recording target of 30 seconds or less on the iPhone 13 (128 GB) baseline, then verify the same budget on iPhone 12.
- Define versioned content schemas for projects, stages, rewards, and inventory.
- Instrument the activation funnel without sending freeform task text.
- Test recording-first onboarding and the no-camera fallback as separate cohorts.
- Define the Timelapse Gallery data model, local-media lifecycle, playback states, deletion behavior, and export behavior before implementing cloud backup.
- Write the privacy/data-retention contract before implementing cloud backup.
