# Master Tuning Sheet 1.0

These are the authoritative initial defaults. Values marked TUNABLE are expected to move during playtesting, but implementation should begin with the specified value rather than inventing substitutes.

## Match

| Parameter | Default | Status |
|---|---:|---|
| Match duration | 15:00 | LOCKED |
| Preparation end / forced opening | 03:00 | LOCKED |
| Delivery | 07:00 | TUNABLE |
| Morning Rush start | 13:00 | LOCKED |
| Shared customer target | 20 served | TUNABLE |
| Total potential customers | 32 | TUNABLE |
| Design player count | 10 | LOCKED |
| Public minimum start | 6 | TUNABLE |
| Private range | 2–10 | LOCKED |
| Quest reading/clock-in | ~6 sec | TUNABLE |
| Cosmetic post-zero physics settle | 0.75–1.0 sec | TUNABLE |

At exactly 15:00 the authoritative logical snapshot is taken **before** cosmetic physics settling.

## Opening criteria

Five current-state conditions, 4/5 required for manual OPEN STORE:

- 20 products correctly stocked.
- ≥5 carts in cart-staging zone.
- Checkout 1 operational.
- Checkout 2 operational.
- Entrance practically traversable/not severely obstructed.

All three checkouts begin OFF. Carts/entrance initially make the store approximately 2/5 ready.

## Player movement

| Parameter | Default |
|---|---:|
| Walk | 4.2 m/s |
| Sprint | 6.0 m/s |
| Backward | 3.6 m/s |
| Crouch | 2.3 m/s |
| Ground acceleration | 18 m/s² |
| Ground deceleration | 22 m/s² |
| Air control | ~30% of ground |
| Jump height | ~0.75 m |
| Stamina | none |
| Fall damage | none |

## Controller scale

| Parameter | Default |
|---|---:|
| Nominal visual/gameplay height | ~1.75 m |
| Visual generated range | ~1.65–1.85 m |
| Standing collider height | ~1.70 m |
| Collider radius | ~0.32 m |
| Crouched collider height | ~1.05 m |
| Post-ragdoll increased knockdown resistance | 1.25 sec |
| Player OOB recovery delay | ~2 sec |
| Manual Unstuck stationary delay | ~3 sec |

## Camera

| Parameter | Default |
|---|---:|
| Mode | third person |
| Distance | 4.0 m |
| Pivot/head height | ~1.5 m |
| Horizontal FOV target | ~75° |
| Collision | pull inward |

Drunk camera effects must be user-reducible/disableable.

## Interaction

| Parameter | Default |
|---|---:|
| Standard grab range | 2.0 m |
| Server latency-tolerant grab validation | ~2.3–2.4 m with recent-history validation |
| General use range | ~2.0 m |
| Precision movement multiplier | 0.50× |
| Placement-assist range | ~0.08–0.12 m |
| Simple placement confirmation | 1–3 sec |
| Multi-object arrangement confirmation | 3–5 sec |
| Direct inventory slots | 0 |
| Directly carried objects | 1 |

## Throw target ranges

| Class | Intended range |
|---|---:|
| Can/small box | 10–14 m |
| Medium box | 8–12 m |
| Watermelon | 5–8 m |
| Chair | 3–5 m |
| Heavy object | 1–3 m |

These are desired outcomes, not hard-coded trajectory distances.

## Handling classes

| Class | Typical gameplay mass | Intended behavior |
|---|---:|---|
| Small | 0–2 kg | easy carry |
| Medium | 2–10 kg | normal carry |
| Large | 10–25 kg | awkward carry |
| Heavy/cooperative | 25–80 kg | drag/poor solo handling; two players much better |
| Effectively immovable | >80 kg | not normally hand-carried |

## Key object dimensions/masses

| Object | Approx. size | Gameplay mass |
|---|---|---:|
| Watermelon | 0.30×0.25 m | 2.8 kg |
| Can | Ø0.07×0.12 m | 0.25 kg |
| Cereal box | 0.20×0.07×0.30 m | 0.35 kg |
| Small food box | ~0.16×0.06×0.22 m | 0.25 kg |
| Milk carton | 0.09×0.09×0.24 m | 0.70 kg |
| Baguette | ~0.55 m long | 0.30 kg |
| Toilet-paper pack | 0.45×0.25×0.30 m | 0.40 kg |
| Delivery box S | ~0.40×0.30×0.25 m | 3 kg |
| Delivery box M | ~0.55×0.40×0.35 m | 7 kg |
| Delivery box L | ~0.70×0.50×0.45 m | 12 kg |
| Chair | typical | ~5 kg |
| Fire extinguisher | ~0.55 m tall | ~5 kg |
| Staff table | 1.6×0.8×0.75 m | heavy |
| Vending machine | ~0.9×0.8×1.9 m | cooperative/heavy |
| Pallet | 1.2×0.8×0.15 m | ~15 kg |
| Shopping cart | 1.0×0.6×1.05 m | ~15 kg empty |

Gameplay stability is more important than real-world mass accuracy.

## Shopping carts

12 total:

- 5 indoor cart staging;
- 2 exterior cart return;
- 2 parking;
- 1 sales floor;
- 2 storage/loading.

One cart has a visually distinct red handle and is otherwise mechanically identical.

One rider maximum. Cart access to roof is required. Loaded cart should feel heavier. Steering worsens with speed.

## Physics

| Parameter | Default |
|---|---:|
| Authoritative physics baseline | 50 Hz |
| Sleeping rigid bodies | expected/enabled |
| Generic durability/HP | none |
| Arbitrary mid-round resets | none |

### Impact response

| Response | Duration |
|---|---:|
| Minor recoil | ~0.3 sec |
| Stumble | 0.7–1.0 sec |
| Normal ragdoll | 1.5–2.5 sec |
| Ordinary cap | ~3 sec |

## Voice

| Distance | Intended experience |
|---|---|
| 0–2 m | full |
| 2–6 m | clear |
| 6–10 m | audible |
| 10–14 m | quiet but understandable |
| 14–18 m | very faint |
| >18 m | effectively inaudible |

| Occlusion | Initial target |
|---|---:|
| Normal wall | ~7 dB + mild low-pass |
| Heavy wall | ~10 dB + mild low-pass |
| Closed door | ~6–8 dB |
| Open doorway | minimal |

Nameplate normal maximum: ~8 m and not through walls.

## PA

- One user at a time.
- Global map broadcast.
- Hold interaction.
- Broadcast ends on release, leaving radius, or ragdoll.
- No mandatory cooldown initially.
- Target artificial PA delay: ~100–150 ms.
- Narrow/compressed/mildly distorted presentation; intelligibility preserved.

## Coffee/caffeine

| Parameter | Default |
|---|---:|
| Reusable cups | 6 |
| Coffee supply | effectively unlimited |
| Fill time | ~1.5 sec |
| Drink time | ~1.0 sec |
| Caffeine/coffee | +12 |
| Maximum | 100 |
| Decay | none during shift |

Bands:

- 0–24 Normal
- 25–49 Alert
- 50–74 Wired
- 75–99 Over-caffeinated
- 100 Extreme

Approximate relative eye-scale target by band: 1.00×, 1.05–1.10×, 1.15–1.20×, 1.25–1.30×, ~1.35×.

Movement bonus starts at **0%**. Any future small speed bonus is EXPERIMENTAL.

## Alcohol/intoxication

12 drinkable employee items total: 8 breakable glass bottles + 4 unbreakable cans.

| Parameter | Default |
|---|---:|
| Intoxication/drink | +25 |
| Max | 100 |
| Drink time | ~1.5 sec |
| Decay | ~2 points/min during active shift |

Bands:

- 0–24 Sober
- 25–49 Slight
- 50–74 Drunk
- 75–100 Very Drunk

Quest canonical thresholds: Drunk ≥50, Very Drunk ≥75.

## Food/fullness

12 baguettes.

| Parameter | Default |
|---|---:|
| Fullness/baguette | +20 |
| Max | 100 |
| Eat time | ~2 sec |
| Decay | none during shift |

Quest canonical Stuffed threshold: ≥75.

Approximate belly increase: baseline, +5%, +10–12%, +18%, +22–25% at max.

## Spit

- Drunk-context mischief action.
- Cooldown: 2.5 sec.
- Mess radius: ~0.20 m.
- Cleanable: yes.
- Slippery: no.

## Puddles

| Parameter | Default |
|---|---:|
| Standard radius | ~1.0 m |
| Typical max/merged size | ~1.4 m |
| High-speed slip threshold | ~4.5 m/s |
| Standard cleaning | ~3 sec |
| Large cleaning | ~5 sec |
| Auto-disappear | never before round end |

## Customer population

| Parameter | Default |
|---|---:|
| Potential customers | 32 |
| Pre-opening waiting | 3 |
| 03:00–07:00 additional | 7 |
| 07:00–13:00 additional | 12 |
| 13:00–15:00 additional | 10 |
| Soft active target | 10 |
| Hard active ceiling | 12 |
| Typical shop-to-queue duration | ~45–90 sec |

Pre-opening arrivals approximately 01:15, 01:50, 02:20. They do not lose patience while waiting for scheduled opening.

### Shopping-list length

| Items | Probability |
|---:|---:|
| 1 | 20% |
| 2 | 40% |
| 3 | 30% |
| 4 | 10% |

Expected average: 2.3 categories.

### Category weights

| Category | Weight |
|---|---:|
| Boxed food/cereal | 18 |
| Canned food | 16 |
| Small packaged food | 14 |
| Non-alcoholic drinks | 15 |
| Milk/dairy | 10 |
| Frozen | 8 |
| Produce | 7 |
| Bakery | 5 |
| Household/cleaning | 4 |
| Toilet paper | 3 |

No duplicate category within one customer's list. Employee alcohol is excluded.

## Customer patience

Start 100, leave at 0.

| Event | Initial effect |
|---|---:|
| Desired product unavailable | -12 |
| Blocked >5 sec | -5 then gradual |
| Light collision | -2 |
| Strong stumble | -10 |
| Ragdoll | -25 |
| Slip | -15 |
| Checkout disabled while queued | -8 |
| Queue grace | 20 sec |
| Stronger queue frustration | after 45 sec |

Visual bands: 70–100 normal, 40–69 impatient, 1–39 angry, 0 abandon.

Recommended maximum complaint events/customer for stats: 3.

## Checkout

- 3 lanes.
- ~5 preferred queue positions/lane.
- ~0.9 m queue spacing.
- Conveyor ~0.5 m/s.
- Authoritative final required scan counts service immediately.
- Cosmetic payment/exit delay ~1.5 sec.

## Delivery

- Arrival 07:00.
- Warning/reversing sound around 06:50.
- 16 boxes.
- Distribution: 4 Drinks, 3 Canned, 3 Boxed, 2 Frozen, 2 Produce, 2 Household.

## World inventory baseline

| Prop | Count |
|---|---:|
| Watermelons | 40 |
| Cans | ~30 |
| Cereal boxes | ~24 |
| Small food boxes | ~16 |
| Milk cartons | ~16 |
| Small non-alcoholic drinks | ~16 |
| Large drinks | ~12 |
| Employee alcohol | 12 |
| Baguettes | 12 |
| Toilet-paper packs | 10 |
| Cleaning bottles | ~10 |
| Frozen boxes | ~12 |
| Shopping baskets | 10 |
| Shopping carts | 12 |
| Hand trucks | 2 |
| Pallets | 4 |
| Movable chairs | 6 |
| Wet-floor signs | 4 |
| Mops | 2 |
| Buckets | 2 |
| Fire extinguishers | 4 |
| Coffee cups | 6 |
| Vending machine | 1 |
| Office plant | 1 |

Watermelon distribution: 24 produce displays, 8 produce reserve, 8 storage.

## Map geometry targets

| Region | Approx. size |
|---|---:|
| Main building | 30×24 m |
| Indoor area | ~700 m² |
| Parking/exterior | 24×14 m |
| Loading exterior | ~8×7 m |
| Storage | 16×7 m |
| Staff room | 6×6 m |
| Manager office | 5×4 m |
| Accessible roof | ~15×10 m |
| Main entrance width | ~3 m |
| Roof access width | 1.8–2.0 m |
| Central cross-aisle | 2.2–2.5 m |
| Typical aisle walk width | ~2.0 m |
| Shelf height | ~1.8 m |
| Shelf depth/side | ~0.45 m |
| Shelf module width | ~1.0 m |

Traversal targets: manager office→entrance ~6–8 sec sprint; storage→produce ~6–8 sec; longest reasonable indoor route ≤10 sec.

## Quest generation

- 3 quests/player.
- Light effort = 1; Medium = 2; Heavy = 3.
- Personal effort budget target 5–7.
- Max one Heavy/player.
- Intentional conflict relationships in a 10-player round: ~2–4, target ~3.
- Prep-specific assignments/lobby: max ~3.
- Delivery-dependent assignments/lobby: max ~4.
- No hard-exclusive conflict pairs initially.
- Avoid exact same quest to same player in consecutive rounds where alternatives exist.

Finite-resource allocation targets:

| Resource | Available | Recommended allocated requirement cap |
|---|---:|---:|
| Watermelons | 40 | ~28 |
| Alcohol | 12 | ~8 |
| Baguettes | 12 | ~8 |
| Carts | 12 | ~8 simultaneous |
| Chairs | 6 | ~4 normally |
| Wet-floor signs | 4 | ~3 |
| Fire extinguishers | 4 | ~3 |

## Incidents

- Typical 2–4/match, ~3 target average.
- Chaotic upper expectation ~5.
- Global ordinary cooldown 45 sec.
- Same-incident cooldown 150 sec.
- Escalation minimum separation ~30 sec.
- Banner ~5 sec.
- No culprit display.

See `INCIDENTS.md` for exact triggers.

## Character generation

- ~60% normal-ish.
- ~30% noticeably odd.
- ~10% rare extreme.
- Usually max 2–3 strongly extreme traits.

Representative ranges:

| Trait | Common | Rare |
|---|---:|---:|
| Head overall | 0.92–1.10× | 0.85–1.20× |
| Head width | 0.92–1.08× | 0.85–1.15× |
| Ear size | 0.90–1.20× | 0.75–1.60× |
| Nose size | 0.90–1.15× | 0.75–1.50× |
| Eye size | 0.90–1.15× | 0.80–1.30× |
| Torso width | 0.92–1.10× | 0.85–1.20× |
| Limb thickness | 0.90–1.10× | 0.80–1.20× |

## Networking engineering targets

These are starting engineering targets, not player-facing promises:

- Authoritative physics: 50 Hz.
- Client movement input: ~30 Hz.
- Authoritative player snapshots: ~20 Hz.
- Held-object/cart canonical updates: ~30 Hz while active.
- Ordinary moving rigid bodies: ~15–20 Hz.
- Customers: ~10–15 Hz.
- Door snapshots while moving: ~15 Hz.
- Remote interpolation buffer: ~100 ms (roughly 80–120 ms tunable).
- Normal gameplay target: comfortable around 100 ms RTT, graceful around 150 ms.
- Non-voice gameplay bandwidth target: roughly <500 kbit/s/client during normal play.
- Disconnect declaration after ~2.5 sec of lost communication.
- Reconnect reservation: 120 sec.

Sleeping rigid bodies receive no continuous transform updates until woken.
