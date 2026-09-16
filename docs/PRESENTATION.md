# Presentation, Characters, Audio, UI and Onboarding

## Art direction

Target visual identity:

> **Simple, readable, slightly crude on purpose, expressive when things go wrong.**

Do not chase photorealism, high-detail material realism, complex lighting, or dense decorative noise. The supermarket should look like a cheap stylized workplace with clear silhouettes and low-poly/mannequin-like humans.

Visual priority:

1. player readability/recognition;
2. interaction readability;
3. physics readability;
4. clear map landmarks;
5. graphical detail last.

If extra detail makes an object harder to identify from across an aisle, remove the detail.

### Relative asset detail

| Asset | Relative detail target |
|---|---|
| Player employee | Medium |
| Customer | Medium-low |
| Shopping cart | Medium-low |
| Large interaction props | Low-medium |
| Sale products | Very low |
| Static supermarket architecture | Very low |
| Decorative filler | Extremely low |

A cereal box can be a textured cuboid.

## Materials/lighting

- Mostly simple opaque materials.
- Restrained reflections and shader complexity.
- Freezer glass may use simple transparency.
- Floor needs enough contrast/roughness that puddles read clearly.
- Bright fluorescent-style supermarket lighting.
- No horror darkness/day-night cycle.
- Storage may be slightly dimmer, staff room slightly warmer, loading slightly cooler, but all gameplay remains clearly visible.
- Suspicious behavior hides through geography/incomplete information, not darkness.

Environment colors should be relatively commercial/neutral so employees/products remain legible. Employee accent colors should be stronger than the background but not recolor entire bodies neon.

## Employee generation

Each player has one persistent deterministic **CharacterSeed**.

The seed determines baseline:

- head proportions;
- facial feature variants/scales;
- torso/body proportions;
- visual height/proportions;
- posture archetype;
- hair;
- facial hair;
- skin tone;
- uniform variant.

No normal reroll/customization in V1. Restart/rejoin/new round must not regenerate the baseline character.

### Mechanical equality

All generated employees share:

- one gameplay rig/skeleton family;
- same collider/reach;
- same movement/jump;
- same physics/interaction capabilities.

Visual size does not provide mechanical advantage.

### Weirdness distribution

Target approximately:

- 60% normal-ish stylized employees;
- 30% noticeably odd;
- 10% rare extreme/unfortunate.

Do not independently maximize every random trait. Normally cap strong extremes at about 2–3 traits/employee so unusual features retain contrast.

### Baseline proportional ranges

Representative ranges from `TUNING.md`:

- head scale common 0.92–1.10×, rare 0.85–1.20×;
- head width common 0.92–1.08×, rare 0.85–1.15×;
- ear size common 0.90–1.20×, rare 0.75–1.60×;
- nose size common 0.90–1.15×, rare 0.75–1.50×;
- eye size common 0.90–1.15×, rare 0.80–1.30×;
- torso width common 0.92–1.10×, rare 0.85–1.20×;
- limb thickness common 0.90–1.10×, rare 0.80–1.20×.

Limb-length variation remains conservative to preserve hand/object interactions.

Visual overall height may read around 1.65–1.85 m while gameplay collision/reach remains standardized.

### Posture

A small baseline posture pool may include:

- upright;
- slightly slouched;
- forward-leaning;
- chest-out;
- mildly hunched shoulders.

Keep restrained enough that locomotion/carry animations remain compatible.

### Face

Simple stylized face is sufficient:

- eyes/pupils;
- eyebrows;
- simple nose;
- simple mouth;
- optional facial hair.

No realistic facial rig/teeth/tongue/motion-capture requirement.

### Hair/facial hair

Initial hair pool target ~8–12 low-poly variants such as bald, buzz, short messy, side part, bowl, longer messy, ponytail, curls, cheap flat work haircut, etc.

Facial-hair pool may include none, stubble, moustache variants, short beard, simple goatee.

No strand physics.

### Uniform

Players should visibly belong to the same supermarket. Small uniform pool:

- short-sleeve polo;
- long-sleeve polo;
- polo + apron;
- simple work overshirt.

Add trousers, cheap work shoes, name badge, and small match-recognition accent (apron/stripe/badge/sleeve trim). Do not use accent color as the only identifier.

### Name badge

Close-readable badge may contain player name + current lobby/employee number. It is not intended to remain legible at long range.

## Temporary character states

Temporary states modify **relative to the generated baseline**.

### Caffeine

0–24: normal.

25–49 Alert:

- eyes +~5–10%;
- blink rate +~20%;
- slightly more active idle/foot tap.

50–74 Wired:

- eyes +~15–20%;
- faster/irregular blinking;
- mild hand tremor;
- slight forward posture/head twitch.

75–99 Over-caffeinated:

- eyes +~25–30%;
- obvious tremor/tension;
- energetic idle.

100 Extreme:

- eyes ~+35%;
- obvious full-body tension/shake;
- extremely alert expression.

No mandatory movement-speed buff initially.

### Intoxication

0–24 sober.

25–49:

- slight facial redness;
- mildly lowered eyelids;
- relaxed posture.

50–74 Drunk:

- obvious flush;
- loose arms/head lag;
- wider stance;
- idle sway.

75–100 Very Drunk:

- stronger sway/low eyelids/sloppy corrective animation;
- occasional sparse hiccup/burp;
- lower stumble resistance.

Do not introduce randomized steering that removes reliable WASD agency.

### Fullness

Relative belly increase target:

- 0–24 baseline;
- 25–49 +~5%;
- 50–74 +~10–12%;
- 75–99 +~18%;
- 100 +~22–25%.

Collider/reach unchanged. At high fullness, occasional belly-holding idle is acceptable.

### Dirt/wet

Use simple broad material overlays/stains rather than fluid/dirt simulation. A generic dirty/wet lower-body/back overlay is enough if orientation-specific staining becomes expensive.

### State stacking

Caffeine, intoxication, fullness, dirt/wet should combine on the same baseline character. Ragdoll/special-interaction/movement animation has priority over idle-state presentation, while status modifiers layer back in where compatible.

Temporary reaction expressions (e.g. cart impact surprise) may briefly override baseline status face then return.

## Customer appearance

Reuse compatible human/rig technology where practical with civilian clothing and **less extreme** generation. Customer identity is temporary/per-NPC, not persistent.

Players should remain visually easier to recognize due to employee uniform/accent.

## Required animation families

Keep the animation set compact and polish the interactions players repeatedly see.

Must-have families:

- idle/limited posture variants;
- walk;
- run;
- crouch;
- jump/fall;
- small carry;
- medium/large carry;
- push/pull/cart push;
- throw;
- drink;
- eat;
- marked-seat sit;
- PA use;
- mop/clean;
- stumble;
- ragdoll recovery.

Rough total may land around 20–30 clips after directional/transitional variants.

Highest-quality priorities: carry, cart push, drink/eat, stumble, ragdoll recovery.

Use procedural hand/object alignment where necessary to make shared carry classes connect believably; do not author unique animations per cereal box/watermelon/milk carton.

Animation should be slightly awkward/exaggerated rather than elegant. These are employees, not action heroes.

Simple voice-reactive mouth opening is LATER/nice-to-have; no phoneme lip-sync in V1.

## Ragdoll presentation

Visual ragdoll must preserve generated proportions and recover into authored get-up animation. Detailed limbs may be client-local cosmetic; authoritative root/state comes from networking rules.

Voice remains available while ragdolled.

## Audio direction

Audio is critical because players often hear consequences they cannot see.

### Material impact families

Reusable families rather than one sound per object:

- cardboard;
- plastic;
- metal;
- glass;
- soft/produce;
- shopping cart;
- doors/freezers;
- footsteps.

Impact loudness should scale meaningfully: a dropped bottle is not a cart crash. A strong cart crash should be audible several aisles away and prompt “what was that?”

### Checkout

Scanner needs a distinct satisfying simple electronic beep. Repeated scanning should be socially audible.

### Coffee

Button click, short machine hum/pour, cup handling. No elaborate machine simulation.

### Eating/drinking

Short recognizable sounds; coffee ~1 sec, alcohol ~1.5 sec, baguette ~2 sec sequence. Avoid prolonged gross chewing.

### Sparse automatic state sounds

Possible randomized sounds:

- caffeine nervous inhale/teeth chatter/tap;
- drunk hiccup/burp;
- stuffed uncomfortable grunt;
- hard-impact generic effort/yell.

Use long randomized cooldowns roughly 20–45 sec per eligible nonverbal state sound so they do not interfere with voice chat.

### Ambience/music

Subtle refrigerator hum, ventilation, fluorescent buzz, automatic-door/checkout environment.

Gameplay music minimal, bland/cheerful supermarket/elevator style and quiet under communication. A subtle slightly more urgent Morning Rush variation is acceptable; do not compete with speech/physical cues.

### Corporate vs PA cues

Corporate incident: recognizable short **DING**.

Player PA: distinct **DING-DONG** or equivalent.

Players should instantly know whether the store system or another employee is talking.

Full corporate synthesized/voice-acted messages are LATER. Text + chime is sufficient V1.

## HUD

Normal gameplay screen should remain clean.

### Always visible

- match timer/phase at top center;
- compact shared `Customers served: X / 20`.

### Private quests

Collapsed by default, e.g. `Personal Tasks 2/3 [key] View`.

Expanded view shows exact condition/progress. Other players never receive/render this private panel.

End-state quests may show current count/state but do not permanently complete before 15:00.

### Opening checklist

Visible only during Preparation:

- 20 products correctly stocked;
- 5 carts staged;
- Checkout1;
- Checkout2;
- Entrance clear;
- current 0–5 / need 4.

Disappears permanently at StoreOpened.

### Interaction UI

Tiny neutral reticle + subtle object highlight + very short contextual action label such as Grab, Push Cart, Sit, Use PA, Drink, Open.

Avoid glowing the whole supermarket.

### No normal gameplay UI

Do not add:

- minimap/player dots;
- world quest arrows;
- health bars;
- customer patience bars;
- caffeine/intoxication numeric labels over characters;
- management-dashboard meters;
- global speaker list;
- culprit/evidence logs.

Physical world/animation should communicate these states.

## Incident presentation

Short chime + compact banner for ~5 sec. No modal, camera cut, freeze, or teleport.

No permanent normal incident list during the shift.

## Nameplates/speaking

Minimal name within ~8 m and visible/near-direct line of sight. No through-wall nameplates.

Nearby audible speaker may show small speaker icon. Do not reveal globally that someone elsewhere is speaking.

## Customer UI

Virtually none. Customer body language communicates patience.

Checkout may locally show an operational/readability indicator such as remaining customer items. Do not float full shopping lists over NPC heads.

## Private quest feedback

On private completion show brief subtle local feedback with title/check mark. Nobody else sees it.

Definitively impossible quest may privately show failed/unachievable without dramatic punishment.

No arcade `+100`, combo, XP popups.

## Results presentation

At 15:00 freeze final world after logical snapshot/cosmetic settle.

Show:

1. Team result (`SHIFT SURVIVED` / `MANAGEMENT IS NOT IMPRESSED`).
2. Store metrics.
3. Player quest reveals with final-state character.
4. 3–5 selected interesting weird stats/person.

Voice/text global during results.

No MVP, loser, ranking, medals, or forensic culprit timeline.

Full default presentation target ~60–90 sec max and accelerable.

## Onboarding

Do not build a long tutorial level.

First-run Employee Orientation target ~60–90 sec, using reusable supermarket/test assets, skippable later.

Teach only:

1. movement/camera;
2. grab a simple product;
3. place it on a shelf;
4. throw a watermelon/object;
5. push a cart;
6. use a button/coffee machine;
7. open personal-task UI;
8. explain that personal tasks are secret;
9. explain there is also a shared shift objective.

Do **not** teach players to lie, accuse, sabotage, or use PA maliciously.

### Voice onboarding

Before first public match where relevant:

- microphone detected/test;
- Push-to-talk or voice-activation choice;
- explain gameplay voice is proximity-based;
- explain PA broadcasts globally.

Do not suggest how to socially exploit the PA.

### Contextual beginner reminders

May remain stronger for first ~2–3 matches then reduce/suppress obvious repeated prompts.

Preparation itself serves as onboarding by teaching carts/stocking/checkouts without a lecture.

## Accessibility

- Corporate announcements always have text/subtitles.
- Proximity text provides non-microphone communication path.
- Colors never the sole player/state identifier.
- Important states use posture/shape/motion in addition to color.
- Controls remappable.
- Camera sensitivity adjustable.
- Drunk/camera motion effects reducible/disableable without hiding the third-person character's visible state.

Speech-to-text for voice is valuable future work but not required for core V1.
