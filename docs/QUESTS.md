# Private Quest Catalogue 1.0

## Global rules

- Exactly **3 private quests per participating player** at round start.
- Other clients do not receive hidden quest IDs/details before results.
- No mid-match reroll/substitution.
- Failure carries no ranking penalty.
- Quest wording must be precise; humor belongs primarily in the title.
- Validation is deterministic from explicit state/events. Never infer intent.
- If a quest is impossible at assignment, generation is invalid and must regenerate.
- Later player actions may make a previously valid quest impossible; mark it privately failed only when impossibility is definitive.

### Effort

- Light = 1 point.
- Medium = 2 points.
- Heavy = 3 points.
- Target player effort budget: 5–7.
- Max one Heavy quest/player.

### Completion semantics

- **EventCounter** — count explicit authored events; permanent at threshold.
- **CurrentState** — condition must hold for confirmation window, then permanently completes.
- **SustainedState** — condition must remain continuously valid for stated duration; timer resets on break.
- **CumulativeState** — progress accumulates across valid intervals and does not reset.
- **Sequence** — ordered stages.
- **EndSnapshot** — evaluated only from the authoritative 15:00 snapshot.
- **MultiActorState** — simultaneous player state/zone requirement.

### Canonical state thresholds

- Drunk: intoxication ≥50.
- Very Drunk: intoxication ≥75.
- Highly Caffeinated: caffeine ≥75.
- Stuffed: fullness ≥75.

### Generator-wide limits

- Intentional conflict relationships/10-player match: ~2–4, target ~3.
- Prep-specific assignments/lobby: max ~3.
- Delivery-dependent assignments/lobby: max ~4.
- No deliberate hard-exclusive conflict pairs initially.
- Avoid same quest to same player in consecutive rounds where alternatives exist.
- Protection/endurance quests that can otherwise complete passively should be weighted toward rounds containing a meaningful opposing quest or natural pressure source rather than filling the pool with unchallenged timers.

Finite-resource allocation targets are defined in `TUNING.md`.

---

# WORK — productive / plausible supermarket work

## WORK-01 — Shelf Stacker

**Text:** Correctly stock 20 sale products.

- Effort: Medium.
- Type: EventCounter.
- Each product credits once when the quest owner personally places it in its valid category shelf/display zone and it remains physically supported there for at least 1 sec.
- Product orientation does not matter.

## WORK-02 — Cart Attendant

**Text:** Return 5 shopping carts to cart staging.

- Effort: Light.
- Type: EventCounter.
- A cart credits when personally moved into the cart-staging zone and remains there at least 3 sec.
- Each physical cart can credit this quest once.

## WORK-03 — Cashier

**Text:** Personally serve 5 customers at checkout.

- Effort: Medium.
- Type: EventCounter.
- Credit goes to the player who performs the final required authoritative scan for that customer.

## WORK-04 — Frozen Department

**Text:** Have 8 frozen products correctly stored in the freezer area at the same time.

- Effort: Light.
- Type: CurrentState.
- Required count: 8.
- Confirmation: 3 sec.

## WORK-05 — Delivery Worker

**Text:** Move 6 current-shift delivery boxes from the loading dock into Storage.

- Effort: Medium.
- Type: EventCounter.
- Delivery-dependent; tracker shows Waiting for delivery before 07:00.
- Only the 16 boxes spawned by the current shift's delivery qualify.

## WORK-06 — Produce Clerk

**Text:** Correctly place 10 watermelons on designated produce displays.

- Effort: Light.
- Type: EventCounter.
- Personally placed watermelon must remain supported on a valid produce display for at least 2 sec.

## WORK-07 — Cleanup Crew

**Text:** Clean 3 slippery puddles.

- Effort: Medium.
- Type: EventCounter.
- Quest owner must personally finish cleaning three distinct authoritative slippery puddles.
- Assign only when the configured round/content guarantees sufficient achievable spill opportunities.

## WORK-08 — Safety First

**Text:** Place warning signs beside 2 different active slippery puddles.

- Effort: Light.
- Type: EventCounter.
- A wet-floor sign must remain within the configured generous adjacency radius of an active slippery puddle for 3 sec.
- Same puddle cannot credit twice.

## WORK-09 — Opening Specialist

**Text:** During Preparation, personally complete 3 different qualifying opening actions.

- Effort: Medium.
- Type: EventCounter with unique categories.
- Preparation-only; fails permanently when the store opens if below 3/3.
- Qualifying categories, each max one credit:
  1. correctly stock a sale product;
  2. return a cart to staging;
  3. activate Checkout 1;
  4. activate Checkout 2;
  5. clear the entrance by personally moving/removing a blocking movable object such that the authoritative entrance detector transitions from severely obstructed to clear within 2 sec of that interaction.

## WORK-10 — Checkout Technician

**Text:** Personally activate all 3 checkout lanes during the shift.

- Effort: Light.
- Type: EventCounter by unique checkout ID.
- Each lane credits when the quest owner switches it from non-operational to operational at least once.

---

# PLACE — absurd physical placement

## PLACE-01 — Watermelon Logistics

**Text:** Have at least 15 watermelons on the roof when the shift ends.

- Effort: Heavy.
- Type: EndSnapshot.
- Counts authoritative roof-zone membership at 15:00.

## PLACE-02 — Executive Produce

**Text:** Have 8 watermelons inside the Manager Office at the same time for 5 seconds.

- Effort: Medium.
- Type: CurrentState.
- Required count: 8.
- Confirmation: 5 sec.
- Any player's actions can contribute.

## PLACE-03 — Modern Art

**Text:** Have 10 cans inside a 0.75 m-radius cluster, with at least one can resting above another, for 5 seconds.

- Effort: Medium.
- Type: CurrentState.
- Cluster must be freestanding/resting on world/other cans, not all currently held.
- Does not require a mathematically perfect tower.

## PLACE-04 — Toilet Fortress

**Text:** Use at least 6 toilet-paper packs to severely obstruct the Manager Office doorway for 10 seconds.

- Effort: Medium.
- Type: SustainedState.
- At least 6 toilet-paper packs must be inside the authored office-doorway obstruction volume while that doorway's authoritative traversability detector reports severe obstruction.

## PLACE-05 — Chair Department

**Text:** Have all movable chairs inside Aisle 4 at the same time.

- Effort: Medium.
- Type: CurrentState.
- Current V1 movable-chair count: 6.
- Confirmation: 3 sec.

## PLACE-06 — Plant Promotion

**Text:** Keep the office plant on an active checkout conveyor for 30 cumulative seconds.

- Effort: Medium.
- Type: CumulativeState.
- Plant must be intact/not Destroyed and physically supported by a powered/moving checkout conveyor.

## PLACE-07 — Bread Architecture

**Text:** Have 8 baguettes inside the same 1.25×1.25 m area in the Staff Room for 5 seconds.

- Effort: Light.
- Type: CurrentState.
- Confirmation: 5 sec.

## PLACE-08 — Roof Supplies

**Text:** Have a shopping cart, a movable chair, and a watermelon on the roof at the same time.

- Effort: Heavy.
- Type: CurrentState.
- Confirmation: 3 sec.

## PLACE-09 — Checkout Upgrade

**Text:** Have one current-delivery box on each of the 3 checkout conveyors at the same time.

- Effort: Medium.
- Type: CurrentState.
- Delivery-dependent.
- Confirmation: 3 sec.

## PLACE-10 — Produce Parking

**Text:** Have 3 shopping carts inside Produce at the same time for 5 seconds.

- Effort: Light.
- Type: CurrentState.

---

# CHAOS — controlled interference

## CHAOS-01 — Cart Shortage

**Text:** Keep all but 2 shopping carts away from cart staging for 60 seconds.

- Effort: Medium.
- Type: SustainedState.
- With 12 carts total, staging occupancy must remain ≤2 continuously.

## CHAOS-02 — Department Restructuring

**Text:** Keep at least 15 products whose correct home is Aisle 3 outside Aisle 3 for 30 seconds.

- Effort: Medium.
- Type: SustainedState.
- Only products whose authored correct sales zone is Aisle 3 count.

## CHAOS-03 — Open Door Policy

**Text:** Keep at least 3 freezer doors open for 45 seconds.

- Effort: Light.
- Type: SustainedState.
- Authoritative door state: Open at ≥35° from closed orientation; Closed at ≤10°; 10–35° neutral.

## CHAOS-04 — Storage Problem

**Text:** After delivery, keep at least 8 current-delivery boxes outside Storage for 90 continuous seconds.

- Effort: Medium.
- Type: SustainedState.
- Delivery-dependent.

## CHAOS-05 — Entrance Decoration

**Text:** Have 10 non-cart movable props inside the Entrance zone for 5 seconds.

- Effort: Light.
- Type: CurrentState.
- Shopping carts are excluded from this quest.

## CHAOS-06 — Checkout Reduction

**Text:** While the store is open, personally switch Checkout 3 from operational to non-operational, then keep it non-operational for 90 seconds.

- Effort: Medium.
- Type: Sequence → SustainedState.
- Stage 1 only begins from a real operational→non-operational transition caused by the quest owner after StoreOpened.
- Checkout 3 beginning the shift OFF does not grant progress.
- If Checkout 3 becomes operational during the 90-sec hold, the sustained timer resets; the owner may disable it again to restart the hold without repeating a separate permanent stage.

## CHAOS-07 — Product Relocation

**Text:** Have 10 canned products outside their correct sales shelves at the same time.

- Effort: Light.
- Type: CurrentState.
- Confirmation: 3 sec.

## CHAOS-08 — Wet Floor

**Text:** Personally create 3 slippery puddles.

- Effort: Medium.
- Type: EventCounter.
- Direct authoritative break/spill causality is required. If causality is ambiguous the puddle still exists but gives no personal credit.

## CHAOS-09 — Unsafe Workplace

**Text:** While at least one slippery puddle exists, keep every wet-floor sign away from active puddles for 30 seconds.

- Effort: Medium.
- Type: SustainedState.
- Sign adjacency uses the same detector as WORK-08.

## CHAOS-10 — Customer Relations

**Text:** Personally cause 5 attributable customer complaints.

- Effort: Medium.
- Type: EventCounter.
- Only robust direct causes qualify, initially:
  - hard body/cart impact by quest owner;
  - quest owner's actively pushed cart causes customer ragdoll;
  - customer slips on a puddle directly attributed to quest owner;
  - quest owner disables the checkout the customer is currently queued at;
  - quest owner removes one of that customer's unscanned checkout products and a complaint event is generated.
- General product shortages do not grant personal credit without robust attribution.

---

# STATE — consumption / visible employee state

## STATE-01 — Occupational Hazard

**Text:** Drink 10 coffees.

- Effort: Medium.
- Type: EventCounter.
- Coffee supply is effectively unlimited but a physical cup is required.

## STATE-02 — Liquid Courage

**Text:** Reach at least 50 intoxication before the store opens.

- Effort: Light.
- Type: CurrentState with deadline.
- Deadline is the actual StoreOpened event, including manual early opening.

## STATE-03 — Unprofessional Conduct

**Text:** Become Drunk, then spit on the floor 5 times.

- Effort: Medium.
- Type: Sequence.
- Stage 1: reach intoxication ≥50.
- Stage 2: perform 5 successful spit actions after Stage 1.
- Spits before Stage 1 do not count.
- Once Stage 1 occurs, later decay below 50 does not invalidate the sequence.

## STATE-04 — Breakfast of Champions

**Text:** Before the store opens, drink 5 coffees and eat 2 baguettes.

- Effort: Medium.
- Type: EventCounter bundle with actual-opening deadline.
- If the store opens before both thresholds are complete, quest permanently fails.

## STATE-05 — Perfect Employee

**Text:** Personally serve 3 customers while Drunk.

- Effort: Medium.
- Type: EventCounter.
- Quest owner must have intoxication ≥50 at the authoritative final scan event.

## STATE-06 — Evidence

**Text:** At shift end, have at least 5 empty alcoholic containers inside the Manager Office.

- Effort: Medium.
- Type: EndSnapshot.
- Empty means drinkable contents were consumed by an employee.
- Broken bottles do not count.

## STATE-07 — Caffeine Tour

**Text:** Drink coffee in 5 distinct eligible zones.

- Effort: Medium.
- Type: EventCounter by unique zone.
- Eligible: Produce, Bakery, any numbered Aisle, Freezer, Checkout area, Storage, Staff Room, Manager Office, Roof, Parking.
- Entrance and Back Corridor do not count.
- One drink credits one zone based on player's authoritative body centre.

## STATE-08 — Bad Combination

**Text:** Be Drunk and Highly Caffeinated at the same time.

- Effort: Light.
- Type: CurrentState.
- Requirements: intoxication ≥50 and caffeine ≥75.
- Completes immediately when both are true.

## STATE-09 — Bakery Problem

**Text:** Eat 5 baguettes.

- Effort: Medium.
- Type: EventCounter.

## STATE-10 — Absolute Specimen

**Text:** Simultaneously become Very Drunk, Highly Caffeinated, and Stuffed.

- Effort: Heavy.
- Type: CurrentState.
- Requirements: intoxication ≥75, caffeine ≥75, fullness ≥75.

---

# SOCIAL — multi-player interaction

## SOCIAL-01 — Passenger

**Text:** Ride in a shopping cart pushed by another employee for 60 cumulative seconds.

- Effort: Medium.
- Min players: 2.
- Type: CumulativeState.
- Other pusher/cart may change.
- Progress pauses when no other player actively controls the cart, rider exits, or rider is ejected.

## SOCIAL-02 — Taxi Driver

**Text:** While personally pushing a cart containing another employee, visit all 8 aisle zones during the shift.

- Effort: Heavy.
- Min players: 2.
- Type: EventCounter by unique aisle.
- Passenger may change between aisle credits.

## SOCIAL-03 — Office Meeting

**Text:** Keep at least 3 employees inside the Manager Office for 10 seconds.

- Effort: Medium.
- Min players: 3.
- Type: SustainedState / MultiActorState.

## SOCIAL-04 — Break Time

**Text:** Have 3 employees seated in the Staff Room at the same time.

- Effort: Medium.
- Min players: 3.
- Type: MultiActorState.
- Confirmation: 3 sec.

## SOCIAL-05 — Human Delivery

**Text:** Personally push a cart containing another employee into Storage.

- Effort: Light.
- Min players: 2.
- Type: Event.
- Owner must be active cart controller when cart crosses into Storage with rider aboard.

## SOCIAL-06 — Drinking Buddy

**Text:** While you are Drunk, spend 30 cumulative seconds within 3 m of another Drunk employee.

- Effort: Medium.
- Min players: 2.
- Type: CumulativeState.
- Multiple nearby drunk employees do not multiply accumulation rate.

## SOCIAL-07 — Mandatory Coffee Break

**Text:** Have 3 distinct employees consume coffee inside the Staff Room within the same rolling 60-second window.

- Effort: Medium.
- Min players: 3.
- Type: Event-window condition.
- Quest owner may be one of the three.

## SOCIAL-08 — Collision Test

**Text:** Be ragdolled by a shopping cart actively pushed by another employee twice.

- Effort: Light.
- Min players: 2.
- Type: EventCounter.
- Two distinct qualifying ragdoll impacts. Same other player may cause both.
- Uncontrolled environmental cart motion does not count.

## SOCIAL-09 — Ambulance

**Text:** Drag a ragdolled employee a cumulative 5 metres.

- Effort: Light.
- Min players: 2.
- Type: CumulativeState.
- Distance is authoritative ragdoll-root displacement while validly grabbed by quest owner.

## SOCIAL-10 — Roof Party

**Text:** Have at least 4 employees on the roof at the same time.

- Effort: Medium.
- Min players: 4.
- Type: MultiActorState.
- Confirmation: 3 sec.

---

# KEEP — protection / endurance

## KEEP-01 — Plant Guardian

**Text:** The office plant must not be Destroyed when the shift ends.

- Effort: Light.
- Type: EndSnapshot.
- Plant states: Intact, FallenButIntact, Destroyed.
- Plant may leave office/fall/move; only Destroyed fails.
- No deliberate destroy-the-plant hard-conflict quest in initial catalogue.

## KEEP-02 — Red Cart Custodian

**Text:** At shift end, the red-handle shopping cart must be upright and inside the supermarket.

- Effort: Medium.
- Type: EndSnapshot.
- Inside: cart centre within indoor building zone and not in recovery/OOB state.
- Upright: local up-axis within ~45° of upright.
- It does not need to be at cart staging.

## KEEP-03 — Perfect Stack

**Text:** Keep a 6-can stack/structure standing for 60 seconds.

- Effort: Medium.
- Type: SustainedState.
- At least 6 cans within a 0.5 m radius cluster.
- Highest qualifying can at least 0.45 m above the supporting surface.
- Slight leaning/pyramids are valid.

## KEEP-04 — Clean Freak

**Text:** While the store is open, keep the Staff Room free of sale products for 90 seconds.

- Effort: Medium.
- Type: SustainedState.
- Sale products count; normal staff-room equipment (cups, chairs, table, mop, etc.) does not.

## KEEP-05 — Freezer Discipline

**Text:** While the store is open, keep every freezer door Closed for 60 seconds.

- Effort: Medium.
- Type: SustainedState.
- Closed = ≤10° from closed orientation.
- A neutral/open door breaks the condition.

## KEEP-06 — No Produce Executives

**Text:** While the store is open, keep the Manager Office at zero watermelons for 90 seconds.

- Effort: Medium.
- Type: SustainedState.
- A watermelon only counts inside after its centre remains in office zone for at least 1 sec; a watermelon flying through the doorway does not reset the timer.

## KEEP-07 — Clear Entrance

**Text:** While the store is open, keep the entrance practically clear for 60 seconds.

- Effort: Medium.
- Type: SustainedState.
- Uses the same authoritative entrance-traversability/severe-obstruction detector as opening readiness and the entrance incident. One stray small product must not invalidate the condition.

## KEEP-08 — Checkout Hero

**Text:** Maintain at least 2 operational checkout lanes for 120 continuous seconds while the store is open.

- Effort: Medium.
- Type: SustainedState.

## KEEP-09 — Dry Floor

**Text:** After at least one slippery puddle has existed during the shift, restore the store to zero active slippery puddles and keep it dry for 180 continuous seconds.

- Effort: Heavy.
- Type: Sequence → SustainedState.
- Stage 1 arms once any authoritative slippery puddle has existed during this shift.
- Stage 2 requires zero active slippery puddles for 180 continuous seconds while the store is open.
- Spit does not count because it is non-slippery.
- The owner may deliberately create/clean a puddle if nobody else does; the quest must not complete passively before any spill has occurred.

## KEEP-10 — Orderly Produce

**Text:** While the store is open, keep at least 10 watermelons correctly supported on designated produce displays for 60 seconds.

- Effort: Medium.
- Type: SustainedState.
- Watermelons merely lying on the Produce floor do not count.

---

# Intentional soft-conflict graph

Typical pairings:

- WORK-02 Cart Attendant ↔ CHAOS-01 Cart Shortage.
- WORK-05 Delivery Worker ↔ CHAOS-04 Storage Problem.
- WORK-08 Safety First ↔ CHAOS-09 Unsafe Workplace.
- PLACE-02 Executive Produce ↔ KEEP-06 No Produce Executives.
- PLACE-07 Bread Architecture ↔ KEEP-04 Clean Freak.
- CHAOS-03 Open Door Policy ↔ KEEP-05 Freezer Discipline.
- CHAOS-05 Entrance Decoration ↔ KEEP-07 Clear Entrance.
- CHAOS-06 Checkout Reduction ↔ KEEP-08 Checkout Hero.
- CHAOS-08 Wet Floor ↔ WORK-07 Cleanup Crew.
- CHAOS-08 Wet Floor ↔ KEEP-09 Dry Floor.
- WORK-06/KEEP-10 Produce activity ↔ PLACE-01 Watermelon Logistics as resource/location tension.

Generator should intentionally create roughly 2–4 conflict relationships in a 10-player round but must also respect resource budgets. Example: PLACE-01 + PLACE-02 + KEEP-10 together demand too many simultaneous watermelons and should be prevented by resource-aware incompatibility.

# First-round onboarding pool

For a player's first 1–2 completed shifts, prefer a limited understandable pool such as:

- WORK-01, WORK-02, WORK-03, WORK-06;
- PLACE-02, PLACE-03, PLACE-07;
- CHAOS-03, CHAOS-05;
- STATE-01, STATE-08;
- SOCIAL-01, SOCIAL-03, SOCIAL-04;
- KEEP-05, KEEP-06.

This is onboarding eligibility, not progression/reward.

# Results reveal

At results show each player's exact assigned title, exact condition text, and completion/failure state. Do not expose a forensic action timeline or exact incident attribution.
