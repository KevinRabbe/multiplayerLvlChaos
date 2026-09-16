# Traceability and Consistency Audit

Purpose: ensure every authored player-facing objective/incident depends only on systems, objects, zones, and events explicitly defined elsewhere. Coding agents should not invent missing dependencies.

## Core system availability matrix

| System/content | Required by | Defined source |
|---|---|---|
| Semantic zones | quests/incidents/customers/results | `LEVEL_SUPERMARKET.md`, `ARCHITECTURE.md` |
| Stable dynamic object IDs/archetypes | networking/quests/results | `ARCHITECTURE.md`, `NETWORKING.md` |
| Authoritative match clock/snapshot | phase rules/end-state quests/results | `ROUND_LIFECYCLE.md`, `NETWORKING.md` |
| Grab/carry/place/throw | most physical quests | `PRODUCT_SPEC.md`, `TUNING.md` |
| Precision placement/stable stacking | PLACE-03, KEEP-03 | `PRODUCT_SPEC.md`, `ACCEPTANCE_TESTS.md` |
| Shopping-cart physics/rider | cart/social quests | `PRODUCT_SPEC.md`, `NETWORKING.md` |
| Ragdoll root/drag | SOCIAL-08/09 | `NETWORKING.md` |
| Freezer door hysteresis | CHAOS-03, KEEP-05, INC-03 | `QUESTS.md`, `INCIDENTS.md` |
| Checkout operational/scanner state | WORK-03/10, CHAOS-06, KEEP-08, incidents | `PRODUCT_SPEC.md`, `ROUND_LIFECYCLE.md` |
| Customer product reservation | shared objective/stock incidents | `PRODUCT_SPEC.md`, `ARCHITECTURE.md` |
| Delivery event/current-delivery identity | WORK-05, CHAOS-04, PLACE-09, INC-09/10 | `ROUND_LIFECYCLE.md` |
| Breakable glass/puddle causality | WORK-07/08, CHAOS-08/09, KEEP-09, incidents | `PRODUCT_SPEC.md`, `NETWORKING.md` |
| Coffee/caffeine | state/social quests | `PRODUCT_SPEC.md`, `TUNING.md` |
| Alcohol/intoxication | state/social quests | `PRODUCT_SPEC.md`, `TUNING.md` |
| Baguette/fullness | state/place quests | `PRODUCT_SPEC.md`, `TUNING.md` |
| Plant state | PLACE-06, KEEP-01, INC-12 | `PRODUCT_SPEC.md`, `QUESTS.md` |
| Entrance traversability | opening, WORK-09, KEEP-07, INC-06 | `QUESTS.md`, `INCIDENTS.md` |
| Lost Property/OOB recovery | fail-soft object rules | `LEVEL_SUPERMARKET.md`, `ROUND_LIFECYCLE.md` |
| Proximity voice/text | social information model | `PRODUCT_SPEC.md`, `NETWORKING.md` |
| PA one-user broadcast | social information model | `PRODUCT_SPEC.md`, `NETWORKING.md` |

## Quest dependency matrix

### WORK

| ID | Core dependencies |
|---|---|
| WORK-01 | sale-product categories, correct shelf zones, personal placement event |
| WORK-02 | 12 carts, CartStaging, personal cart movement/zone transition |
| WORK-03 | checkout scanner, CustomerServed final-scanner attribution |
| WORK-04 | frozen product category, Freezer storage zone/support |
| WORK-05 | 07:00 delivery, current-shift delivery identity, Storage zone |
| WORK-06 | watermelons, designated Produce displays, support state |
| WORK-07 | slippery puddles, mop cleaning completion attribution |
| WORK-08 | slippery puddles, 4 wet-floor signs, adjacency detector |
| WORK-09 | Preparation phase, stocking/cart/checkout action events, personal blocker removal causing Entrance obstructed→clear transition within 2 sec |
| WORK-10 | three checkout operational transitions + player attribution |

### PLACE

| ID | Core dependencies |
|---|---|
| PLACE-01 | 40 watermelons, Roof zone, EndSnapshot |
| PLACE-02 | watermelons, ManagerOffice zone, 5-sec confirmation |
| PLACE-03 | cans, cluster query, vertical relation/support |
| PLACE-04 | ≥6 toilet-paper packs in office-doorway volume + doorway severe-obstruction detector |
| PLACE-05 | 6 movable chairs, Aisle4 zone |
| PLACE-06 | unique plant, checkout conveyor support/powered state, cumulative timer |
| PLACE-07 | baguettes, StaffRoom zone, spatial cluster query |
| PLACE-08 | cart/chair/watermelon, Roof zone, cart roof access |
| PLACE-09 | current-delivery boxes, three checkout conveyors |
| PLACE-10 | shopping carts, Produce zone |

### CHAOS

| ID | Core dependencies |
|---|---|
| CHAOS-01 | cart staging occupancy |
| CHAOS-02 | Aisle3-authored product-home mapping, zone occupancy |
| CHAOS-03 | freezer door authoritative angle state |
| CHAOS-04 | current-shift delivery boxes, Storage membership |
| CHAOS-05 | Entrance zone, non-cart movable props |
| CHAOS-06 | StoreOpened + owner-caused Checkout3 operational→non-operational transition + sustained offline state |
| CHAOS-07 | canned product category, correct sales-zone mapping |
| CHAOS-08 | authoritative puddle creation causality |
| CHAOS-09 | active slippery puddles, wet-floor-sign adjacency |
| CHAOS-10 | direct complaint attribution events |

### STATE

| ID | Core dependencies |
|---|---|
| STATE-01 | CoffeeConsumed events |
| STATE-02 | intoxication + actual StoreOpened deadline |
| STATE-03 | intoxication threshold + spit sequence events |
| STATE-04 | CoffeeConsumed + baguette consumption + actual StoreOpened deadline |
| STATE-05 | final CustomerServed scanner + intoxication at event |
| STATE-06 | empty alcohol-container state + ManagerOffice EndSnapshot |
| STATE-07 | CoffeeConsumed + eligible-zone membership |
| STATE-08 | intoxication/caffeine thresholds |
| STATE-09 | baguette consumption events |
| STATE-10 | intoxication/caffeine/fullness thresholds |

### SOCIAL

| ID | Core dependencies |
|---|---|
| SOCIAL-01 | cart rider state + another player active pusher + cumulative time |
| SOCIAL-02 | quest-owner cart control + rider + Aisle1–8 zone visits |
| SOCIAL-03 | ManagerOffice player occupancy |
| SOCIAL-04 | StaffRoom + seated player state |
| SOCIAL-05 | cart control + rider + Storage zone crossing |
| SOCIAL-06 | player intoxication + 3 m authoritative proximity |
| SOCIAL-07 | CoffeeConsumed events + StaffRoom + rolling 60-sec unique-player window |
| SOCIAL-08 | cart actively pushed by another player + ragdoll attribution |
| SOCIAL-09 | ragdoll root + drag relationship + root distance |
| SOCIAL-10 | Roof player occupancy |

### KEEP

| ID | Core dependencies |
|---|---|
| KEEP-01 | unique plant Intact/Fallen/Destroyed + EndSnapshot |
| KEEP-02 | unique red-handle cart, indoor zone, upright orientation + EndSnapshot |
| KEEP-03 | can cluster/height query + sustained timer |
| KEEP-04 | StoreOpened + StaffRoom + SaleProduct classification |
| KEEP-05 | StoreOpened + all freezer doors Closed logical state |
| KEEP-06 | StoreOpened + ManagerOffice watermelon membership with 1-sec entry stability |
| KEEP-07 | StoreOpened + shared authoritative entrance-traversability detector |
| KEEP-08 | operational checkout count + StoreOpened |
| KEEP-09 | first-slippery-puddle-existed event + StoreOpened + active slippery-puddle count |
| KEEP-10 | StoreOpened + Produce display support state for watermelons |

## Incident dependency matrix

| ID | Detector/source |
|---|---|
| INC-01 | CartStaging occupancy ≤2/15 sec |
| INC-02 | CartStaging occupancy 0/10 sec |
| INC-03 | authoritative open freezer count ≥3/20 sec |
| INC-04 | previously operational checkout transitions offline after StoreOpened and remains offline 10 sec |
| INC-05 | operational checkout count ≤1 + active customers ≥4/10 sec |
| INC-06 | shared entrance severe-obstruction detector/10 sec |
| INC-07 | active slippery puddles ≥3/10 sec |
| INC-08 | rapid overlapping puddle-creation event window |
| INC-09 | current-delivery box locations + delivery timestamp |
| INC-10 | current-delivery box locations outside LoadingDock/Storage |
| INC-11 | ManagerOffice loose sale-product occupancy |
| INC-12 | unique plant Destroyed event |
| INC-13 | floor-supported sale-product detector |
| INC-14 | customer aisle traversability detector |
| INC-15 | accessible purchasable product count/category |
| INC-16 | rolling complaint-event timestamps |
| INC-17 | total queue-assigned customers |
| INC-18 | calculated current Cleanliness |

## Result metrics

These formulas are intentionally shallow and are not a hidden management simulation.

### Sales

Each successfully purchased product category has a fixed fictional value. Initial proposed values:

| Category | Value |
|---|---:|
| Small packaged food | 2 |
| Can | 2 |
| Milk | 3 |
| Cereal/boxed food | 4 |
| Non-alcoholic drink | 3 |
| Frozen food | 4 |
| Cleaning/household product | 5 |
| Toilet paper | 6 |
| Baguette | 2 |
| Watermelon/produce unit | 5 |

Sales = sum of values of successfully scanned/purchased products. Avoid attaching a real-world currency initially; display e.g. `Sales: 286`.

### Property damage

Initial event values:

| Event | Damage value |
|---|---:|
| Broken glass bottle | 5 |
| Destroyed office plant/pot | 100 |

No generic damage from knocking products/carts around. Additional fragile decoration must define value explicitly if added.

### Cleanliness

Start current cleanliness at 100 and derive deductions from current/end-snapshot mess state. Initial model:

| Current condition | Deduction |
|---|---:|
| Active standard slippery puddle | -6 each |
| Large/merged puddle | -10 |
| Spit mess | -1 each |
| Broken-glass site | -4 each |
| Every 5 qualifying loose sale products on floor | -2 per group of 5 |

Clamp 0–100. Cleaning/removing current mess removes its active deduction. Historical dirt that was cleaned does not permanently reduce final cleanliness.

### Satisfaction

Each customer contributes based on outcome/final patience:

| Outcome | Contribution |
|---|---:|
| Served with patience 70–100 | 100 |
| Served with patience 40–69 | 70 |
| Served with patience 1–39 | 40 |
| Abandoned | 0 |

Store Satisfaction = arithmetic mean across customers who entered.

### Store results display

Good default summary:

- Customers served: X / entered
- Sales
- Complaints
- Satisfaction %
- Cleanliness %
- Property damage

Optionally add one interesting world statistic (e.g. watermelons on roof) when nonzero/notable.

### Personal weird statistics

Track internally and select ~3–5 interesting nonzero/high values per player, candidate pool:

- coffees/alcohol consumed;
- time drunk/high-caffeine;
- baguettes eaten;
- watermelons moved;
- throws;
- cart crashes;
- cart push/ride distance;
- ragdoll time;
- players dragged;
- roof time;
- puddles created/cleaned;
- customers served;
- complaints caused;
- products stocked.

No scores/rankings/MVP derived from these.

## Resource-consistency notes

- Customers do **not** consume employee alcohol.
- Customers do not use employee coffee cups.
- Customers do not buy store equipment/tools/carts/baskets/delivery boxes/plant/chairs.
- Customer purchasing really removes eligible sale products from world stock.
- Delivery boxes do not unpack into additional sale stock in V1.
- Quest generator must leave slack according to `TUNING.md` resource budgets.
- Passive protection/endurance quests should be weighted toward rounds where opposing quests or natural pressure make them meaningful rather than auto-filling a player's task list with uncontested timers.

## Known future-undecided item

Temporary player death/KO + automatic return/respawn remains LATER/EXPERIMENTAL. Current V1 only has stumble/ragdoll/recovery. Do not add health/death dependencies to present quest/incident logic.

## Traceability completion state

Current design intent: **60/60 quests and 18/18 incidents have explicit deterministic dependency concepts and no required fuzzy intent inference.**

If implementation uncovers a missing dependency or a condition impossible to evaluate reliably, report it rather than substituting an undocumented interpretation.
