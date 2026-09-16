# Networking and Multiplayer Physics 1.0

## Initial model

- Godot 4.x multiplayer stack.
- Player-hosted **listen server** for core prototype/V1 foundation.
- Host process = server authority + one local player client.
- Other players = normal clients.
- Architecture must not require the authority to have a local camera/UI so dedicated/headless operation remains feasible later.

## Core rule

> **The server owns reality. Clients may predict presentation for responsiveness, but prediction never becomes game truth.**

No deterministic lockstep simulation is required. Canonical gameplay state/physics live on the server; clients receive snapshots/events and interpolate/predict where needed.

## Three kinds of truth

### Logical truth — always server-owned

Examples:

- match clock/phase;
- quest assignment/progress/results;
- incidents;
- customer state;
- checkout validation;
- player status values;
- zone membership used for scoring;
- final 15:00 snapshot.

### Physical truth — canonical server version

Examples:

- authoritative player transform;
- cart/rigid-body transform;
- door/freezer angle;
- collision/ragdoll outcome;
- puddle location/state.

### Presentation prediction — client-local temporary approximation

Examples:

- own movement;
- immediate held-object visual response;
- immediate throw response;
- local cart responsiveness.

Presentation can be corrected. Logical truth cannot.

## Initial rates/targets

| Data | Starting target |
|---|---:|
| Server physics | 50 Hz |
| Client movement input | ~30 Hz |
| Player authoritative snapshots | ~20 Hz |
| Held-object/cart updates while active | ~30 Hz |
| Ordinary active rigid bodies | ~15–20 Hz |
| Customers | ~10–15 Hz |
| Slowly moving doors | ~15 Hz |
| Match clock correction | ~2–5 Hz + reliable phase events |
| Remote interpolation buffer | ~100 ms (80–120 tunable) |

Sleeping rigid bodies receive a final state and then no continuous transform replication until they wake.

## Player input and prediction

Clients send input/intention, not authoritative position outcomes. Conceptually include:

- sequence number;
- movement vector;
- look/camera direction as needed;
- sprint/crouch/jump;
- interaction press/release;
- client timestamp.

Server processes input into authoritative player state.

Local player predicts movement immediately. Server snapshots include last processed input sequence; client reconciles prediction.

Starting correction feel target:

- <~0.10–0.15 m: smooth/invisible correction;
- ~0.15–0.5 m: quick smoothing;
- major invalid divergence: decisive correction/snap as necessary.

Server performs basic sanity validation against impossible speed/teleport/vertical movement.

## Interaction request flow

Client sends request referencing stable entity ID and requested interaction/grab context.

Server validates:

- entity exists;
- player/entity state permits interaction;
- distance/recent-history position is reasonable;
- exclusive holder state where relevant;
- requested capability exists.

Player-facing grab range = 2.0 m. Server may accept approximately 2.3–2.4 m using recent transform history to tolerate ordinary latency, without exposing a longer nominal gameplay range.

Server should retain roughly 250–500 ms recent player transform/look history for light lag-tolerant validation. Shooter-grade rollback is unnecessary.

## Local grab prediction

Client may immediately display predicted hold behavior after a locally plausible grab.

If server accepts, prediction continues/reconciles.

If rejected, predicted hold cancels and object returns to canonical presentation.

This is visual responsiveness; client never becomes canonical transform authority.

## Canonical held-object physics

Client controls **desired hold target/intent**. Server simulates the canonical rigid body and physical constraint.

Held objects must continue to collide with doors, shelves, players, NPCs, and props.

Do not teleport/weld rigid bodies to hand transforms.

Local predicted presentation may bias toward predicted hand pose while canonical body catches up, but large disagreement (usually real collision) must resolve to server state.

## Exclusive small-object conflicts

Small single-holder objects: first valid request processed by authority wins.

Rejected client's local prediction is cancelled.

No rollback transaction system is required for two people grabbing the same can.

## Cooperative/large objects

Objects explicitly supporting multi-grab may have multiple server-side constraints/forces simultaneously. No one client becomes sole physics owner.

Opposing players can physically resist one another.

Heavy/cooperative objects (~25–80 kg gameplay range) should move poorly with one player's force and substantially better with two without changing authored mass dynamically.

## Drop

Client predicts release. Server removes hold constraint and leaves canonical object with current physics state/minimal release contribution.

No throw impulse.

## Throw

Do not trust arbitrary client velocity vectors.

Client submits throw action/look intent. Server computes allowed impulse from authoritative player orientation, handling class, and tuning.

Client predicts the same throw for responsiveness.

Server records short-lived causal attribution such as last thrower/timestamp for statistics/physical causality.

Physical causal attribution should favor recent direct events and expire/supersede after roughly 3–5 sec unless a newer interaction/impact provides better cause.

## Shopping cart

The cart is always canonically server-authoritative because it interacts simultaneously with pusher, rider, cargo, players, NPCs, doors, and world geometry.

### Cart control

Active pusher sends forward/back/steering/control input. Server applies cart forces/steering.

Local pushing client may use predicted **visual** cart response if required by latency tests; prediction must not own canonical collisions.

Implementation order should begin with server-authoritative/interpolated cart and only add prediction when real latency testing proves necessary.

### Rider

Seat/rider relationship is authoritative. Server validates seat availability/range/state.

Server decides tip/impact ejection and triggers ragdoll. Rider may voluntarily exit at any time.

### Cargo

Loose objects remain real server-simulated rigid bodies; no hidden cart inventory.

At rest they should sleep and stop continuous network updates. Strong cart motion/collision wakes them.

## Doors/freezers

Server-authoritative hinge/physical state. Client interaction affects server simulation. Clients interpolate angle.

No advanced prediction initially required because motion is slow.

Logical freezer state is computed server-side:

- Closed ≤10°;
- Neutral 10–35°;
- Open ≥35°.

Quest/incident logic always uses server state.

## Breakables/puddles

Break thresholds are evaluated from canonical server impacts.

When break occurs:

- server changes persistent object state;
- emits authoritative ObjectBroken event;
- creates authoritative puddle if applicable;
- clients render sound/particles/cosmetic shards.

Cosmetic fragments need no network identity.

Puddle authoritative data includes ID, position, radius, active/cleaned state, and creator attribution when robust.

Server determines slip/ragdoll and authoritative cleaning progress.

## Player/customer ragdolls

Do **not** continuously replicate every ragdoll bone.

Server owns:

- ragdolled state;
- simplified authoritative root/body location;
- initial impulse;
- recovery timing;
- dragging interaction;
- post-recovery resistance.

Clients simulate detailed limb presentation locally from authoritative state/root. Minor limb differences between clients are acceptable.

Quest/zone/drag distance uses authoritative root, never cosmetic bone positions.

## Customers

Fully server-driven for logical behavior:

- state machine;
- shopping list;
- product reservation;
- navigation intent/path result handling;
- patience/complaints;
- queue/checkout;
- service/abandonment.

Clients interpolate customer transforms/animation from snapshots.

Customer ragdolls use the same authoritative-root/cosmetic-limb principle.

## Product reservation

Reservation is atomic on the server. If two customers target one final product, only one reserves it; the other follows unavailable/fallback logic.

## Checkout scanning

Client may predict obvious scan presentation, but server validates:

- checkout operational state;
- customer ownership;
- eligible unscanned product;
- valid scanner event/region;
- duplicate prevention.

Final required valid scan is authoritative CustomerServed event and records:

- customer ID;
- checkout ID;
- final scanner player ID;
- sale value;
- shared served count;
- Cashier quest credit.

Exactly once.

## Match clock

Server owns monotonic match time and phase transitions.

Clients display time derived from synchronized server clock with periodic correction. Do not maintain an independent authoritative local 15-minute timer.

At 15:00 the server alone captures EndSnapshot. Client prediction/late messages cannot alter it.

## Hidden quest privacy

Before results, detailed quest assignment/progress is sent only to the owning client (plus server authority). Do not broadcast hidden IDs and merely hide them in UI.

At results, server intentionally publishes assignments/completion for reveal.

## Incident privacy

Server may internally know causal attribution, but public incident packet contains only public incident ID/message/location/category defined by `INCIDENTS.md`.

Never transmit culprit data as part of player-facing incident payload.

## Spawn authority

Only server creates gameplay-authoritative runtime entities such as:

- customers;
- delivery boxes;
- puddles;
- authoritative break-state entities.

Clients may spawn non-gameplay particles/fragments/effects locally.

## Network IDs

Every replicated dynamic entity has stable per-match ID independent of NodePath. Internal representation may be integer/compact; human-readable debug aliases are useful.

## State vs event replication

Persistent conditions use replicated state/snapshots:

- transform;
- checkout on/off;
- door angle/state;
- plant intact/destroyed;
- player status;
- customer state.

One-shot important occurrences use reliable events:

- quest assignment/completion;
- incident announcement;
- delivery event;
- customer served;
- object break;
- authoritative phase/results transitions.

Frequent replaceable transforms/snapshots use unreliable/sequenced delivery. Do not reliably queue stale physics transforms.

Separate traffic/channels where supported so delayed reliable events do not block fresh movement/physics snapshots.

## Object sleeping and prioritization

At authoritative sleep:

1. send final canonical state;
2. stop normal continuous transform updates;
3. resume on wake due to player interaction/collision/support movement/event.

If bandwidth pressure requires prioritization, prefer local-interaction-relevant objects, carts/players, nearby active bodies, customers, then distant low-impact objects. Avoid MMO-grade spatial-interest infrastructure until measurements prove it necessary.

## Bandwidth target

Engineering target excluding voice:

> roughly **<500 kbit/s per client during normal play**, with temporary chaos spikes acceptable.

Server total outgoing for ten players should ideally remain low-single-digit Mbit/s class.

Voice bandwidth is profiled separately.

## Latency quality target

Comfortable target around 100 ms RTT; graceful degradation around 150 ms. 200+ ms may produce visibly awkward physics and is not required to feel identical.

Test profiles:

| Profile | RTT | Loss |
|---|---:|---:|
| Excellent | 20 ms | 0% |
| Normal | 60 ms | 0–1% |
| Acceptable | 100 ms | ~1% |
| Poor | 150 ms | 2–3% |
| Stress | 250 ms | 5% |

Prioritize testing movement, grab/carry/throw, cart, checkout, ragdoll, doors.

## Disconnect/reconnect

After ~2.5 sec without valid communication:

- declare disconnected;
- release held object;
- release PA/cart control;
- remove physical avatar so it cannot block world;
- reserve slot for 120 sec.

Reconnect within reservation restores logical personal state at a safe recovery point:

- persistent character;
- same quests/progress/completion;
- caffeine/intoxication/fullness;
- statistics.

Do not restore held object, cart seat, or ragdoll pose.

Absent avatars do not count for simultaneous social/zone quests.

## Host loss

Core prototype: host/process loss ends current match cleanly. No host migration required initially.

Before public networking is production-robust, host migration or dedicated public servers must be addressed separately.

## Headless/dedicated compatibility boundary

Do not couple MatchSession, customer logic, quests, incidents, or canonical physics to a local host camera/UI. This is architectural hygiene, not an instruction to deploy dedicated servers in V1.

## Desync/correction rules

Canonical server state always wins.

Client reconciliation must not itself generate gameplay events. Quest/incident/zone evaluation always uses authoritative transforms/states.

Prediction showing a watermelon briefly on the roof does not count unless server canonical body satisfies the zone condition.

## OOB recovery

Server alone decides OOB state/recovery:

- player invalid/OOB ~2 sec → safe recovery;
- object invalid/OOB ~5 sec → LostProperty placement.

Recovery is an authoritative teleport/state transition. Clients do not independently recover gameplay entities.

## Security scope

Basic server validation is required for:

- movement sanity;
- interaction distance/state;
- spawn authority;
- hidden quest privacy;
- arbitrary object impulses;
- checkout/customer/quest/results authority.

No kernel anti-cheat/industrial adversarial platform is required.

## Development network diagnostics

Development-only overlay should expose useful metrics such as:

- ping/loss;
- server physics rate;
- snapshot rates;
- active/sleeping bodies;
- prediction error/corrections;
- traffic by category;
- current interaction ownership/control.

Gameplay replication and voice traffic should be reported separately.

## Mandatory multiplayer physics gates

Before full content work, prove at representative latency:

- two remote players contest a medium object;
- carried watermelon respects door/shelf collision;
- thrown can converges to one canonical outcome;
- loaded cart with rider remains usable;
- cart impact causes one agreed player ragdoll event;
- two players cooperate on heavy object;
- ~20 cargo items spill without permanent divergence;
- freezer state agrees;
- glass breaks exactly once and creates one puddle.

Cosmetic physics need not be pixel-identical across clients. Shared meaningful facts must agree.
