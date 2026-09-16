# Supermarket Level Specification 1.0

## Coordinate convention

Design-space only; exact Godot scene coordinates may mirror this directly.

- 1 design unit = 1 metre.
- Indoor southwest/front-left corner = `(0, 0)`.
- +X = east/right.
- +Z = north/back.
- Customer entrance on south/front wall.

Main building target: **30×24 m** (~700 m² indoor).

Small exterior/parking extends south; loading exterior extends west/back. Accessible maintenance roof covers part of the service wing.

## Social-geography goals

The map is intentionally compact.

- Players on the sales floor should frequently see/hear one another.
- Storage and roof should create partial information/isolation.
- Entrance/checkouts should be highly public.
- Manager office should be partly visible through glass.
- One central position must not provide total visual surveillance of the whole store.
- Customers should have alternate routes around ordinary aisle blockage.

Traversal targets:

- Manager office → entrance sprint: ~6–8 sec.
- Storage → produce sprint: ~6–8 sec.
- Longest reasonable indoor route: ≤10 sec.

## Conceptual layout

```text
                                  NORTH / BACK

       LOADING EXTERIOR
      ┌───────────────┐
      │               │
┌─────┴───────────────┬─────────────────┬─────────────┬─────────┐
│                     │                 │             │ Roof    │
│   STORAGE           │   STAFF ROOM    │  MANAGER    │ access  │
│                     │                 │  OFFICE      │         │
│                     │                 │ [glass wall] │         │
├─────────────────────┴─────────────────┴─────────────┴─────────┤
│                                                               │
│ BAKERY       AISLES 5  6  7  8                  FREEZERS      │
│                                                               │
│              ───── CENTRAL CROSS AISLE ─────                  │
│                                                               │
│ PRODUCE      AISLES 1  2  3  4                  DAIRY/DRINKS  │
│                                                               │
├───────────┬────────────────────────────────────┬───────────────┤
│ CARTS /   │ CHECKOUT 1   CHECKOUT 2   CHECKOUT 3 │ SERVICE   │
│ BASKETS   │                                      │ DESK + PA  │
│           │              ENTRANCE                │             │
└───────────┴──────────────────┬───────────────────┴─────────────┘
                              │
                         FRONT SIDEWALK

                    SMALL PARKING / CART RETURN

                                  SOUTH / FRONT
```

## Functional zones

| Zone | Approximate design coordinates | Size / notes |
|---|---|---|
| Entrance vestibule | X 12–18, Z 0–3.5 | ~6×3.5 m |
| Cart staging/baskets | X 1–6, Z 1–5 | ~5×4 m |
| Checkout zone | X 6–23, Z 1–5.5 | ~17×4.5 m |
| Service desk/PA | X 23–29, Z 1–5 | ~6×4 m |
| Produce | X 1–7, Z 5.5–11 | ~6×5.5 m |
| Bakery | X 1–7, Z 11–16 | ~6×5 m |
| Central aisles | X 8–23, Z 6–16 | ~15×10 m |
| Dairy/drinks | X 23.5–29, Z 5.5–11 | ~5.5×5.5 m |
| Freezers | X 23.5–29, Z 11–16 | ~5.5×5 m |
| Back corridor | X 0–30, Z 16–17 | at least ~1 m clear, wider locally as needed |
| Storage | X 0–16, Z 17–24 | ~16×7 m |
| Staff room | X 16–22, Z 17–23 | ~6×6 m |
| Manager office | X 22–27, Z 17–21 | ~5×4 m |
| Roof access/service side | X 27–30, Z 16–24 | route to roof |
| Parking/exterior | X 3–27, Z -14–0 | ~24×14 m |
| Loading exterior | west/back of Storage | ~8×7 m |
| Roof | service-wing roof | ~15×10 m |

These are greybox targets, not architectural-realism requirements.

## Aisle layout

Use four north/south shopping corridors split by a wide central cross-aisle:

```text
NORTH

       A5     A6     A7     A8
       │      │      │      │
       │      │      │      │

============ CROSS AISLE ============

       │      │      │      │
       │      │      │      │
       A1     A2     A3     A4

SOUTH
```

Target each numbered aisle section at roughly 4–4.5 m long with ~2.0 m walking width.

Central cross-aisle: ~2.2–2.5 m wide.

Shelf target:

- module width ~1.0 m;
- depth ~0.45 m/side;
- height ~1.8 m;
- 5 visible shelves;
- simple/stable collision rather than high-detail mesh collision.

Shelves are static/non-movable in V1.

## Department assignment

| Area | Main product categories |
|---|---|
| Aisle 1 | Cereal / boxed food |
| Aisle 2 | Canned goods |
| Aisle 3 | Small packaged food |
| Aisle 4 | Cleaning / household |
| Aisle 5 | Non-alcoholic drinks |
| Aisle 6 | Mixed boxed/small food |
| Aisle 7 | Employee alcohol shelf + decorative filler |
| Aisle 8 | Toilet paper / bulky household |
| Produce | Watermelons + simple fruit dressing |
| Bakery | Baguettes |
| Dairy wall | Milk/dairy |
| Freezer wall | Frozen boxes |

Customers do not purchase the 12 employee-drinkable alcohol items.

## Produce

Four produce tables, each roughly 1.5×0.9×0.9 m, arranged as two pairs with ~1.6–1.8 m paths.

Watermelons:

- 24 on produce displays;
- 8 produce reserve;
- 8 Storage reserve;
- 40 total.

## Bakery

Counter/shelving only; no baking system.

12 physical baguettes at round start.

## Freezer area

Four main freezer units with **8 interactive doors total**.

Authoritative logical door state:

- Closed ≤10° from closed orientation;
- Neutral 10–35°;
- Open ≥35°.

The freezer area should be visible from parts of the back sales floor but not the entire store.

## Checkout geometry

Three lanes, west→east:

1. Checkout 1
2. Checkout 2
3. Checkout 3
4. Service Desk / PA farther east

Each checkout counter target: ~2.2 m long ×0.8 m wide, with ~1.2–1.4 m customer/passage space.

Each lane should support ~5 preferred queue positions at ~0.9 m spacing. Queue spots guide NPC navigation but do not physically lock customers.

Checkout 2 is roughly centered on the entrance and may naturally receive heavy traffic.

## Entrance

Main automatic entrance approximately 3 m wide when open.

Store closed during Preparation. Automatic customer entrance activates irreversibly at StoreOpened.

The area must be large enough for physical clutter while retaining a meaningful traversability detector.

## Cart distribution

12 total:

- 5 indoor CartStaging;
- 2 exterior cart return;
- 2 parking lot;
- 1 sales floor;
- 2 storage/loading.

One cart has a red handle and is otherwise identical.

## Shopping baskets

10 physical baskets:

- 6 entrance;
- 2 checkout/service area;
- 2 storage.

NPC customers do not use them in V1.

## Service desk / PA

Front-right, highly visible from entrance/checkouts and parts of the sales floor.

The microphone is a physical one-user-at-a-time interaction. It should be accessible enough that other players can physically confront/disrupt the speaker.

## Staff room

~6×6 m.

Required contents:

- 1 staff table (1.6×0.8×0.75 m);
- 4 standard chairs;
- 1 coffee machine;
- 6 reusable coffee cups;
- 1 vending machine;
- lockers/bank;
- 1 trash bin;
- nearby cleaning equipment storage.

Coffee machine placement must allow multiple employees to crowd around it physically.

## Manager office

~5×4 m with a largely glass south/front wall facing the back sales-floor/corridor area.

Contents:

- static manager desk;
- movable rolling office chair;
- movable monitor;
- small office trash bin;
- one unique movable/breakable office plant.

The office must hold 3 players plus multiple quest objects without clipping becoming the main challenge.

## Storage

~16×7 m, large but broken up by static perimeter racks/sight-line blockers.

Maintain a central ~2.5–3 m route for carts/boxes.

Initial content includes:

- reserve products;
- 8 watermelons;
- 2 carts in storage/loading allocation;
- pallets;
- hand trucks;
- large bin;
- cleaning equipment;
- Lost Property area.

Storage should be acoustically/visually private enough that actions there are not obvious from checkouts.

## Loading dock

~8×7 m exterior/service area directly connected to Storage by ~3 m roller door.

At 07:00, 16 current-shift delivery boxes become available in predefined safe placement positions. A static/set-dressing delivery vehicle may appear/be present; no driving simulation.

## Roof

Accessible maintenance roof target ~15×10 m, around 3 m above exterior ground.

Access route must be a wide service stair/ramp path around **1.8–2.0 m** wide and shallow enough for:

- player carrying medium props;
- shopping cart traversal;
- chair/watermelon transport.

Roof should be mostly isolated from indoor sales-floor communication/visibility while visible from some exterior locations.

No climbing system is required.

## Parking/exterior

Small fake parking area:

- ~6 marked spaces;
- 2–3 static cars;
- exterior cart return;
- sidewalk/entrance approach;
- enough space for cart riding/throwing/roof visibility;
- no drivable vehicles.

## Player spawn/staging

Ten player spawn positions around the Staff Room/immediate rear employee area, spaced roughly 0.8–1.0 m apart and oriented generally toward the sales-floor exit.

During the short pre-clock quest-reading sequence, players are together and may look around. At CLOCK IN they can split:

- left → Storage/loading;
- forward → sales floor;
- right → office/roof access;
- remain → coffee/staff room.

## Customer exterior spawns

Use ~5 approach/spawn points near the exterior boundary/sidewalk so customers do not visibly pop into existence at the door.

Customers leave/despawn beyond exterior exit boundaries after shopping/abandonment.

## Pre-opening customers

Three scheduled exterior arrivals around:

- 01:15;
- 01:50;
- 02:20.

They wait outside without patience loss until the store opens.

## Customer circulation

Provide a broad multi-route circulation backbone:

Entrance → front path → aisle ends → central cross-aisle → rear cross-aisle/departments → checkout.

Each aisle should have alternate access where reasonable so one blockage does not necessarily softlock shopping.

Product categories should have multiple physical pickup anchors/subzones to prevent many NPCs targeting one exact point.

## Semantic zone IDs

Authoritative logical IDs must include at least:

- Entrance
- CartStaging
- CheckoutArea
- Checkout1
- Checkout2
- Checkout3
- ServiceDesk
- Produce
- Bakery
- Aisle1 ... Aisle8
- DairyDrinks
- Freezer
- BackCorridor
- Storage
- LoadingDock
- StaffRoom
- ManagerOffice
- Roof
- Parking
- LostProperty

Zones may overlap where logically appropriate. An object may belong to multiple semantic zones simultaneously.

Quest conditions should use generous human-readable interpretations rather than pixel-perfect invisible boundaries. Confirmation/stability windows prevent objects flying through a zone from counting.

## Lost Property

Marked ~2×2 m safe recovery area inside Storage.

Authoritative objects genuinely outside valid world bounds for ~5 sec are moved to safe placements within LostProperty rather than original spawns.

Unique intact objects recover. Destroyed objects do not resurrect.

## Player recovery points

Provide at least:

- default Staff Room/back-corridor recovery;
- exterior/parking recovery;
- roof-landing recovery when appropriate.

Server chooses nearest sensible safe point.

## Fixed unique/equipment props

Initial predictable placement should exist for:

- office plant;
- red-handle cart;
- manager chair;
- PA microphone;
- coffee machine;
- mops/buckets;
- fire extinguishers.

Fire extinguishers: 4, suggested locations service/entrance wall, back corridor, Storage, LoadingDock.

Cleaning equipment: 2 mops + 2 buckets, one set near checkout/service/back area and one near Storage/back corridor.

## Round-start visual condition

At 00:00 the supermarket is organized but incompletely prepared:

- products mostly stocked;
- 5 carts in CartStaging;
- entrance clear;
- all 3 checkouts OFF;
- plant intact;
- zero puddles/broken glass;
- delivery not yet present;
- chairs/equipment in normal locations;
- roof empty except permanent scenery.

By 15:00 the world should often visibly reflect its unique round history.

## Randomization

Major geometry and department locations remain fixed across rounds so spatial memory matters.

Small prop/visual variations may be introduced later only if they do not undermine authored spawn counts, quest reliability, or recognizability.
