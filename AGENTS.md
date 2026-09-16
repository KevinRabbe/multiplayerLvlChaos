# AGENTS.md

This repository contains an implementation-ready game specification. Coding agents are expected to **implement the documented game**, not redesign it.

## 1. Source of truth

Read these documents before implementing affected systems:

1. `docs/PRODUCT_SPEC.md`
2. `docs/V1_SCOPE.md`
3. `docs/TUNING.md`
4. `docs/QUESTS.md`
5. `docs/INCIDENTS.md`
6. `docs/LEVEL_SUPERMARKET.md`
7. `docs/ROUND_LIFECYCLE.md`
8. `docs/ARCHITECTURE.md`
9. `docs/NETWORKING.md`
10. `docs/ACCEPTANCE_TESTS.md`
11. `docs/IMPLEMENTATION_PLAN.md`
12. `docs/TRACEABILITY.md`

If documents conflict, **stop and report the contradiction**. Do not choose whichever interpretation seems preferable.

## 2. No product/design decisions

Do not silently:

- add or remove gameplay systems;
- change quest semantics;
- change map topology;
- change player-facing tuning defaults;
- add progression, inventory, combat, death, classes, teams, voting, or new maps;
- substitute global voice for proximity voice;
- remove physical awkwardness because a simpler nonphysical solution is easier;
- reinterpret a requirement because another design would be more conventional.

If an engine limitation materially conflicts with the specification, report it with the smallest viable alternatives.

## 3. Scope discipline

Implement only requirements classified **IN V1** plus development tooling needed to verify them.

Features classified **LATER** are not invitations to pre-build frameworks. Features classified **REJECTED** must not be introduced without an explicit approved design change.

Use the simplest architecture that satisfies current requirements. Do not create abstractions for hypothetical airports, weapons, vehicles, health/death, crafting, MMO persistence, map rotation, or monetization.

## 4. Server authority is structural

Do not compromise these boundaries for speed:

- server/host owns match clock and phases;
- server owns quest assignments/progress/results;
- server owns incidents;
- server owns customer state and checkout validation;
- server owns canonical gameplay physics;
- clients may predict movement/held-object/cart presentation for responsiveness but prediction is never game truth;
- hidden quest data must not be sent to unrelated clients before results;
- the 15:00 end snapshot is server-authoritative.

Prototype visuals may be crude. Core authority/state ownership may not be a temporary hack.

## 5. Gameplay logic must be explicit and deterministic

Quest/incident validation must derive from explicit state/events. Do not infer intent using fuzzy heuristics, natural-language interpretation, or AI.

Avoid gameplay magic strings. Use stable IDs/enums/definitions for zones, archetypes, product categories, quests, incidents, and handling classes.

Invalid authored configuration should fail loudly in development rather than silently inventing defaults.

## 6. Tuning

`docs/TUNING.md` defines the initial implementation defaults. Expose tunable values cleanly, but do not silently substitute different defaults.

If playtesting approves a tuning change, update the documentation in the same change.

## 7. Architecture constraints

- Godot 4.x + C#/.NET; pin exact versions at implementation start.
- Prefer one straightforward Godot C# project initially.
- Prefer composition and explicit services over deep inheritance and global god objects.
- Use Autoloads sparingly for true application-wide state only.
- Match-specific systems belong to the match/session rather than persistent global singletons.
- World objects expose state/capabilities; they must not contain hard-coded knowledge of individual quests/incidents.
- Reuse shared world queries and typed gameplay events where current content proves the abstraction useful.
- Do not build a universal mission scripting language.
- Do not add a heavy dependency-injection framework unless an actual requirement appears.

## 8. Testing expectations

Logical systems should have automated tests where practical, especially:

- match lifecycle;
- quest evaluators;
- quest generation/resource constraints;
- incident scheduling/cooldowns;
- end-of-shift snapshot behavior;
- customer shopping-list generation;
- result calculations;
- character-seed determinism;
- save/load identity.

Physics/network requirements require dedicated test scenes and latency tests described in `docs/ACCEPTANCE_TESTS.md` and `docs/IMPLEMENTATION_PLAN.md`.

A feature is not complete merely because code exists. It must pass its documented acceptance gate.

## 9. Documentation stays current

Feature implementation, tests, and approved documentation changes should land together.

Do not allow code behavior and authoritative docs to drift. If implementation reveals an unresolved product decision, report it instead of burying the choice in code.

## 10. Preserve emergent behavior

This is a physics/social-comedy game. Harmless emergent physics behavior is not automatically a bug.

Fix behavior that causes crashes, desync, duplication, softlocks, unrecoverable geometry traps, infinite velocities, state corruption, or severe loss of player agency. Do not automatically remove silly but stable physical solutions such as unusual stacking, cart routing, or carrying strategies merely because they were not explicitly scripted.
