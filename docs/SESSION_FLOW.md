# Lobby, Matchmaking and Session Flow

## Player-facing modes

V1 exposes two primary entry paths:

- **Quick/Public Shift** — join an available public supermarket lobby.
- **Private Shift** — invite/code/friends lobby.

No ranked queue, map vote, difficulty queue, mode vote, persistent server browser, clan queue, or alternate game mode in V1.

## Public lobby

Design target is 10 players.

- Minimum automatic public start: 6 players (TUNABLE).
- Maximum: 10.
- At ≥6 players, initial start countdown target ~30 sec.
- If lobby reaches 10, countdown may shorten to ~10 sec.
- Exact countdown values are TUNABLE; purpose is to allow filling without making players wait indefinitely.

Public lobby voice/text is global.

## Private lobby

- 2–10 players.
- Invite/join-code path as platform/runtime permits.
- Host may start when desired/ready flow permits.
- Private host may remove participants.
- No gameplay-rule modifier panel in core V1. Everyone plays the same authored Morning Shift rules.

Custom duration, physics, quest count, customer count, etc. are LATER.

## Persistent employee identity in lobby

Players visibly use their persistent generated employee appearance in the lobby/clock-in presentation.

A first-generation employee card may simply show the generated character/name/employee presentation and welcome them. There is no reroll button.

Match-specific accent/badge/number may help recognition but does not replace persistent visual identity.

## Roster lock and loading

Quest generation must occur only after the roster for the upcoming shift is locked.

Sequence:

1. Lobby reaches start condition / private host starts.
2. Participating roster locks.
3. Required world/load begins.
4. Quest generator validates full assignment set against exact roster.
5. All required participants reach pre-round staging.
6. Each player receives own 3 private quests.
7. ~6-sec reading/clock-in period.
8. CLOCK IN → authoritative 00:00.

Do not give fast-loading players a gameplay-time advantage.

## Spawn presentation

All participating employees begin close together in the Staff Room/immediate employee area at safe distinct positions. During pre-clock reading they may look around. At CLOCK IN controls release and players naturally split toward Storage, sales floor, office/roof, or remain around coffee/staff room.

## No gameplay roles

Every participant is simply **EMPLOYEE**.

Do not display Innocent/Impostor/Saboteur/etc.

Everyone simultaneously has shared operational pressure and private motives.

## No meeting/vote/ejection loop

Players cannot remove another player through ordinary gameplay accusation.

There is no emergency meeting, voting screen, jail, ejection, or elimination phase.

If a lobby decides someone is responsible for a problem, they must continue dealing with that employee socially/physically for the shift.

Moderation controls are separate from gameplay fiction.

## Communication transitions

- Lobby: global voice/text.
- Active supermarket: proximity voice/text.
- PA: global while physically used.
- Results: global voice/text.
- Between rounds: global voice/text.

No global gameplay radio channel.

## No ordinary join-in-progress

New participants do not enter an already active shift.

Reasons:

- they missed Preparation/early match information;
- quest conflict/resource assignment was generated without them;
- physical economy/world state is already altered;
- new quest set could become unfair/impossible.

A disconnected existing participant may reclaim their reserved slot under reconnect rules because their quest state already belongs to the match.

## Disconnect/reconnect

Authoritative details in `ROUND_LIFECYCLE.md` / `NETWORKING.md`:

- ~2.5 sec connection-loss declaration;
- held object/PA/cart control released;
- avatar removed from active physics so it does not block world;
- 120-sec reserved slot;
- reconnect restores logical personal progress/status at safe point;
- no held object/seat/ragdoll pose restoration.

If reservation expires, quests become abandoned for the remainder of the shift and are not redistributed.

## AFK public handling

Initial player-facing target:

- ~90 sec without meaningful input → private warning such as **YOU APPEAR TO BE ON AN EXTENDED BREAK**;
- ~150 sec without meaningful input → remove from active public session.

AFK thresholds are TUNABLE and should not trigger on ordinary quiet observation/chat use if the system can robustly detect activity.

Private sessions are more lenient; host can decide manually.

When an active AFK removal occurs:

- held item drops;
- active controls/PA/cart release;
- physical avatar disappears safely;
- private quests are not redistributed.

## Moderation vs legitimate chaos

The following are normal gameplay and **not** automatically griefing:

- stealing/moving quest objects;
- hiding carts;
- opening/closing freezers;
- disabling checkout;
- creating spills;
- temporarily barricading a doorway;
- lying about personal quests;
- interfering with stacks;
- pushing/impacting other players within normal systems;
- using PA obnoxiously;
- ignoring the shared objective;
- hoarding products/watermelons;
- causing customer complaints.

Actual disruptive behavior includes external harassment/abuse, cheating, crash/desync exploits, unrecoverable geometry traps, intentional AFK, and repeated exploitative control denial that prevents one player from participating at all.

### V1 moderation minimum

- personal voice mute;
- personal text hide/mute;
- private-lobby host remove;
- report pathway where supported by the selected account/platform/backend environment.

Public vote-kick is **not a core gameplay feature**. It may be added as release-hardening if public moderation requires it. If added, it must not use gameplay accusations such as “sabotaging supermarket” as reasons because sabotage-like behavior may be legitimate private-objective play.

## Between-round persistence

After results the same lobby remains where possible.

Players may leave; public matchmaking can refill open slots **between** rounds.

Human memory across rounds is desirable:

- “Don't trust him around freezers again.”
- “This time it isn't my quest.”

Do not create a persistent Trustworthiness/Sabotage score. Reputation should remain human social memory.

## Between-round ready flow

Initial public target:

- minimum six connected participants;
- around 60% ready can start a next-round countdown;
- exact countdown/ready threshold is TUNABLE;
- a single non-ready player should not permanently block nine others.

Private host may start according to the private ready/start UI.

Roster becomes final only when the next-round launch begins; quest generation then uses that locked roster.

## Host identity

In public gameplay the technical host should not receive special in-world status/marker.

Private lobby may expose host status because start/remove permissions require it.

Core prototype host loss ends the active match cleanly. Production public robustness requires later host migration or dedicated public servers.

## First-time public entry

Before first public match, onboarding may include:

- short Employee Orientation;
- microphone test/voice mode selection;
- explanation that gameplay voice is proximity-based;
- explanation that PA broadcasts globally;
- explanation that personal tasks are private and the shift also has a shared target.

Do not teach deception/accusation strategies.

## Session success goal

Persistent lobbies are not just convenience. By round 2–3 players should naturally reference prior behavior and carry human suspicion/jokes into the next shift even though quest assignments changed.
