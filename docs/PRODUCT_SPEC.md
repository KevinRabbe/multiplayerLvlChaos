# Product Specification

## Status vocabulary

- **LOCKED** — core product behavior. Change only through explicit design decision.
- **TUNABLE** — exact starting value is specified but expected to move during playtesting.
- **EXPERIMENTAL** — mechanic/parameter is intentionally subject to validation.

## Product thesis

`multiplayerLvlChaos` is a multiplayer physics/social-comedy game about employees trying to operate a compact supermarket while each has three secret personal responsibilities. Some responsibilities are useful, some absurd, and some quietly conflict with what other employees are trying to do.

The game should create conversations such as:

- “Who did this?”
- “Why are you doing that?”
- “I was fixing it.”
- “It was him.”
- “No it wasn't.”

without using a designated impostor, forced meeting, voting phase, or scripted accusation sequence.

### Core social pillars

1. **Secret motives** — each player receives exactly three private quests.
2. **Ambiguous consequences** — the same mess/problem may have been intentional, accidental, or multi-causal.
3. **Partial observation** — proximity communication and map geometry prevent complete information.
4. **Neutral incident announcements** — the game reports problems, never culprits.
5. **Persistent physical evidence** — moved/broken objects remain where players left them until round end unless genuinely out of bounds.
6. **End reveal** — private motives become public after the shift, recontextualizing visible behavior without providing a forensic action log.

## First scenario: Morning Shift

**LOCKED:** one supermarket map, one primary scenario, 15-minute round.

- 00:00–03:00 Preparation.
- Store may open early when at least 4/5 opening conditions are currently satisfied and a player presses OPEN STORE.
- 03:00 forced opening if not opened earlier.
- 07:00 guaranteed delivery event.
- 13:00 Morning Rush.
- 15:00 immediate authoritative end-state snapshot, then results.

Shared primary objective:

> **Serve at least 20 customers before shift end.**

The shared objective provides pressure but does not force behavior. Ignoring it is allowed; the round continues and results become worse/funnier.

## Player count and session structure

- Design target: **10 players**.
- V1 maximum: **10**.
- Public minimum start: **6** (TUNABLE).
- Private: **2–10**.
- No normal join-in-progress.
- Disconnected active players may reclaim their reserved slot for roughly 120 seconds.
- Same lobby remains together after results where possible.
- Lobby/results voice and text are global; active gameplay communication is proximity-based.

## Player role

Every player is simply an **EMPLOYEE**.

There are no classes, factions, teams, impostors, saboteur roles, traitor roles, voting/ejection mechanics, or match-long elimination in V1.

A later temporary death/KO/respawn mechanic is **undecided/experimental future scope**. Permanent match elimination remains outside the current direction.

## Player controller

Third person.

Core actions:

- walk;
- sprint;
- jump;
- crouch;
- grab/carry/place;
- drop;
- throw;
- precision handling;
- push/pull;
- use;
- sit/ride;
- generic gesture/mischief action;
- stumble/ragdoll/recover.

No stamina, health, fall damage, direct combat, prone, climbing, conventional inventory, independent two-hand control, or crafting.

Player-player collision is enabled but soft. Standing players cannot normally be picked up. Ragdolled players can be dragged until they recover.

## Physical interaction philosophy

**Stable first, funny second, realistic third.**

Objects are physical rather than teleported/welded to the hand. Medium/large objects should lag, collide, and become awkward in tight spaces. Precision placement may apply subtle assistance close to surfaces but should never look like grid snapping.

Small held objects are exclusive to one holder. Medium/large cooperative objects may accept simultaneous constraints from multiple players. A player's held item releases on ragdoll.

Harmless emergent physics strategies are desirable. Reliability failures such as duplication, infinite velocities, state corruption, unrecoverable trapping, or persistent desync are not.

## Signature shopping cart

The shopping cart is a core mechanic, not decoration.

It must support:

- physical pushing;
- worsening steering at speed;
- real loose cargo;
- one rider;
- tipping;
- cargo spilling;
- impacts with players/NPCs/props;
- rider ejection on sufficiently severe tip/impact;
- traversal to the accessible roof.

The rider can exit voluntarily at any time and must never become permanently trapped.

## Supermarket interaction rules

The supermarket does not clean/reset itself during the shift.

Moved products, overturned carts, bottles, puddles, chairs, delivery boxes, doors, and other world state persist until the end of the round unless an object is genuinely invalid/out-of-bounds.

Out-of-bounds objects recover to the in-world **Lost Property** area rather than their original spawn position. Legitimately destroyed objects do not resurrect.

A manual Unstuck option may recover players after a short stationary delay and drops their held object; it is not fast travel.

## Customers

Customers are supporting pressure, not the main attraction.

Each customer:

- approaches/enters the store;
- has a 1–4 category shopping list;
- visits category locations;
- reserves a real eligible product;
- skips unavailable products after a short attempt;
- queues for an operational checkout;
- can become impatient, complain, slip, stumble/ragdoll, abandon, or leave served;
- never remains permanently stuck by design.

Hidden patience starts at 100. Players see broad body-language states rather than a numeric meter.

NPC customers do not use the physical player-interactable baskets in V1 and do not purchase employee alcohol, coffee cups, store equipment, quest props, carts, chairs, tools, or delivery boxes.

### Sale-product lifecycle

1. Product is available in a correct customer-accessible sales zone.
2. Customer reserves it and it becomes non-grabbable during normal shopping.
3. At checkout it becomes physical/interactable again until scanned.
4. Once validly scanned it becomes bagged/reserved and non-grabbable.
5. The authoritative final required scan immediately counts the customer as served, awards Cashier credit to that scanner, records sales, and increments the shared served count.
6. Purchased products leave the physical economy with the customer.

The cosmetic payment/exit delay after the final scan does not delay logical service credit.

## Checkout

Three lanes.

Each supports:

- on/off state;
- conveyor;
- scanner;
- customer queue;
- physical unscanned customer products;
- authoritative final transaction.

No cash/change minigame, receipt handling, manual price entry, payment-method selection, or detailed economics.

## Delivery

At 07:00, 16 delivery boxes become available at the loading dock. The delivery always arrives even if players have cluttered the area.

Delivery boxes are their own physical/event system. V1 does **not** implement opening boxes into additional shelf inventory.

## Player consumable states

The game intentionally has a shallow state model. There is no hunger/thirst/survival simulation.

### Caffeine

Coffee machine in staff room, six reusable physical cups, effectively unlimited coffee supply. Caffeine is 0–100 and persists through the active shift.

Visible effects scale from normal to very alert/twitchy/wide-eyed. Gameplay effects remain mild; no strong control impairment.

### Intoxication

Twelve employee-drinkable alcohol items exist: 8 breakable glass bottles + 4 unbreakable cans. One drink adds 25 intoxication. Controls remain substantially reliable; visible posture/sway and reduced stumble resistance are the main consequences.

### Fullness

Baguettes add fullness. Higher fullness visibly scales the belly and may change idle posture but does not change collider size.

### Mischief

A generic gesture/mischief action exists. While sufficiently drunk it can produce a spit action. Spit creates a small cleanable, non-slippery mess.

No vomiting or bathroom simulation in V1.

## Breakage and mess

Breakable V1 categories are intentionally narrow:

- glass bottles;
- office plant/pot;
- optionally a few small decorative fragile props if explicitly documented.

No generic HP/durability system.

Breaking a valid liquid glass container creates an authoritative puddle plus cosmetic shards. Puddles persist until cleaned/end of shift. Standard puddles are slippery at sufficiently high movement speed; ordinary walking is safe.

Mops clean puddles through cumulative valid contact. Spit mess is cleanable but not slippery.

## Character generation

Every player gets a persistent randomly generated employee identity from a deterministic seed.

The player does **not** choose or reroll the base physical character in V1.

All employees use the same gameplay rig/collider/reach and identical mechanical capabilities. Appearance variation is visual only.

Generation combines constrained proportional variation with modular hair/facial-hair/uniform features. Target population distribution:

- ~60% normal-ish stylized employee;
- ~30% noticeably odd;
- ~10% rare extreme/unfortunate appearance.

Strong extremes should normally be limited to 2–3 traits per character so the rare characters remain readable rather than random noise.

Temporary caffeine/intoxication/fullness/dirt states modify the persistent baseline rather than replacing it.

Customers may reuse compatible human rigs with less extreme generation and civilian clothing.

## Communication and information

### Gameplay voice

Proximity-based and directional. Rough experience:

- 0–2 m full;
- 2–6 m clear;
- 6–10 m audible;
- 10–14 m quiet;
- 14–18 m very faint;
- >18 m effectively inaudible.

Walls/closed doors attenuate and mildly low-pass speech. Open doorways minimally attenuate. Aisle shelving should not behave like concrete acoustic walls.

No global gameplay speaker list, player-location minimap, or through-wall nameplates.

### Text

Gameplay text should follow proximity rules so non-voice players can participate without creating a global information channel. Lobby/results text is global.

### PA

A physical microphone at the service desk globally broadcasts one player's voice/text while actively used. Only one user at a time. Broadcast ends when the user releases interaction, leaves range, or ragdolls.

No mandatory cooldown initially; physical contestability should be tested before restrictions are added.

## Incidents

Corporate incidents observe **actual world conditions** and announce the problem, never responsibility.

Examples:

- missing carts;
- open freezers;
- checkout outage;
- blocked entrance;
- spills;
- delivery backlog;
- office full of merchandise;
- dead plant;
- products on floor;
- blocked aisle;
- product category unavailable;
- complaint spike;
- checkout crowd;
- low cleanliness.

No incident is allowed to reveal culprit identity or hidden quest information. Normal incidents are inactive during preparation except explicit exceptional events such as plant destruction.

## Private quests

Exactly **3 private quests per player** at match start.

Other players cannot see assigned quest IDs/details until results. Players may voluntarily reveal or lie about their objectives; the game provides no proof/show-quest function during the match.

Quest evaluation is deterministic and derives from explicit state/events. No fuzzy intent inference.

Supported semantics:

- event counter;
- current state + confirmation;
- sustained state (resets when broken);
- cumulative state;
- ordered sequence;
- end-of-shift snapshot;
- multi-actor state.

No reroll during the active match. Quest failure has no ranking punishment.

## Results

At exactly 15:00 the server captures an immediate authoritative logical snapshot before cosmetic physics settling.

The snapshot determines end-state quests, cleanliness, unique-object survival/location, and other final-world metrics. Events after the snapshot do not alter scoring.

Team result:

- ≥20 served: **SHIFT SURVIVED**
- <20 served: **MANAGEMENT IS NOT IMPRESSED**

Then show store statistics and reveal each player's three quests and completion states. Display the player's final temporary visual state alongside the reveal.

Do **not** reveal a forensic action timeline or exact incident culprits.

No MVP, worst player, individual ranking, or competitive score.

After results, return to the same lobby with global communication and allow another fully reset shift.

## Tone

The supermarket/corporate system is the straight man reacting dryly to nonsense. Humor should arise from interacting systems and player behavior rather than constant scripted jokes.

Slapstick, mess, drunken visual stupidity, awkward bodies: yes.

Gore, serious injury detail, bodily-horror detail, forced gross-out simulation: no.

Quiet setup periods are allowed. The desired rhythm is **setup → consequence → social reaction**, not nonstop random chaos.
