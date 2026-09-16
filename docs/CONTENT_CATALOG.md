# V1 Content and Object Catalogue

This document defines the initial physical archetypes required by V1. Counts are initial tuning defaults; one reusable scene/archetype should normally be instantiated many times rather than creating unique per-instance implementations.

## General rules

- Sale products and store equipment are distinct semantic categories.
- No conventional player inventory; physical world objects remain visible and shared.
- No personal ownership of loose objects.
- Generic durability/HP is not used.
- Most objects are unbreakable; breakage is explicitly opt-in.
- Object gameplay mass is tuned for stable/funny handling rather than strict real-world accuracy.
- Stable per-match instance IDs are required for networked dynamic entities.
- Decorative filler may exist behind/around interactive products to make shelves look fuller, but should not create confusing “identical object sometimes fake” interactions in obvious reachable positions.

## Sale product archetypes

| Archetype | Initial count | Approx. size/mass | Key behavior |
|---|---:|---|---|
| Watermelon | 40 | 0.30×0.25 m, 2.8 kg | signature medium prop, rolls, throwable, unbreakable V1 |
| Can | ~30 | Ø0.07×0.12 m, 0.25 kg | stable stacking, throwable; several label/material variants |
| Cereal/large box | ~24 | 0.20×0.07×0.30 m, 0.35 kg | simple box, stockable, small |
| Small packaged-food box | ~16 | 0.16×0.06×0.22 m, 0.25 kg | may share base geometry with visual variants |
| Milk carton | ~16 | 0.09×0.09×0.24 m, 0.70 kg | dairy category |
| Small non-alcoholic drink | ~16 | small bottle/can | normal sale product |
| Large non-alcoholic drink | ~12 | larger bottle | normal sale product |
| Frozen-food box | ~12 | small/medium box | freezer category |
| Cleaning/household bottle | ~10 | bottle | household category, visually distinct from drinks |
| Toilet-paper pack | 10 | 0.45×0.25×0.30 m, 0.40 kg | bulky/light; useful for barricades |
| Baguette | 12 | ~0.55 m, 0.30 kg | sale product + player consumable/fullness |

### Watermelon initial distribution

- 24 Produce displays.
- 8 Produce reserve.
- 8 Storage.

## Employee alcohol

Exactly **12 employee-drinkable alcohol items**:

- 8 glass bottles — breakable;
- 4 metal cans — unbreakable.

Customers do not purchase these items.

Alcohol is a finite shared player resource. Once consumed/broken it is gone for the remainder of the shift. Empty consumed containers remain physical and quest-relevant.

## Delivery boxes

16 spawned at 07:00.

Three reusable scale/mass classes:

| Size | Approx. dimensions | Mass |
|---|---|---:|
| Small | 0.40×0.30×0.25 m | 3 kg |
| Medium | 0.55×0.40×0.35 m | 7 kg |
| Large | 0.70×0.50×0.45 m | 12 kg |

Category labels/distribution:

- Drinks 4
- Canned goods 3
- Boxed food 3
- Frozen 2
- Produce 2
- Household 2

Boxes do **not** unpack into sale inventory in V1.

## Shopping carts

12 total; one reusable cart scene/archetype.

Initial distribution:

- 5 CartStaging;
- 2 exterior cart return;
- 2 parking;
- 1 sales floor;
- 2 storage/loading.

One cart has a **red handle** and stable unique identity for KEEP-02.

Required behavior:

- server-authoritative physical pushing;
- speed-dependent steering degradation;
- one rider;
- physical loose cargo;
- tipping/spilling;
- severe-impact rider ejection;
- impacts can produce player/customer stumble/ragdoll;
- roof route compatible;
- ~15 kg empty gameplay mass;
- loaded feel heavier through physics/handling.

No hidden cart inventory.

## Shopping baskets

10 physical containers:

- 6 entrance;
- 2 checkout/service;
- 2 storage.

Loose props placed inside can spill when tipped. NPC customers do not use these baskets in V1.

## Staff-room equipment

### Coffee machine

- 1 fixed/special interaction object.
- Effectively unlimited coffee during one shift.
- Fill ~1.5 sec.
- Requires one reusable cup.

### Coffee cups

- 6 reusable physical cups.
- Drink ~1 sec.
- Become empty/reusable after consumption.
- No magical duplication if hidden; if all six are unavailable, coffee cannot currently be consumed.
- True OOB cup recovers through Lost Property.

### Staff table

- 1 movable heavy table.
- ~1.6×0.8×0.75 m.

### Chairs

6 movable chairs total across the store, including standard staff chairs and manager rolling chair.

Normal chair target ~5 kg. Marked seats may be used; no universal freeform sitting system.

### Vending machine

- 1 heavy/cooperative movable/drag object.
- ~0.9×0.8×1.9 m.
- No vending/economy minigame required initially unless explicitly promoted later; physical/social role is enough.

## Manager office props

### Office plant

One unique movable/breakable plant.

Logical states:

- Intact;
- FallenButIntact;
- Destroyed.

Falling/moving does not itself destroy it. Destruction is permanent for that shift and supports quest/incident/result logic. OOB while intact recovers to Lost Property; Destroyed does not resurrect.

### Manager rolling chair

Movable/rollable, counts among the 6 movable chairs.

### Monitor

One movable physical monitor. Keyboard/mouse may be merged/decorative static props rather than separate physics bodies.

### Desk

Static in V1.

## Cleaning/safety equipment

### Wet-floor signs

4 physical movable signs.

### Mops

2 physical mops. Valid mop/puddle contact accumulates cleaning time.

### Buckets

2 physical buckets. No fluid simulation/refill requirement; thematic/physics object.

### Fire extinguishers

4 physical extinguishers, ~5 kg each, suggested spawn locations:

- service/entrance wall;
- back corridor;
- Storage;
- LoadingDock.

**No spray functionality in V1.**

## Logistics equipment

### Hand trucks

2 physical hand trucks.

### Pallets

4 physical pallets, ~1.2×0.8×0.15 m, gameplay mass ~15 kg.

No forklift/pallet-jack system in V1.

### Bins

- 3 small office/staff bins.
- 2 large wheeled bins.

They may be physical containers/obstacles but do not require detailed garbage simulation.

## Promotional signs

~6 movable promotional signs. Simple reusable geometry/material variants.

## Checkout equipment

Each of 3 lanes:

- static counter;
- conveyor (~0.5 m/s initial target);
- authoritative on/off/operational state;
- scanner with distinct beep;
- queue anchors/space.

No cash/change/receipt minigame.

## Freezers

Four main freezer units, 8 interactive doors total.

Doors are physical hinged stateful objects. Server logical thresholds:

- Closed ≤10°;
- Neutral 10–35°;
- Open ≥35°.

## Entrance doors

Automatic entrance/customer doors activate at StoreOpened and remain open/operational according to the authored entrance behavior. They are not a player-controlled re-closing mechanic after opening.

## Staff/manager/loading doors

- staff swing doors as physical doors where used;
- manager office physical door;
- LoadingDock roller door ~3×3 m.

Large carried boxes should be able to become awkward/stuck in doorways without being impossible to move.

## Breakables

V1 breakable whitelist:

- 8 employee alcohol glass bottles;
- office plant/pot;
- optional explicitly documented small fragile decoration only if added later to V1.

Everything else is unbreakable unless its definition explicitly says otherwise.

### Glass break result

- intact object enters broken/removed state;
- authoritative break event;
- cosmetic non-gameplay shards (roughly 2–4 visual pieces, no network rigid-body identity required);
- authoritative slippery puddle when liquid-bearing;
- breakage attribution when robust;
- no generic durability.

## Puddles/mess

### Slippery puddle

- standard radius ~1.0 m;
- may merge/cluster up to ~1.4 m typical visual/gameplay size;
- persists until cleaned/round end;
- ordinary walking essentially safe;
- speed ~4.5 m/s+ creates strong slip risk/condition;
- cleaning standard ~3 sec, large ~5 sec.

### Spit mess

- ~0.20 m visual area;
- cleanable;
- not slippery;
- counts toward cleanliness;
- generated from drunk-context Mischief action with 2.5 sec cooldown.

No realistic fluid simulation.

## Physical containers

Carts/baskets/boxes may physically contain loose props through ordinary rigid-body collision. Do not replace them with hidden inventory semantics.

## Handling categories

- Small: 0–2 kg.
- Medium: 2–10 kg.
- Large: 10–25 kg.
- Heavy/cooperative: 25–80 kg.
- Effectively immovable: >80 kg.

The category defines interaction feel/throw/constraint behavior; it is not a realistic mass taxonomy.

## Asset-reuse priorities

Reuse where visual readability remains clear:

- cereal/generic boxes can share base mesh/material variants;
- frozen/small food boxes may share geometry;
- delivery boxes share one mesh with scale presets;
- bottle families may share compatible base geometry while semantic/material definitions remain distinct.

Do not merge assets merely because geometry can technically be shared if it makes drinks/cleaners/alcohol hard to distinguish.

## Non-content in V1

Do not add:

- forklifts;
- pallet-jack mechanics;
- drivable cars;
- weapons;
- extinguisher spray;
- cooking/baking;
- bathrooms;
- generic item HP;
- full structural destruction;
- true fluids;
- customer-use physical baskets;
- delivery-box unpacking/inventory.
