# Tiny Observer planning feedback

Reviewed against the [Companion Focus App Project Hub](https://www.notion.so/3bbee37f1d1381faa63ee3f8fcf8c6d5) and its product, architecture, art, and launch plans.

## What is already strong

- The product has a memorable wedge: a real focus session becomes part of the companion's world through the Viewing Relic.
- The companion-first positioning is coherent across the loop, room progression, widgets, desktop presence, and shareable recaps.
- The privacy stance is unusually clear for a camera-adjacent product: local-first media, optional Focus Sense, and full no-camera functionality.
- The plans correctly defer social systems, Linux support, and aggressive monetization until individual retention is demonstrated.
- The technical plan calls out difficult areas early: timer recovery, idempotent rewards, cross-device conflicts, media storage, widgets, and desktop packaging.

## Highest-priority decisions

### 1. Define the smallest proof of value

The current plan still contains enough surface area for several products. Freeze the prototype around one companion, one room, one project chain, one Viewing Relic, one camera mode, and one reward path. The success criterion should be repeat behavior, not feature completeness.

### 2. Separate the emotional loop from the camera loop

Camera composition is the most distinctive demo, but it adds permission, storage, battery, rendering, and privacy risk. Validate two paths in parallel: no-camera focus and camera/timelapse focus. The product should remain delightful if a user never grants camera access.

### 3. Turn the architecture into a risk order

The proposed stack is reasonable, but the implementation phases should explicitly prioritize the highest-uncertainty proofs: timer correctness under interruption, local media capture/rendering, companion animation performance, and reward reconciliation after offline completion. Delay backend breadth and multi-platform polish until these work on one target device.

### 4. Make the two reward lanes measurable

The main currency should be earned from active, unpaused focus minutes and remain available to every user, including no-camera users. Focus Sense can later award a separate, capped Focus Marks currency when the user opts into the model. Keep Focus Marks out of the first proof-of-value build until the model passes privacy, accuracy, battery, and thermal checks; it should never replace or reduce the main progression path.

### 5. Replace directional targets with decision thresholds

The GTM plan has useful targets, but each should specify what decision it unlocks. For example: if first-session completion is weak, test setup length and camera timing; if completion is strong but second-session return is weak, test the first unlock and project framing; if camera opt-in is weak but no-camera retention is healthy, keep camera optional rather than forcing it.

### 6. Establish a single source of truth for content

Viewing Relics, project stages, rewards, room items, and animation states should have stable IDs and versioned definitions. Keep these definitions data-driven so art and balance changes do not require rewriting client logic or invalidating user inventory.

## Recommended first build sequence

1. Clickable prototype of the home room, focus setup, active session, and completion reward.
2. Required account sign-in followed by a real timer with interruption/relaunch recovery, local persistence, and queued sync.
3. One Rive companion state machine with idle, working, sleepy, and celebrate states.
4. No-camera session completion with time-based currency from active minutes only.
5. Camera preview clipped into one Viewing Relic.
6. Local recap export on one mobile platform.
7. Small cohort test with event instrumentation and explicit privacy messaging.

## Open questions to resolve before production

- Is the first launch iOS-first, Android-first, or simultaneous, and which device baseline is supported?
- What exactly counts as a meaningful session for rewards, and how are long, abandoned, and resumed sessions handled?
- How should account recovery, second-device sign-in, and queued offline mutations behave when a user changes devices?
- What is the minimum acceptable battery/storage cost for a camera session and a recap render?
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
- Build a one-relic local compositor spike.
- Define versioned content schemas for projects, stages, rewards, and inventory.
- Instrument the activation funnel without sending freeform task text.
- Test no-camera and camera onboarding as separate cohorts.
- Write the privacy/data-retention contract before implementing cloud backup.
