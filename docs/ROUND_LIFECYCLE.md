# Authoritative Round Lifecycle

This document defines exactly when systems activate, score, reset, or become available.

## Pre-round

Before the 15-minute clock begins:

- required participants have loaded;
- supermarket is fully reset to canonical initial state;
- next-round roster is locked;
- quest generator assigns exactly 3 quests/player using the actual roster;
- private quest data is sent only to the owning player;
- players begin with caffeine/intoxication/fullness = 0;
- no held object/ragdoll state carries over;
- persistent character seed/appearance remains;
- players stand together in employee staging and receive ~6 sec to read quests.

Delivery-dependent quests may already be assigned but show **Waiting for delivery** until 07:00.

At the end of the reading countdown:

> **CLOCK IN**

The authoritative match timer becomes 00:00.

---

## 00:00 initial world state

Required starting conditions:

- 40 watermelons in canonical distribution;
- all authored initial sale products/equipment present;
- 12 carts in specified distribution;
- 6 chairs in canonical positions;
- plant Intact;
- zero active puddles;
- zero broken initial glass;
- no current-shift delivery boxes;
- roof clear of movable round content;
- all three checkouts OFF;
- entrance/customer doors closed;
- normal employee interactions (PA, coffee, alcohol, props, freezers, doors) already usable.

Opening readiness begins approximately 2/5 because CartStaging and Entrance are initially valid.

## 00:00–03:00 Preparation

Persistent shared opening checklist displays current world state:

1. ≥20 products correctly stocked.
2. ≥5 carts in CartStaging.
3. Checkout 1 operational.
4. Checkout 2 operational.
5. Entrance practically clear.

Need 4/5 for manual OPEN STORE.

These are **live conditions**, not permanently checked accomplishments. If players undo one, readiness can fall again.

Normal corporate incidents are inactive during Preparation except explicit exceptional state-change incidents such as office-plant destruction.

### Opening Specialist

Private WORK-09 tracks qualifying player actions permanently despite shared checklist being live. It fails at StoreOpened if below 3 unique action categories.

## Pre-opening customer arrivals

Approximately:

- 01:15 customer A;
- 01:50 customer B;
- 02:20 customer C.

They wait outside. They do **not** lose patience while waiting for scheduled opening.

## Manual early opening

When current readiness is ≥4/5, the physical/service-desk OPEN STORE control is enabled.

If readiness drops below 4/5 before activation, the control disables again.

When a player activates OPEN STORE:

- StoreOpened event becomes irreversible;
- entrance/customer doors activate;
- Preparation checklist disappears;
- Preparation-only quests immediately resolve success/failure against this actual opening moment;
- waiting customers enter;
- normal incident monitoring activates;
- the global timeline remains unchanged.

Early opening does **not** move 03:00 customer schedule, 07:00 delivery, 13:00 Rush, or 15:00 shift end.

## 03:00 forced opening

If not already opened, the authority opens the store exactly at 03:00 regardless of readiness.

If readiness ≥4/5:

> **STORE OPEN**

If readiness ≤3/5:

> **STORE OPENED UNPREPARED**

No arbitrary extra penalty. Whatever is genuinely unfinished remains unfinished and affects customers/world normally.

## 03:00 customer schedule begins

Seven additional customer arrivals are scheduled between 03:00–07:00.

If active-customer hard ceiling (12) is reached, scheduled arrivals wait until a slot becomes available rather than being discarded.

Early opening only allows the three pre-opening customers to enter earlier; it does not create extra customers.

## Customer product lifecycle after opening

Authoritative lifecycle:

1. sale product is available in correct customer-accessible zone;
2. customer reserves/takes it;
3. during normal shopping it is non-grabbable/customer-reserved;
4. at checkout it becomes physical/interactable until scanned;
5. valid scan marks it scanned/bagged/reserved;
6. final required scan immediately emits CustomerServed, increments shared count, awards final-scanner Cashier credit, and records sales;
7. cosmetic payment/exit sequence (~1.5 sec) follows but does not delay logical service;
8. purchased products leave the world with the customer.

## 03:00–07:00 normal morning

All normal systems active:

- customers;
- checkout;
- private quests;
- incidents;
- consumables;
- persistent world changes;
- proximity communication/PA.

No automatic cleanup/reset occurs.

## 06:50 delivery warning

Approximate reversing/truck audio from loading side.

No mandatory large UI countdown.

## 07:00 Delivery

Exactly once:

> **DELIVERY ARRIVED**

This is a phase/event announcement and does not consume incident cooldown.

Sixteen current-shift delivery boxes become available in safe LoadingDock positions regardless of player clutter.

Delivery-dependent quest trackers move from Waiting for delivery to active progress.

The delivery does not generate extra sale products from inside boxes.

## Delivery incident timing

Delivery Backlog cannot fire until roughly 09:00 because its definition requires two minutes after delivery plus trigger persistence.

Delivery-scatter/backlog detectors are unarmed before the delivery event.

## 07:00–13:00 busy period

Twelve additional customers are scheduled across this period, respecting active ceiling.

No automatic reset or cleanup at phase boundaries.

Caffeine/fullness do not decay. Intoxication decays by current tuning only while active match time advances.

## Quest runtime rules

### Permanent completion

EventCounter, qualifying CurrentState, Sequence, and completed CumulativeState quests stay complete once their authoritative completion condition fires.

Example: PLACE-02 completes after 8 watermelons remain in office for 5 sec; later removal does not undo completion.

### SustainedState

Timer resets when condition breaks unless definition explicitly says cumulative.

### EndSnapshot

Never completes early. It displays current useful progress but is evaluated only at 15:00 authoritative snapshot.

## 13:00 Morning Rush

Exactly at 13:00:

> **MORNING RUSH — 2:00 REMAINING**

Ten additional customer arrivals are scheduled across the final two minutes subject to active ceiling.

No player/NPC speed buff, physics multiplier, or artificial disaster. The same systems simply face more demand.

### Optional shift status summary

At ~13:00/13:05 the game may show one compact non-culprit operational summary such as:

- customers served / target;
- operational checkouts;
- active customers;
- staged carts;
- active slippery spills.

Never show culprit identity, quest data, player locations, or hidden object locations.

## Final minute

At 14:00:

> **1 MINUTE REMAINING**

The normal always-visible timer remains authoritative presentation. A subtle final 10-sec visual emphasis is allowed; do not create loud per-second announcer spam.

## Exactly 15:00

At **15:00.000 authoritative server time**:

1. stop player input for scoring purposes;
2. stop customer AI/progress;
3. stop quest progress;
4. stop incident generation;
5. capture immediate authoritative logical EndSnapshot;
6. calculate EndSnapshot quests and snapshot-derived store metrics;
7. only after the snapshot, optionally allow ~0.75–1.0 sec cosmetic physics settling;
8. freeze the world for results presentation.

Anything that enters a target zone after 15:00 does not count.

Example: a fifteenth watermelon still airborne at 15:00 fails Watermelon Logistics even if it lands at 15:00.2.

A final checkout scan at 14:59.8 already emits CustomerServed immediately and therefore can reach 20/20 before zero even if payment animation continues after zero.

## Results

Once results begin:

- voice/text become global;
- final frozen supermarket should remain visible initially;
- first show team result/store metrics for ~3–5 sec;
- then reveal each player's exact 3 quests/completion states;
- character shown with temporary appearance as captured at 15:00;
- no forensic culprit timeline;
- no MVP/individual ranking;
- total default presentation target ~60–90 sec max and accelerable.

Team result:

- ≥20 served: **SHIFT SURVIVED**
- <20 served: **MANAGEMENT IS NOT IMPRESSED**

## Results data sources

Snapshot-derived examples:

- cleanliness;
- watermelons on roof;
- red cart state;
- plant survival/location state.

Event-history examples:

- total sales;
- complaints;
- customers served;
- coffees/alcohol consumed;
- cart crashes;
- quest event progress.

## Between rounds

After results, return to a global-communication lobby/ready state. Players do not free-roam the old supermarket indefinitely.

The next round uses a **full canonical world reconstruction/reset**, not manual reversal of previous transforms.

Reset includes:

- sale products/spawns;
- carts;
- chairs/tools;
- plant alive/intact;
- zero puddles;
- no delivery boxes;
- freezer/doors canonical;
- all checkouts OFF;
- coffee cups/alcohol/baguettes restored;
- customers gone;
- LostProperty cleared;
- roof cleared;
- transient player states zeroed.

Persistent character identity remains.

## Next-round roster lock

Do not generate quests while lobby population is still changing.

When next shift is ready to launch:

1. lock participating roster;
2. generate/validate full quest assignment set against that roster;
3. reset/load world;
4. deliver each player's private assignments;
5. enter pre-round reading state;
6. CLOCK IN.

Non-participating/waiting connected users do not consume resource budgets/conflict assignments.

## Disconnect/reconnect during active shift

Transient connection loss:

- 0–~2.5 sec: tolerate as network interruption, neutralize unsafe stale inputs as needed.
- after ~2.5 sec: declare disconnected; release held object, PA, cart control; remove physical avatar so it cannot block geometry; reserve slot.
- reservation: 120 sec.

On valid reconnect within reservation:

- same persistent character;
- same three quests;
- same logical progress/completion;
- same caffeine/intoxication/fullness/statistics;
- safe normal-state re-entry at employee recovery point;
- no restoration of held object/cart seat/ragdoll pose.

World continues while player is absent. Simultaneous-presence quests do not count absent avatars.

If reservation expires, player's quests are abandoned/removed for remainder of shift and are not redistributed.

## Host loss in initial prototype

Host/process loss ends the match cleanly and returns clients to lobby/menu if possible with a clear connection-loss message. Do not fake normal results from incomplete/corrupt state.

Host migration/dedicated public-server robustness is later release hardening.
