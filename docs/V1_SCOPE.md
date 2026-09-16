# V1 Scope Freeze

This document prevents feature creep during implementation.

## Status meanings

- **IN V1** — required for the intended first complete supermarket game.
- **LATER** — compatible with the direction but must not be implemented now or pre-architected speculatively.
- **REJECTED** — contrary to the current direction unless the product is explicitly redesigned.

## IN V1

### Core loop

- One supermarket map.
- One 15-minute Morning Shift scenario.
- Lobby → clock-in → three private quests → shift → results/reveal → same lobby → rematch.
- 10-player design target; 2–10 private, public minimum 6.
- 20-customer shared target.
- Preparation, opening, delivery, Morning Rush, results.

### Map

- Entrance/vestibule.
- Cart staging.
- Small parking/exterior.
- Produce.
- Bakery.
- Aisles 1–8.
- Dairy/drinks.
- Freezers.
- Three checkout lanes.
- Service desk + PA.
- Storage.
- Loading dock.
- Staff room.
- Manager office with glass wall.
- Accessible maintenance roof.
- Lost Property recovery area.

### Player movement and physical interaction

- Walk, sprint, jump, crouch.
- Third-person camera.
- Soft player collision.
- Grab, carry, place, drop, throw.
- Precision placement.
- Push/pull.
- Sit/ride.
- Generic gesture/mischief input; drunk spit variation.
- Stumble/ragdoll/recovery.
- Ragdolled-player dragging.
- Multi-player influence on appropriately large/heavy objects.

### Physics/world

- Real loose rigid bodies.
- Stable stacking.
- Physical doors/freezers.
- Shopping carts with cargo/rider/tipping/spilling.
- Narrow breakage system for glass + office plant.
- Puddles/slipping/cleaning.
- Persistent mess through the shift.
- Out-of-bounds recovery to Lost Property.
- Manual Unstuck.

### Customers and checkout

- Customer approach/opening wait.
- 1–4 category shopping lists.
- Product reservation/depletion.
- Obstacle rerouting/fail-soft shopping.
- Hidden patience, annoyance, complaints, abandonment.
- Queue selection and migration.
- Checkout on/off, conveyor, scanner, transaction.
- Customer slip/stumble/ragdoll.
- Shared served-customer counter.

### Consumable states

- Coffee machine + six reusable cups.
- Caffeine.
- 12 employee alcohol items (8 glass bottles + 4 cans).
- Intoxication.
- Baguettes/fullness.
- Small non-slippery spit mess while drunk.

### Social/information

- Proximity directional voice.
- Wall/door attenuation.
- Proximity gameplay text.
- Global lobby/results communication.
- Physical PA with global broadcast.
- Minimal close-range names/speaker indicator.
- No player-location minimap or global gameplay speaker list.

### Objectives/incidents

- Exactly three private quests/player.
- Authored 60-quest catalogue.
- Effort/resource/conflict-aware assignment.
- Deterministic validation.
- 18 corporate incident definitions.
- Incident scheduler with cooldowns, priority, escalation, hysteresis, no culprit exposure.

### Characters/presentation

- Persistent deterministic random employee appearance.
- One shared gameplay rig/collider.
- Controlled facial/body variation.
- Hair/facial hair/uniform pools.
- Caffeine/intoxication/fullness/dirt visual states.
- Simplified procedurally varied customer appearance.
- Required core interaction animations.
- Material-based physical audio, cart/checkout/PA/incident audio, light supermarket ambience/music.

### UI/onboarding/results

- Match timer/phase.
- Shared served target.
- Collapsible private quest tracker.
- Opening checklist.
- Minimal contextual interaction prompts.
- Incident banners.
- Short first-run orientation/microphone check.
- Team result/store metrics.
- Per-player quest reveal and final-state character.
- 3–5 interesting weird stats/person.
- Same-lobby next-round flow.

### Networking/reliability

- Listen-server initial architecture.
- Server-authoritative match/logical truth/canonical physics.
- Client responsiveness/prediction where required.
- Object sleeping/network optimization.
- Reconnect reservation and state restoration.
- Clean host-loss handling in prototype.
- Architecture not coupled to a local host camera/UI so headless/dedicated operation remains feasible later.

## LATER / EXPERIMENTAL FUTURE

Do not implement or create frameworks solely for these:

- Additional maps/scenarios.
- Temporary death/KO + automatic return/respawn. Exact mechanics undecided.
- Dedicated public-server deployment.
- Host migration.
- Fire-extinguisher spraying.
- Forklifts.
- Drivable vehicles.
- Larger random disaster events.
- More breakable categories.
- More consumable/status systems.
- Cosmetic progression/shop/unlocks.
- Character customization on top of persistent generated identity.
- Emote wheel.
- Full corporate voice acting.
- Voice-reactive mouth movement.
- Spectator/replay systems.
- Advanced custom-lobby modifiers.
- Achievements/seasonal content.
- Deeper customer archetypes/personality.

`LATER` does **not** mean implementation should introduce generic interfaces for these today.

## REJECTED for the current direction

- Designated impostor/traitor/murderer role.
- Teams/factions as the core match structure.
- Forced meeting phases.
- Voting/ejection as gameplay.
- Permanent match-long player elimination.
- Traditional health/combat system in V1.
- Punching/kicking/direct attack button.
- Weapons.
- Conventional inventory/backpack/equipment slots.
- Crafting.
- Skill trees/build stats.
- Hunger/thirst/bladder/survival simulation.
- Realistic fluid simulation.
- Massive structural destruction.
- Player-location minimap/tracking dots.
- Detective/forensic culprit UI.
- Automatic quest sharing/proof button.
- Ranked/MMR-driven primary mode.
- MMO persistence/open world.
- Procedurally infinite supermarket.
- Giant realistic supermarket simulation.

## Scope decision test

For any new proposal ask:

> Does the first supermarket need this feature to produce the specified social loop?

If the answer is no, classify it **LATER** unless it directly replaces a currently required system.
