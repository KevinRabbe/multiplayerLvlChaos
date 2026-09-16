# Corporate Incident System 1.0

## Purpose

Incidents convert world consequences into shared information without identifying responsibility.

Core rule:

> **Important incidents announce the problem, never the culprit.**

The incident system observes real authoritative world state. It does not check whether a relevant private quest exists and must never fabricate evidence.

## Scheduler rules

- Normal incidents are inactive during Preparation except explicitly exceptional state changes such as office-plant destruction.
- Global ordinary incident cooldown: **45 sec**.
- Same-incident cooldown: **150 sec**.
- Escalation events may bypass same-incident cooldown after at least **~30 sec** if the condition becomes substantially worse.
- Typical match: **2–4** incidents, target average around 3.
- Chaotic upper expectation: about 5, not constant spam.
- Phase announcements such as Delivery and Morning Rush do not consume ordinary incident cooldown.
- Related incident families should coalesce/suppress obvious duplicate symptoms.
- An incident must require meaningful recovery/hysteresis before it can re-arm.
- Banner target duration: ~5 sec.
- No normal permanent incident timeline/log is required during play. Accessibility history, if added, may only repeat the original public message.

## Information policy

Public incident payload may contain:

- incident ID;
- public message;
- location/category only when the definition explicitly includes it.

It must never expose:

- responsible player;
- last interactor;
- hidden quest;
- internal causal attribution;
- forensic event timeline.

The server may retain private developer/statistical causality separately.

## Priority families

Approximate priority order:

1. Critical operational — entrance inaccessible, checkout collapse.
2. Major — delivery backlog, queue surge, large spill.
3. Normal — carts, freezers, stock.
4. Flavor — office warehouse, cleanliness.

Scheduler selects the highest-priority eligible incident when several become eligible simultaneously.

---

# Catalogue

## INC-01 — Cart Shortage

**Trigger:** ≤2 carts in CartStaging continuously for 15 sec while store is open.

**Message:**

> **MANAGEMENT INQUIRY:** We appear to be short on shopping carts.

Re-arm only after meaningful recovery, initially ≥4 staged carts.

## INC-02 — Where Are All The Carts?

**Trigger:** 0 carts in CartStaging continuously for 10 sec while store is open.

**Message:**

> **ASSET ALERT:** Where are ALL the shopping carts?

Acts as escalation from INC-01 when appropriate.

## INC-03 — Freezer Reminder

**Trigger:** ≥3 freezer doors authoritative Open for 20 sec.

Door logic:

- Closed ≤10° from closed orientation.
- Neutral 10–35°.
- Open ≥35°.

**Message:**

> **CORPORATE REMINDER:** Freezers generally work better when closed.

## INC-04 — Register Offline

**Trigger:** any checkout remains non-operational for 10 sec while store is open.

**Message:**

> **CHECKOUT NOTICE:** A register has gone offline.

Do not identify which register in the first-level message.

## INC-05 — Checkout Collapse

**Trigger:** ≤1 operational checkout while ≥4 active customers exist, continuously for 10 sec.

**Message:**

> **CUSTOMER SERVICE:** We are running out of functional checkouts.

Higher priority than INC-04. Related queue symptoms may be temporarily suppressed.

## INC-06 — Entrance Obstruction

**Trigger:** authoritative entrance traversability detector reports severe obstruction continuously for 10 sec.

Severe obstruction means normal customer flow lacks a practical route due to movable props/carts; a few small objects are not enough.

**Message:**

> **SAFETY NOTICE:** The main entrance is becoming theoretical.

## INC-07 — Liquid Problem

**Trigger:** ≥3 active slippery puddles for 10 sec.

**Message:**

> **CLEANUP REQUIRED:** We seem to have developed a liquid problem.

Spit mess does not count.

## INC-08 — Major Spill

**Trigger:** at least 2 standard slippery puddles are created with overlapping/near-overlapping areas within an 8-sec window, producing one obvious clustered contaminated area.

**Message:**

> **CLEANUP REQUIRED:** Major spill reported.

No generic large-liquid-container mechanic is required in V1.

## INC-09 — Delivery Backlog

**Trigger:** at least 10 current-shift delivery boxes remain at/near the loading dock 2 minutes after the 07:00 delivery, then persist for 15 sec.

Cannot fire before approximately 09:00.

**Message:**

> **MANAGEMENT:** Why is the delivery still outside?

## INC-10 — Delivery Scattered

**Trigger:** ≥8 current-shift delivery boxes are outside both LoadingDock and Storage for 20 sec.

**Message:**

> **LOGISTICS UPDATE:** This is not where the delivery goes.

## INC-11 — Office Warehouse

**Trigger:** ≥8 loose sale products are inside ManagerOffice for 10 sec.

**Message:**

> **MANAGEMENT:** Please stop using the office as a warehouse.

## INC-12 — Plant Destroyed

**Trigger:** office plant enters Destroyed state.

**Message:**

> **ASSET UPDATE:** The office plant is no longer with us.

This is allowed during Preparation and should fire immediately subject only to presentation sanity; it is an exceptional state-change incident.

## INC-13 — Merchandise On Floor

**Trigger:** ≥20 qualifying sale products rest on general floor geometry for 20 sec.

A product qualifies only when:

- outside recognized shelf/display/container/checkout areas;
- not customer-reserved;
- supported by general floor geometry;
- stable there for at least 3 sec.

**Message:**

> **HOUSEKEEPING:** Merchandise appears to have migrated to the floor.

## INC-14 — Aisle No Longer An Aisle

**Trigger:** any numbered aisle is severely obstructed for normal customer traversal for 15 sec due to movable objects.

**Message:**

> **CUSTOMER NOTICE:** Aisle [N] is apparently no longer an aisle.

This incident intentionally includes aisle number because otherwise the problem is not actionable.

## INC-15 — Category Unavailable

**Trigger:** a customer-demanded product category has **zero accessible purchasable items** in its correct sales zone continuously for 20 sec.

Accessible means eligible sale product, not customer-reserved/scanned, in the correct sales zone, and practically available for pickup. Items on roof/storage/office do not count as customer stock.

**Message:**

> **STOCK NOTICE:** We appear to be out of [CATEGORY]. Somehow.

## INC-16 — Complaint Spike

**Trigger:** ≥5 valid complaint events in a rolling 60-sec window.

Duplicate-cause cooldowns/per-customer limits must prevent one stuck NPC from farming the incident.

**Message:**

> **CUSTOMER SERVICE:** Complaints are increasing rapidly.

## INC-17 — Checkout Crowd

**Trigger:** ≥6 customers currently assigned to checkout queues across all lanes for 10 sec.

Customers still shopping do not count.

**Message:**

> **ALL AVAILABLE EMPLOYEES TO CHECKOUTS.**

This should be suppressed when a just-announced checkout-collapse incident already describes the same short-lived problem; it may fire independently later if the crowd forms under otherwise operational checkout conditions.

## INC-18 — Dirty Store

**Trigger:** Cleanliness ≤25 continuously for 30 sec.

Initial re-arm recovery threshold: cleanliness >40 before it may become eligible again after cooldown.

**Message:**

> **HEALTH & SAFETY:** Conditions are deteriorating.

---

# Phase announcement separation

The following are scheduled match/phase messages, not incidents:

- STORE OPEN / STORE OPENED UNPREPARED.
- DELIVERY ARRIVED.
- MORNING RUSH.
- 1 MINUTE REMAINING.
- SHIFT END / results transition.

They do not consume ordinary incident cooldown.

## Optional late-shift operational summary

At ~13:00/13:05 the game may show one compact non-culprit status summary such as:

- customers served / target;
- operational checkouts;
- active customers;
- staged carts;
- active slippery spills.

The exact fields are TUNABLE. Never expose hidden quests, culprit data, player locations, or hidden-object locations.

# Incident acceptance standard

An incident is socially valuable when it often causes players to investigate, ask what happened, accuse/defend, temporarily cooperate, use the PA, or discover a physical scene.

Correctness alone is not enough; playtests should record whether each incident generates useful interaction without becoming spam or revealing causality too clearly.
