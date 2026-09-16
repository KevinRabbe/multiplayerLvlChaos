# Technical Architecture 1.0

## Technology

- Engine: **Godot 4.x .NET**.
- Language: **C#**.
- Pin exact Godot/.NET versions at implementation start; do not track “latest”.
- Start with one straightforward Godot C# project unless a concrete testing/build need justifies a split.

## Architectural priorities

1. Deterministic/explicit game rules.
2. Authoritative multiplayer state.
3. Responsive physical interaction.
4. Reusable object archetypes.
5. Centralized tuning/data.
6. Logic testability without loading the entire rendered game.
7. No speculative frameworks for future features.

## Conceptual runtime layers

```text
GAME SESSION
│
├── Match Rules
│   ├── Match clock / phases
│   ├── Opening
│   ├── Delivery
│   ├── Rush
│   └── Results
│
├── Gameplay World
│   ├── Players
│   ├── Customers
│   ├── Products / props
│   ├── Checkouts
│   ├── Doors / freezers
│   └── Semantic zones
│
├── Social / Objective Systems
│   ├── Quest assignment/runtime
│   ├── Incident scheduling
│   ├── Statistics
│   └── PA communication state
│
├── Networking
│   ├── Authority
│   ├── Replication
│   ├── Prediction/reconciliation
│   └── reconnect/session state
│
└── Presentation
    ├── HUD/UI
    ├── Audio/voice bridge
    ├── Animation
    ├── Character visuals
    └── Effects/results
```

These are responsibility boundaries, not mandatory giant classes.

## MatchSession

Exactly one authoritative conceptual `MatchSession` per active shift.

Owns/logically coordinates:

- match ID/seed;
- roster;
- authoritative match time/phase;
- StoreOpened state;
- opening readiness;
- delivery event state;
- customer aggregate state;
- quest assignments/runtime registry;
- incident scheduler/runtime;
- statistics;
- authoritative 15:00 EndSnapshot;
- results transition.

It does **not** contain player movement/rigid-body implementation.

## Explicit phase model

Prefer explicit match phase enum/state model such as:

- PreRound
- Preparation
- Morning
- Busy
- Rush
- Ending
- Results

Do not represent the lifecycle using an uncontrolled pile of unrelated booleans.

Delivery is an event within active play rather than necessarily a separate match phase.

## Scene philosophy

Godot scenes correspond primarily to physical things/coherent behavior:

- Player
- Customer
- ShoppingCart
- Watermelon
- product/prop archetypes
- DeliveryBox
- Checkout
- FreezerUnit
- CoffeeMachine
- PAStation
- OfficePlant
- Mop/Puddle
- SupermarketLevel

Do not create physical scenes for abstract rules such as “WatermelonQuest” or “Revenue”.

## World objects do not know quests/incidents

Physical object scripts expose state/capabilities/events only.

Example Watermelon facts:

- stable instance ID;
- archetype/category;
- transform/velocity;
- zone membership;
- held/support state.

It must not contain logic such as `if player has PLACE-01`.

Quest/incident systems observe/query world facts.

## Archetype vs instance

Static definition/archetype contains data such as:

- scene/model reference;
- object category/archetype;
- handling class;
- gameplay mass;
- sale-product category;
- breakability;
- default collision/interaction capabilities.

Per-match instance contains:

- stable instance/network ID;
- current transform/state;
- current zone membership;
- current holder/interaction state;
- runtime break/consumed/reserved state.

Example: one `Watermelon` archetype + 40 instances.

## Stable IDs

Use stable explicit IDs/enums/definition references for:

- object archetypes;
- product categories;
- semantic zones;
- quest IDs;
- incident IDs;
- handling classes;
- checkout IDs;
- network entities.

Do not make gameplay rules depend on Node names, `Contains("melon")`, or typo-prone ad hoc string tags.

## Static data/resources

Godot Resources or equivalent authored data objects should represent static definitions such as:

- ProductDefinition / PropDefinition;
- QuestDefinition;
- IncidentDefinition;
- CustomerCategoryDefinition;
- CharacterGenerationConfig;
- GameTuning sections.

Exact type names may differ. Principle: values are authored/data-driven rather than scattered literals.

## Central tuning

Mirror `docs/TUNING.md` through one authoritative configuration root with logical subsections, e.g.:

- Player
- Physics
- Match
- Customers
- Consumables
- Voice
- Incidents
- Quests
- World
- Networking

A tunable value has one runtime source of truth.

## Semantic zones

Level contains reusable semantic zone volumes with explicit IDs.

Must support overlapping membership where appropriate (e.g. CheckoutArea + Checkout2).

Maintain robust enter/exit membership and avoid high-frequency boundary spam/jitter. Quest confirmation windows remain separate from base zone membership.

### WorldState / WorldQuery boundary

Provide reusable logical queries required by many systems, e.g.:

- objects by archetype/category;
- objects/players in zone;
- open freezer-door count;
- operational checkout count;
- active puddles;
- queued customer count;
- accessible product inventory;
- entrance/aisle traversability.

The purpose is to prevent every quest/incident from traversing arbitrary scene-tree structures independently.

Do not over-generalize into a universal query language.

## Typed gameplay events

Use explicit typed events/data for action history such as:

- CoffeeConsumed
- AlcoholConsumed
- ProductStocked
- CheckoutActivated
- CustomerServed
- PlayerRagdolled
- CartImpact
- PuddleCreated/Cleaned
- ObjectBroken
- StoreOpened
- DeliveryArrived
- zone transitions where event semantics are needed.

Avoid a global untyped string/dictionary EventBus.

Events carry authoritative fields needed for attribution/tests, e.g. customer ID, final scanner player ID, checkout ID, sale value.

## Quest architecture

Separate:

### QuestDefinition

Static authored data:

- ID/title/text;
- effort;
- evaluator family;
- parameters;
- player-count prerequisites;
- phase prerequisites;
- resource requirements/tags;
- incompatibilities/conflict tags;
- onboarding eligibility.

### QuestAssignment

Per-match relationship: player X received definition Y.

### QuestRuntime

Mutable authoritative state:

- progress/counts;
- sustained/cumulative timers;
- sequence stage;
- completed/failed state.

### Evaluator families

Only implement reusable families actually required:

- EventCounter
- WorldState / WorldStateCount
- SustainedState
- CumulativeState
- Sequence
- EndSnapshot
- MultiActorState
- small specialized evaluator where genuinely necessary.

Do not build a universal mission programming language.

## QuestGenerator

Separate from evaluators.

Input:

- locked roster;
- catalogue;
- onboarding eligibility/player history;
- effort rules;
- resource budgets;
- conflicts/incompatibilities;
- player-count requirements;
- preparation/delivery global limits;
- deterministic seed.

Output: complete validated assignment set.

Before match start validate:

- exactly 3/player;
- max one Heavy/player;
- effort budget valid;
- resource budgets valid;
- minimum-player/phase prerequisites valid;
- conflict targets/forbidden combinations valid;
- prep/delivery assignment limits valid;
- avoid recent repeats where possible.

If invalid, regenerate before round launch.

## Deterministic content seeds

Use a match seed and deterministic derived seeds (or equivalent) for reproducible content selection such as:

- quest assignment;
- customer shopping-list generation;
- customer appearance seeds where useful.

Physics itself does not need cross-machine deterministic lockstep.

## Incident architecture

Separate:

- IncidentDefinition;
- IncidentRuntime;
- IncidentScheduler.

Definition includes:

- trigger detector/parameters;
- eligible phases;
- arming duration;
- priority/family;
- cooldown/re-arm/hysteresis;
- escalation relation;
- public message.

Runtime tracks eligibility/last-trigger/re-arm state. Scheduler chooses one eligible event according to rules.

Reuse the same authoritative world detectors used by quests/opening wherever possible to avoid contradictions.

## Customer architecture

Use an explicit small state machine or equally transparent equivalent:

- Approaching
- WaitingForOpening
- Shopping/SeekingProduct
- WaitingForProduct
- GoingToCheckout
- Queued
- Checkout
- Served
- Leaving
- Abandoning
- TemporaryRagdoll

Separate high-level intent (“seek canned food”) from navigation/path execution.

Navigation failure must feed back into customer logic so the customer can reroute/skip/abandon rather than softlock.

Customer shopping lists/reservations/state are authoritative.

## Checkout architecture

Checkout owns/exposes:

- operational state;
- conveyor state;
- scanner region/state;
- current customer/queue relationship as needed.

Authoritative scan validation determines item validity, customer ownership, duplicate scan prevention, final service event, final-scanner quest credit, and sales value.

## Player architecture

Player responsibilities should be decomposed internally rather than a monolithic script. Conceptual areas:

- Movement
- Interaction
- Carry/Grab
- CharacterVisual
- CharacterState (caffeine/intoxication/fullness)
- Ragdoll
- Voice anchor
- Network representation

Exact Node/component decomposition is an implementation choice.

## Persistent profile vs match state

Persistent profile minimum:

- CharacterSeed;
- onboarding completion;
- recent quest history (small rolling window);
- user settings/preferences as appropriate.

Match state:

- assigned quests/runtime;
- caffeine/intoxication/fullness;
- current zone/physical state;
- round statistics.

Round reset replaces match state, never persistent character identity.

## Character generation

Persistent seed resolves into a baseline appearance definition containing constrained parameters/modules (head/body proportions, feature variants, hair/facial hair, skin tone, posture, uniform).

Runtime character visuals are:

> Baseline(seed) + Caffeine + Intoxication + Fullness + Dirt/Wet + Current animation/reaction.

Never permanently mutate baseline due to temporary state.

## Voice boundary

Voice is a separate communication subsystem consuming game metadata such as:

- speaker/listener positions;
- obstruction relation;
- gameplay vs lobby/results mode;
- PA user state;
- mute/settings.

Gameplay logic does not inspect speech content. No speech recognition is required.

PA state is authoritative and simple: current user (optional) + broadcasting state.

## Audio/presentation

Routine footsteps/impacts/ambience may be generated locally from replicated world state. Important authoritative discrete changes (valid scan, glass break, incident, delivery) emit events/state that presentation reacts to.

## World reset

Prefer reconstructing/reloading the match world from canonical level/spawn definitions between rounds rather than trying to reverse every changed transform/state individually.

The reset must guarantee no previous-round debris/state survives.

## Level scene responsibility

`SupermarketLevel` owns/describes:

- architecture;
- zone instances;
- spawn markers;
- customer/player approach/spawn points;
- recovery points;
- canonical initial object-spawn definitions.

It does not own the 15-minute lifecycle.

Use spawn markers/groups rather than hard-coded position literals in gameplay code.

## Godot Autoloads

Use sparingly for genuinely application-wide services such as app/session navigation, user settings/profile storage where appropriate.

Match-specific quest/incident/customer/world systems should not be persistent mutable global singletons.

## Composition/inheritance

Avoid deep inheritance chains such as `Interactable→Carryable→Throwable→Breakable→Consumable`.

Prefer clear capability composition/data where behavior is genuinely reusable, without turning every boolean property into its own Node.

## Dependency injection

Do not introduce a heavy DI framework. Ordinary constructor dependencies for pure C# services and explicit Godot references/initialization are sufficient unless a real requirement appears.

## Error philosophy

Authored/configuration errors in development should fail loudly:

- duplicate IDs;
- missing zone/spawn/archetype references;
- invalid quest parameters;
- impossible required definitions.

Runtime player-created weird states should fail soft according to product rules.

## Logging/dev reports

Structured development logs should cover:

- phase transitions;
- quest assignment/progress/completion/failure;
- incident eligibility/firing;
- customer failure/recovery;
- OOB recovery;
- network interaction ownership/validation;
- disconnect/reconnect.

Internal round reports may record seed, roster, assignments, completion times, incidents, customer outcomes, and errors. These are developer diagnostics and must not become player-facing forensic information.

## Recommended repository structure

```text
/
├── AGENTS.md
├── README.md
├── docs/
├── game/
│   ├── project.godot
│   ├── MultiplayerLvlChaos.csproj
│   ├── Assets/
│   │   ├── Audio/
│   │   ├── Materials/
│   │   ├── Models/
│   │   └── Textures/
│   ├── Scenes/
│   │   ├── Characters/
│   │   ├── Customers/
│   │   ├── Levels/
│   │   ├── Props/
│   │   ├── Systems/
│   │   └── UI/
│   ├── Scripts/
│   │   ├── Characters/
│   │   ├── Customers/
│   │   ├── Gameplay/
│   │   ├── Interaction/
│   │   ├── Match/
│   │   ├── Networking/
│   │   ├── Objectives/
│   │   ├── Physics/
│   │   ├── Social/
│   │   ├── UI/
│   │   └── World/
│   ├── Data/
│   │   ├── Quests/
│   │   ├── Incidents/
│   │   ├── Products/
│   │   └── Tuning/
│   └── Tests/
│       ├── Logic/
│       ├── Integration/
│       └── TestScenes/
└── tools/
```

Exact names may change for concrete implementation reasons; keep the structure obvious and avoid vague dumping grounds (`Misc`, `ManagerManager`, etc.).

## No speculative future systems

Do not add interfaces/components solely for hypothetical:

- weapons;
- health/death/respawn;
- vehicles/forklifts;
- inventory/crafting;
- map rotation;
- multiple game modes;
- progression/monetization.

Current architecture should remain clean enough to extend later, but extension points are created when real requirements exist.
