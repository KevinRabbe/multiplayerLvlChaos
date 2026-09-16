# multiplayerLvlChaos

A multiplayer physics/social-comedy game built around a compact supermarket shift.

Ten-ish employees try to operate one physical supermarket while each receives three private personal responsibilities. Some responsibilities are useful, some absurd, and some quietly conflict with what other employees are trying to do. Proximity communication, persistent physics, incomplete information, and neutral corporate incident announcements make players naturally ask questions such as “who did this?”, “why are you doing that?”, and “I was fixing it.”

There is no impostor role, no forced meeting, no voting phase, and no player elimination in V1. The social layer emerges from real world state and secret motives.

## Current project state

**Design/specification phase. No gameplay code should be written until the current documentation set is treated as authoritative.**

Target technology: **Godot 4.x + C#/.NET**. Exact engine/runtime versions are to be pinned when implementation begins.

## Documentation order of authority

When documents appear to overlap, use this order and report any contradiction instead of choosing an interpretation:

1. `AGENTS.md` — implementation rules for coding agents.
2. `docs/PRODUCT_SPEC.md` — authoritative player-facing behavior.
3. `docs/V1_SCOPE.md` — IN V1 / LATER / REJECTED boundaries.
4. `docs/TUNING.md` — initial numeric defaults.
5. `docs/QUESTS.md` — private quest catalogue and validation rules.
6. `docs/INCIDENTS.md` — corporate incident rules.
7. `docs/LEVEL_SUPERMARKET.md` — map, zones, spawns, object placement.
8. `docs/CONTENT_CATALOG.md` — physical object/archetype catalogue and counts.
9. `docs/PRESENTATION.md` — characters, art, animation, audio, UI and onboarding.
10. `docs/ROUND_LIFECYCLE.md` — chronological match lifecycle.
11. `docs/ARCHITECTURE.md` — technical architecture.
12. `docs/NETWORKING.md` — authority, replication, prediction, reconnect.
13. `docs/ACCEPTANCE_TESTS.md` — definition of a successful implementation.
14. `docs/IMPLEMENTATION_PLAN.md` — milestone order and gates.
15. `docs/TRACEABILITY.md` — system/content dependency audit and result metrics.

## Core design pillars

- **Few systems, many combinations.** Reuse a small set of physical archetypes rather than authoring hundreds of one-off jokes.
- **Meeting-less accusation loop.** Incidents announce problems, never culprits. Players create their own arguments continuously during play.
- **Partial information.** Proximity voice/text, physical sight lines, and world persistence mean no player sees the whole story.
- **Stable first, funny second, realistic third.** Physics must be trustworthy enough that awkwardness feels intentional rather than broken.
- **Persistent physical history.** The supermarket is not cleaned/reset during a shift except for true out-of-bounds recovery.
- **Server owns game truth.** Clients may predict presentation for responsiveness but do not author quest results, customer service, incidents, or canonical physics.
- **No speculative architecture.** Do not build frameworks for hypothetical future maps, vehicles, weapons, death systems, inventory, or progression.

## First playable scenario

**Morning Shift** — 15 minutes, supermarket only.

- 00:00–03:00 Preparation
- 03:00 Store open / normal morning
- 07:00 Delivery event
- 13:00 Morning Rush
- 15:00 Immediate authoritative end-state snapshot and results
- Shared objective: serve at least 20 customers
- Exactly 3 private quests per player
- 10-player design target, 6-player public minimum, 2–10 private

## Repository philosophy

The repository documentation is intended to remove product/design decisions from implementation work. If an implementation agent finds a missing or contradictory requirement, it must report it rather than invent a new game rule.
