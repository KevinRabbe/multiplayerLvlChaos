# Implementation Plan 1.0

Implementation advances through explicit milestones. A milestone is complete only when its acceptance gate passes.

## Milestone order

| Milestone | Purpose |
|---|---|
| M0 | Repository/Godot/.NET baseline and tests |
| M1 | Local controller + interaction feel |
| M2 | Basic multiplayer/session transport |
| M3 | Networked physical-object interaction |
| M4 | Shopping cart + ragdoll proof |
| M5 | Greybox supermarket + semantic zones |
| M6 | Authoritative match lifecycle/reset |
| M7 | Checkout + customers |
| M8 | Delivery/mess/consumables/world persistence |
| M9 | Quest engine + generator |
| M10 | Incident system |
| M11 | Voice/proximity information + PA |
| M12 | Character generation/state readability |
| M13 | Complete V1 content population |
| M14 | Results/lobby continuity/reconnect |
| M15 | Performance/network hardening |
| M16 | Full 8–10-player validation |

No second map milestone exists in V1.

---

# M0 — Repository foundation

Pin explicit Godot 4.x .NET + compatible .NET SDK versions.

Establish:

- Godot C# project;
- documented clone/setup/run/test workflow;
- repository/folder baseline from `ARCHITECTURE.md`;
- config/tuning-loading foundation;
- logging conventions;
- development/test launch paths;
- basic CI/test execution if practical.

### M0 gate

Fresh clone follows docs and successfully:

- opens/launches project;
- runs tests;
- validates config IDs/references;
- demonstrates deterministic character/content seed basics.

Initial automated checks should reject duplicate IDs/unknown authored references rather than silently accepting them.

---

# M1 — Local player/controller

Use an empty development test room.

Implement:

- third-person camera/collision;
- walk/sprint/jump/crouch;
- interaction targeting/highlight/context prompt;
- one can, watermelon, box, chair, door, shelf;
- grab/carry physical constraint;
- drop;
- throw;
- precision placement.

No customers/quests/supermarket art required.

### M1 gate

- movement/camera trustworthy;
- no recurring slope/corner/crouch instability;
- small object pickup works reliably;
- carried objects respect collisions;
- drop vs throw predictable;
- experienced tester can construct stable 6-can stack with reasonable effort;
- watermelon rolls/boxes rest on shelves acceptably.

Do not proceed if stacking/carrying is fundamentally unreliable.

---

# M2 — Multiplayer transport

Two processes/machines in simple gray test room.

Implement:

- listen-server host;
- client join/roster;
- stable player network IDs;
- authoritative server session skeleton;
- remote interpolation;
- local movement prediction/reconciliation;
- basic disconnect handling.

### M2 gate

Test 20/60/100/150 ms RTT profiles.

At ~100 ms:

- local movement remains comfortable;
- remote movement smooth;
- jump/state consistent;
- client cannot trivially author arbitrary final positions;
- join/leave does not corrupt session.

---

# M3 — Networked physical objects

Add can/box/watermelon/heavy test object.

Implement:

- server canonical rigid-body authority;
- stable dynamic entity IDs;
- sleeping-body replication foundation;
- grab request/validation;
- local predicted hold presentation;
- server physical hold constraint;
- release/drop;
- server-calculated throw;
- reconciliation;
- exclusive small-object conflict resolution;
- cooperative multi-grab on designated heavy object.

### M3 gate

At ~100 ms:

- A grabs/carries/throws watermelon; B sees coherent result;
- carrying respects doorway/shelf collision;
- both clients converge on thrown-object result;
- simultaneous grab has one authoritative winner;
- no duplication/permanent divergence;
- two players can cooperate or oppose one another on heavy object without authority corruption.

---

# M4 — Shopping cart + ragdoll

Build dedicated cart test lane/ramps/corners/cargo/seat.

Implement:

- server-authoritative cart physics;
- push control;
- speed-dependent steering;
- one rider;
- real loose cargo;
- tipping/spilling;
- rider exit/ejection;
- cart→player/NPC impact interface;
- simplified authoritative ragdoll root + local visual ragdoll;
- ragdoll dragging/recovery/post-recovery resistance.

### M4 gate

At local and ~100 ms:

- slow push predictable;
- high-speed turning worse but controllable;
- rider stable in normal motion;
- severe tip/impact ejects once;
- cargo survives normal movement/spills under severe motion;
- remote cart impact produces one agreed ragdoll event;
- no routine infinite acceleration/rubber-band catastrophe.

Mini stress: one cart + two players + ~20 loose cargo objects repeatedly crash/spill without permanent desync.

Stay here until cart is trustworthy. It is signature gameplay.

---

# M5 — Supermarket greybox + zones

Build primitive 30×24 m supermarket from `LEVEL_SUPERMARKET.md`.

Add all required rooms/aisles/exterior/roof route, zone volumes, spawn markers, recovery points.

No final art.

Add development zone-debug visualization showing boundaries/IDs/current memberships.

### M5 gate

Measure:

- ManagerOffice→Entrance sprint ~6–8 sec;
- Storage→Produce ~6–8 sec;
- longest indoor route ≤10 sec;
- two carts can pass central cross-aisle;
- one poorly parked cart annoys an ordinary aisle;
- cart reliably reaches roof;
- social sight-line goals hold (office visible, storage/roof private, no total-surveillance central spot).

Fix layout before art.

---

# M6 — Match lifecycle + reset

Implement authoritative:

- PreRound;
- Preparation/opening readiness;
- manual early opening;
- forced 03:00 opening;
- placeholder 07:00 delivery;
- 13:00 Rush;
- exact 15:00 snapshot;
- Ending/Results transition;
- full canonical next-round world reconstruction.

Delivery can initially spawn 16 colored cubes.

### M6 automated gate

- early opening does not move global timestamps;
- preparation deadlines use actual opening event;
- forced opening exactly once;
- delivery exactly once;
- Rush exactly once;
- EndSnapshot captured at 15:00 before cosmetic settle;
- post-zero events cannot alter scoring;
- dirty/moved world fully reconstructs to canonical initial state next round.

---

# M7 — Checkout, then customers

## M7A Checkout

Implement checkout on/off, conveyor, scanner validation, customer order/test fixture, final service event, sale value.

### Gate

Two players around same checkout cannot duplicate scans/service.

Final required item yields exactly one:

- CustomerServed event;
- served-count increment;
- final-scanner owner;
- sale addition.

## M7B Customer base state machine

Implement gradually:

- approach/wait for opening;
- enter;
- shopping list;
- seek category;
- reserve product;
- next item;
- choose queue;
- checkout;
- leave.

First make one boring customer reliable, then many.

### Navigation gate

Test normal obstacle, partial blockage, complete blockage. Customer reroutes/abandons target rather than permanent stuck.

### Reservation gate

One remaining product + two customers → exactly one reservation, other follows unavailable behavior; no duplication.

## M7C Patience/complaints

Add only after navigation works:

- patience bands;
- queue frustration;
- collisions/slips hooks;
- complaint events;
- abandonment.

### Population gate

Run actual arrival schedule/hard 12 active ceiling without pathfinding/network overload.

---

# M8 — World consequences / consumables

Implement:

- real delivery box content/event behavior;
- freezer-door physical/logical hysteresis;
- glass breakage;
- authoritative puddles/slipping;
- mop cleaning;
- office plant states;
- coffee machine/cups/caffeine;
- alcohol/intoxication/empty containers;
- baguette/fullness;
- spit mess;
- LostProperty/OOB recovery.

Logic first; polish later.

### Breakage gate

One bottle breaks exactly once server-side → one puddle → same persistent state on all clients → slip/clean works → puddle disappears once.

### LostProperty gate

Launch multiple normal/unique objects OOB:

- recover exactly once after timing;
- safe placement;
- no duplicates/stale holders;
- intact unique props recover;
- destroyed plant does not resurrect.

---

# M9 — Quest engine + generator

## M9A Evaluator families

Begin with six reference quests covering the vocabulary:

- STATE-01 Occupational Hazard — EventCounter;
- PLACE-02 Executive Produce — CurrentState;
- KEEP-05 Freezer Discipline — SustainedState;
- SOCIAL-01 Passenger — CumulativeState;
- STATE-03 Unprofessional Conduct — Sequence;
- PLACE-01 Watermelon Logistics — EndSnapshot.

### Gate

Automated boundary tests, e.g.:

- Freezer Discipline 59.9 sec then break → reset;
- Executive Produce 4.9 sec → no; 5.0+ sec → permanent complete;
- fifteenth roof watermelon after 15:00 → no EndSnapshot credit.

## M9B Privacy

Two-client test verifies other client does not receive hidden quest assignment data before results.

## M9C Quest generator

Implement roster/global assignment constraints and deterministic seed.

Run ≥100,000 generated 10-player rounds in automated/dev simulation. Validate zero invalid sets and report distribution/bias metrics.

## M9D Full catalogue

Only after evaluator/generator stability, author/import all 60 definitions. Every quest must be demonstrated complete in dev/integration testing at least once.

Add dev tooling to force specific quest/inspect progress/timers/conditions.

---

# M10 — Incident system

Start with representative:

- cart shortage;
- freezer warning;
- checkout collapse.

Implement Definition/Runtime/Scheduler, arming, priority, cooldown, escalation, hysteresis, family suppression, public payload privacy.

### Synthetic gate

Feed fake world state directly. Verify exact arming times/priority/re-arm without manually moving props.

Then add full 18 catalogue.

### Playtest gate

Verify messages are actionable enough to understand the problem, ambiguous enough not to reveal cause, and infrequent enough not to become narration spam.

---

# M11 — Voice/proximity/PA

Select/document voice solution based on Godot/.NET compatibility, latency, licensing, and required routing. Do not couple game logic to provider internals.

Implement:

- gameplay proximity/direction;
- obstruction attenuation;
- nearby speaker indicator;
- global lobby/results mode;
- mute;
- proximity text;
- PA global voice/text path.

### Voice test-room gate

Use 2–4 clients with same room, adjacent room, open/closed door, wall, long range, PA.

Tune to `TUNING.md` experiential geography.

Voice failure/missing microphone must not block gameplay; text remains functional and no quest requires speech.

---

# First representative vertical slice

At roughly M11 the project should support the first meaningful social-thesis test **before** final art/all content polish.

Recommended 12-quest slice:

- WORK-01 Shelf Stacker
- WORK-02 Cart Attendant
- WORK-03 Cashier
- PLACE-02 Executive Produce
- PLACE-03 Modern Art
- PLACE-10 Produce Parking
- CHAOS-01 Cart Shortage
- CHAOS-03 Open Door Policy
- STATE-01 Occupational Hazard
- SOCIAL-01 Passenger
- KEEP-05 Freezer Discipline
- KEEP-06 No Produce Executives

Recommended incident slice:

- cart shortage;
- freezer warning;
- checkout failure;
- entrance obstruction;
- checkout crowd;
- plant destruction.

Representative props may still be simplified. The question is whether players naturally begin interpreting/blaming one another without meetings/impostor coaching.

---

# M12 — Character generation/presentation

Implement persistent CharacterSeed and baseline appearance, controlled weirdness, hair/facial hair/uniform, match accent, status deformation.

### Statistical gate

Generate ~10,000 seeds:

- all ranges valid;
- same seed stable;
- ~60/30/10 distribution approximately represented;
- extreme-trait count limits respected.

Also visually inspect generated grids; statistical validity alone is insufficient.

### State readability gate

At ~6–8 m testers can broadly recognize very drunk/high caffeine/extremely stuffed without HUD icons.

---

# M13 — Complete content population

Replace placeholders with full V1 archetype catalogue and specified counts.

Add required core animations/audio/materials/art.

Decorative shelf filler may be noninteractive, but reachable/obvious interactive products should not be visually indistinguishable from fake props in a frustrating way.

Content enters V1 only with gameplay/quest/customer/navigation/readability or deliberate dressing purpose.

---

# M14 — Results/lobby continuity/reconnect

Implement full loop:

15:00 snapshot → team/store results → quest reveal → global conversation → same lobby → ready/roster refill → next roster lock → full reset → new shift.

Finish reconnect flow:

- ~2.5 sec disconnect declaration;
- 120-sec reservation;
- logical progress/status restore;
- safe normal-state re-entry;
- no held-object/seat/ragdoll pose restore.

### Gate

Two consecutive complete rounds without restarting executable/server. No previous-round physical/transient state leaks into next round.

---

# M15 — Performance/network hardening

Profile representative full content:

- physics CPU;
- navigation/customer CPU;
- C# allocation/GC;
- rendering;
- network traffic/corrections;
- voice separately;
- active/sleeping body behavior.

Representative target:

- 10 players;
- up to 12 active customers;
- ~250–300 interactable objects, mostly sleeping;
- 100 active-body stress scenario still playable.

Normal render target should aim for 60 FPS class once representative minimum/recommended hardware is formally selected.

Network target: normal non-voice gameplay roughly <500 kbit/s/client initially.

Optimize measured bottlenecks rather than deleting object density prematurely.

---

# M16 — Multiplayer validation

Scale sessions deliberately:

1. 2 players
2. 4
3. 6
4. 8
5. 10

First 10-player session is technical/destructive: deliberately block, scatter, pile, crash carts, stress sync/voice/customers/reset.

Only after technical stability run less-coached social tests according to `ACCEPTANCE_TESTS.md`.

Do not instruct testers to accuse, lie, sabotage socially, or use PA for comedy.

## V1 complete only when

- documented round runs end-to-end;
- 10 players remain stable;
- interaction/cart physics usable;
- customers/checkout fail soft;
- all quest definitions can validate correctly;
- generator produces valid assignment sets;
- incidents behave correctly;
- proximity information/PA work;
- results/reveal/reset/rematch work;
- reconnect works;
- repeated uncontrolled playtests show the intended social pattern and rematch desire.

---

# Priority if schedule slips

Cut/defer from outside inward:

- extra visual product variants;
- decorative props;
- advanced hair/cosmetic variety;
- music polish;
- some quest/incident variety;
- minor animation variants.

Do **not** sacrifice:

- reliable physical interaction;
- cart physics;
- server authority;
- proximity information;
- meaningful private quest conflict;
- checkout/customer pressure;
- persistent world state;
- results reveal.

## Placeholder policy

Placeholders encouraged for:

- models;
- textures;
- animations;
- audio;
- UI art.

Do not use temporary incorrect architecture for:

- authority/state ownership;
- quest semantics;
- round lifecycle;
- stable IDs;
- semantic zones;
- resource budgeting.

Prototype the correct structural behavior with ugly assets.

## Stop conditions for coding agent

Stop/report rather than improvise when encountering:

- contradictory authoritative docs;
- quest/content requiring undefined behavior;
- engine limitation that materially changes player-facing behavior;
- networking requirement impossible under chosen stack;
- missing incompatible/licensing-critical dependency;
- conflicting acceptance criteria.

Small internal implementation decisions that do not alter observable design remain normal engineering freedom.
