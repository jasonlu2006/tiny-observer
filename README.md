# Tiny Observer

Tiny Observer is a cozy-eldritch focus companion: your real focus sessions help a small companion grow, improve its room, and advance projects.

The signature experience places an optional local timelapse inside an in-world Viewing Relic—a window, orb, mirror, portal, or similar magical object—while the companion studies nearby. The product is designed to extend from mobile to widgets, lock-screen presence, and native Windows/macOS desktop companions.

## Current status

This repository is the clean starting point for implementation. Product, UX, architecture, art direction, and launch plans currently live in the [Companion Focus App Project Hub](https://www.notion.so/3bbee37f1d1381faa63ee3f8fcf8c6d5).

The first milestone is a focused mobile prototype that proves:

1. starting and completing a reliable focus session;
2. showing the companion and one room;
3. rendering an optional local timelapse through one Viewing Relic; and
4. awarding visible project progress after completion.

## Direction

- Companion-first, timer-second.
- Privacy by default: camera media stays local unless explicitly exported or backed up.
- No-camera sessions receive the full baseline experience.
- One polished companion and one clear progression loop before expanding platforms or social features.
- Native desktop presence follows evidence of mobile retention.

## Planned workspace

The intended monorepo shape is documented in the Notion architecture plan:

```text
/apps/mobile
/apps/desktop
/packages/domain
/packages/ui
/packages/api-client
/packages/analytics
/packages/assets
/packages/rive
/packages/config
/supabase/migrations
/supabase/functions
/native/ios-widget
/native/android-widget
```

## Planning feedback

See [`docs/planning-feedback.md`](docs/planning-feedback.md) for the current review and the recommended next decisions.
