# Phase one catalogue and experiment contract — T01

Status: consolidated Q1–Q29 planning understanding confirmed by the owner on 2026-10-22; evidence gates remain open. Implements the documentation deliverable of
[issue #2](https://github.com/bernajaber/OpenMU/issues/2), under
[specification #1](https://github.com/bernajaber/OpenMU/issues/1).
The local specification is `server-setup-spec.md`. This document neither enables
content nor authorizes runtime, database, simulator or load-test execution.
Balance values below are hypotheses. No experiment has been executed.

## Authority and evidence

- **Confirmed** means a decision already recorded in #1, not implemented behavior.
- **Proposed** means a concrete choice submitted for owner review, not approval.
- **Unmeasured** means evidence is still required. A proposal cannot become
  selected configuration merely because it appears in this document.
- Initializer inspection uses commit `f8af54d37e8849d8687e659c9b091c1e58ee12de`.
  Initializers describe defaults, not the effective running database. T03 (#4)
  must export identifiers, requirements, options and probabilities with hashes.
- The existing glossary's terms apply: trio do proprietário, tempo de mundo,
  full atributos and orçamento de atributos por resets have distinct meanings.

The proposed catalogue is closed by the rules below: an unlisted content family
is excluded pending review. Before T04 (#5), export a versioned, expanded manifest
of item/skill/monster identifiers and access conditions. No guessed IDs or copied
probabilities may become implementation inputs. Catalogue approval and manifest
verification are separate gates.

## Classes, skills and control

Confirmed classes: DW, DK, Elf, MG and DL, with MG/DL available at creation.
Soul Master, Blade Knight and Muse Elf are the applicable second evolutions.
Summoner, Rage Fighter, third evolutions and Master Level are excluded.
Second-evolution quests and their materials must remain obtainable in allowed maps.

Proposed skill allowlist, intersected with actual class/evolution/equipment
requirements (names refer to `Initialization/Skills/SkillNumber.cs`):

| Class | Proposed skill families |
| --- | --- |
| DW / Soul Master | EnergyBall, FireBall, PowerWave, Meteorite, Lightning, Teleport, Ice, Poison, Flame, Twister, EvilSpirit, Hellfire, AquaBeam, Cometfall, Inferno, TeleportAlly, SoulBarrier, Decay, IceStorm, Nova / NovaStart |
| DK / Blade Knight | Weapon skills FallingSlash, Lunge, Uppercut, Cyclone, Slash; Defense, TwistingSlash, RagefulBlow, DeathStab, Impale, SwellLife, Combo after its quest |
| Elf / Muse Elf | TripleShot, Heal, GreaterDefense, GreaterDamage, IceArrow, Penetration; SummonGoblin, SummonStoneGolem, SummonAssassin, SummonEliteYeti, SummonDarkKnight, SummonBali, SummonSoldier |
| MG | Compatible basic DW spells and weapon skills above, TwistingSlash, FireSlash, PowerSlash; actual class compatibility remains mandatory |
| DL | Force, FireBurst, Earthshake through Horse, Summon, IncreaseCriticalDamage, FireScream; Raven commands through the pet system |

Propose excluding unlisted later skills (including MultiShot, FlameStrike,
GiganticStorm, ExpansionofWizardry), master upgrades and siege-only skills.
Do not infer availability from an enum member alone. Scrolls, orbs, weapon skill
flags, quests and bot auto-learning must use the same resolved allowlist.

One human-controlled character at a time; the other two use existing Helper.
No new follow/buff automation. Every trial records which character is manual,
Helper settings, switching policy, Elf support uptime and interruptions. Bots use
only demonstrated AI capabilities; missing support behavior is a limitation,
not a simulated perfect buff. DL pet damage, experience, durability and costs are
recorded separately.

## Maps, access and encounters

Proposed ordinary-map allowlist:

| Map | Number | Access contract |
| --- | --- | --- |
| Lorencia | 0 | Creation/return for applicable classes; town protection proposal below |
| Dungeon, all internal floors | 1 | Existing entrances and level requirements |
| Devias | 2 | Existing gates/warps and requirements |
| Noria | 3 | Elf creation/return; town protection proposal below |
| Lost Tower, all internal floors | 4 | Existing gates/warps and requirements |
| Atlans, all internal areas | 7 | Existing access and equipment restrictions |
| Tarkan, all internal areas | 8 | Existing gates/warps and requirements |
| Icarus | 10 | Validate flight/pet restrictions from effective gates; no bypass for DL |

The map numbers identify the proposed manifest; they are not an ordinal range.
Arena, Kalima 1–7, Land of Trials, Valley of Loren/siege, Aida, Crywolf,
Kanturu, Raklion, Swamp, Karutan, Barracks/Refuge and all other unlisted maps
are proposed excluded. Box of Kundun +3 does **not** authorize Kalima or boss
Kundun. DL flight access with Horse and no cape needs verification in T03;
if unavailable, report the blocked route rather than silently add a cape.

Proposed event exceptions: non-master BC 1–7, DS 1–6 and CC 1–6, subject to
actual level eligibility. BC8, DS7 and CC7 are excluded with Master Level.
DS instances can share a map number: identify event tier/instance as well as map.
BC/DS each hourly and interleaved by about 30 minutes is confirmed. Proposed
schedule: BC on the hour, DS at :30; CC hourly at :15. Timezone, registration,
capacity, entry costs and duration need owner review and T03 extraction.

Proposed encounter roster:

| Encounter | Role / access | Proposed reward ceiling |
| --- | --- | --- |
| Gorgon in Dungeon; Balrog in Lost Tower | Evolution milestones; ordinary-map route | Allowed equipment and jewels |
| Hydra in Atlans | Evolution milestone; no MR gear prerequisite | Allowed equipment and jewels |
| Golden Goblin / Golden Titan / Golden Dragon | Invasions in allowed maps; review spawn mapping | Box +1/+2/+3 respectively, mapping proposed |
| Golden Lizard King / Golden Tantalos | Excluded initially; avoid default +4/+5 sources | None |
| Dark Phoenix in Icarus | Difficult reference encounter, trio + two bots | Allowed equipment, jewels, at most Box +3 |
| BC objective / DS waves / CC opponents | Event success criteria below | Phase-filtered completion loot |
| Kundun, Balgass, Selupan, Erohim, Nightmare, Maya and unlisted bosses | Excluded | None |

The first two rows use ordinary equipment/jewel rewards within the same ceiling.
Spawn schedules, summon adds, reward tables and precise monster IDs remain
unmeasured; T03 resolves them and T18 (#19) validates the proposed difficulty
roles. Naming Dark Phoenix a difficult boss does not prove its current behavior
requires cooperation. No boss may require its own drop or MR gear to reach first MR.

For every permitted map/event, inspect walking gates, warp menu, chat commands,
NPC movement, invitations, teleport/summon, respawn, reconnect, saved position,
GM test fixtures and bot routing. A denied destination must not become reachable
through a different entry. Proposed protected zones are existing safe areas in
Lorencia/Noria/Devias plus creation/respawn points; exact polygons and any wider
beginner protection require review. Preserve current death/PK/loot rules and
measure their actual losses before proposing changes.

## Equipment, recipes and every source of rewards

Confirmed: maximum +13, first-generation wings, Box of Kundun +1/+2/+3,
Raven/Horse with real requirements/costs; no Ancient options or Harmony.

Proposed finite base-item catalogue: the union of equipment in the Season Six
`Items/BoxOfLuck.cs` +1/+2/+3 pools, filtered to the permitted classes, plus
first-generation wings and the consumables/material families below. This is a
base-type ceiling, not a promise that every type drops Excellent everywhere.
Apply the filter to normal drops too. In that file the boxes are item (14,11)
with internal levels 8/9/10; their labels +1/+2/+3 are not item upgrades.
Exclude levels 11/12 (+4/+5) from generation and opening.

The +3 pool includes Great Scepter (2,10), Adamantine armor (set 26), Storm
Crow (set 15), Guardian (set 14), Legendary (set 3), Dragon (set 1), and Plate
(set 9). This provides DL/MG candidates without adding later sets. Filter Sacred
Glove, sticks and class-exclusive sets for excluded classes. A set named
“Ancient” in a base-item pool is not itself an Ancient set-option flag: identify
both separately, and exclude incompatible class gear regardless of name.

| Family / source | Proposed disposition |
| --- | --- |
| Weapons, shields, armor, rings, pendants | Filtered union above; class/slot restrictions apply; no blanket permission for later normal items |
| Wings | First generation only where class-compatible; MG compatibility verified; no second/third wings, small rental wings or DL cape by inference |
| DL cape | Proposed excluded; Horse is allowed. Owner must review access/consequences before manifest approval |
| Pets | Raven/Horse confirmed; propose ordinary Angel, Imp, Uniria and Dinorant; exclude Fenrir, premium/time-limited pets and transformations |
| Jewels/materials | Propose Bless, Soul, Life, Chaos, Creation, Guardian only if needed by an allowed recipe; Raven/Horse spirits, Chaos weapon/wing and event-entry ingredients |
| Consumables | Propose ordinary HP/mana/AG potions, arrows/bolts and town scrolls; exclude premium EXP/drop boosters and siege-only consumables |
| Sockets / seeds / spheres | Proposed excluded, including empty-socket bases and socket crafting |
| Item-level-380 system | Proposed excluded: bases, special options, recipes and materials dedicated to it |
| Ancient / Harmony | Confirmed excluded, including set-option generation, refining and removal/addition services |
| Other advanced sources | Propose excluding lucky sets, pentagrams, seasonal gifts, Gens prizes, cash shop grants, premium rentals and unlisted boxes |
| NPC shops | Only allowed low-tier basic equipment, ammunition, consumables and approved skill/entry materials; no Excellent shortcut |
| Crafting | Propose normal upgrades through +13, ordinary additional options, Chaos weapons, wings1, Dinorant, Raven/Horse and BC/DS entries; prohibit other outputs |
| Quests / BC / DS / CC / invasions | Filter both fixed and random rewards and intermediate quest items; BC quest weapon is temporary objective, not tradable advanced equipment |
| MR direct shop | Chosen eligible piece +0, no Luck, two chosen compatible Excellent options; no wings/pets/consumables unless separately reviewed |
| MR box | Same eligible equipment pool, any permitted class, exactly two random compatible Excellent options, chance of Luck; propose +0 output |
| Market / bot creation / automatic equipment | Same catalogue, actual ownership and costs; no free upgrades, wings, class evolution or generated market stock |

Luck remains obtainable only through drops/boxes, without an add-Luck service.
Propose ordinary jewels never add Luck/Excellent flags. Direct MR delivery cannot
be rerolled through a repair/upgrade service. Compatible Excellent pairs must be
expanded by item type from actual option definitions; never put armor-only options
on weapons or assume wing/pet options are ordinary Excellent options. Proposed
DD/Zen armor pairs are economic/defensive variants, not universal best gear.
Proposed ordinary-item policy: normal drops have zero Excellent options;
Excellent drops have one or two distinct compatible options, with no third option.
Use a uniform choice among compatible pairs conditional on drawing two options
as an initial experimental hypothesis; measure and report the changed distribution
against the extracted baseline. The chance of Excellent, second option and Luck
remains a scenario input, not a selected rate. Propose ordinary additional options
up to +16 attack/wizardry/defense where those increments exist, and the matching
legal native cap for recovery/wing options; never reinterpret percentages as raw
points. MR items obey the same additional-option ceiling. Luck and Excellent
rolls must use their actual conditional dependencies. Upgrade failure/destruction,
pet resurrection and recipe costs remain scenario inputs requiring extraction. Filter
recipe inputs/outputs, NPC inventory, quest rewards, nested boxes, bot upgrades
and market imports; filtering only the initial drop is insufficient.

## Reference loadouts

These are proposed fixture recipes, not measured performance tiers. Before a
trial, expand each into exact item IDs, upgrade, durability, additional options,
Excellent pairs, Luck, skills, stat allocation/caps, pet level and supply budget.
Reject a fixture whose requirements are not met; do not grant hidden stats.

| Actor | Basic candidate | Functional candidate | Good / optimized base candidates |
| --- | --- | --- | --- |
| DL | Leather-compatible pieces, Battle Scepter; verify compatibility | Light Plate, Master Scepter; Raven/Horse when earned | Adamantine, Great Scepter, Raven/Horse |
| MG physical | Compatible Leather, Short Sword | Scale without helm, Double Blade | Storm Crow, Lightning Sword, offhand empty |
| MG magical | Compatible Leather, Skull Staff | Scale without helm, Staff of Resurrection | Storm Crow, Legendary Staff; class compatibility verified |
| Elf agility/support | Vine, Short Bow, arrows | Wind, Crossbow | Guardian, Aquagold Crossbow; support skills with real energy requirements |
| Bot A: DK / Blade Knight | Leather, Short Sword | Brass, Double Blade | Dragon, Lightning Sword, class-valid skills |
| Bot B: DW / Soul Master | Pad, Skull Staff | Sphinx, Staff of Resurrection | Legendary, Legendary Staff, class-valid spells |

Proposed tier modifiers: **basic** +0 normal/no Luck/no Excellent, supplies for
entry farm; **functional** +7 normal with legal ordinary additional options,
first-generation wings where legal and affordable pets; **good** +9 with two
compatible Excellent options on eligible equipment, no assumed Luck;
**optimized** +13 with legal chosen options and Luck only when its acquisition
route permits it. These upgrade bands and bot classes are hypotheses for review.
Use every class-compatible armor slot of the named set; MG has no helmet.
Unless listed otherwise, offhand, jewelry and wing slots start empty. Functional
and higher use Wings of Satan for physical MG/DK, Wings of Heaven for magical
MG/DW and Wings of Elf for Elf, subject to verified compatibility; DL has no cape.
DL functional and higher include both Raven and Horse at their minimum legal pet
level for the encounter; keep that level fixed across equipment comparisons.
All fixtures start at maximum durability and carry the same measured supply
budget; ammunition is included for Elf. Functional ordinary additional options
are zero; good/optimized use the proposed cap. Good armor uses damage reduction
and maximum-life Excellent options; physical weapons use Excellent damage rate
and attack speed, magical weapons use Excellent damage rate and wizardry increase.
If the actual option table rejects a pair, mark the fixture invalid and review it;
do not silently substitute an option. Wings/pets receive no Excellent pair.
Optimized adds Luck only on compatible drop/box-sourced pieces; MR-direct pieces
remain without Luck. Primary attack choices are FireBurst (DL), FireSlash
(physical MG), EvilSpirit (magical MG/DW), TripleShot (Elf), TwistingSlash (DK),
with Elf Heal/GreaterDefense/GreaterDamage and DL IncreaseCriticalDamage logged
under the existing control limitations.

For each fixture derive requirement minima per attribute as the maximum of the
exported requirements of its equipped items, skills and pets at the specified
upgrade. Meet level/evolution/command restrictions independently; allocate those
minima first. Proposed residual-point shares (hypotheses): DL strength/command
50/50, physical MG strength/agility 50/50, magical MG energy 100%, Elf agility/
energy 90/10, DK strength/agility 50/50, DW energy 100%. Integer remainders rotate
in listed order; capped shares spill to uncapped selections, then remain unspent.
If the stage budget cannot meet minima, the fixture is invalid at that stage,
not a reason to grant extra points. The 90/10 Elf split is an experimental
proposal, not a reinterpretation of the owner's qualitative agility preference.
Apply this recipe with the same declared budget/caps to reproduce allocations;
exact exported numeric requirements are evidence to attach, not new tuning choices.

A good fixture may be built in a controlled combat test; it must never be injected
into the zero-to-first-MR journey or treated as a naturally obtained drop rate.

Basic must sustain entry farm; functional must sustain paid progression; good
must enable consistent prepared difficult-boss attempts; optimized must offer
measurable improvement without eliminating consequences of mistakes. Test these
claims rather than defining tiers by success after seeing results. Swap physical
and magical MG in otherwise matched trials; redistribution costs remain real.
Keep attributes identical for basic/good comparisons, subject to valid equipment
requirements. Separately test progression at stage-appropriate attributes.

## Experiment conventions and measurements

Proposed population convention: 15/50/150/300 **total active characters**, including
the trio, hence 12/47/147/297 bots. The earlier 15-bot pilot is distinct: 18 total.
Do not mix these series. Count connected characters with observable actions,
not created accounts; report connected, acting, idle, dead and disconnected
counts each minute. Record activity type and completed farm/event/trade actions;
waiting for an event is not equivalent to generating farm output.

- **Zero:** fresh characters, zero resets/MR, equivalent basic equipment and
  documented initial resources; no inherited market stock or advanced fixtures.
- **First MR:** trajectory from zero to eligibility and voluntary execution;
  report time waiting voluntarily separately. No MR-shop prerequisite.
- **Post-MR:** begin immediately after actual MR: level 1, city of class, zero
  cycle resets/power, retained legal gear/resources/skills/evolution/preferences.
  Unusable retained gear must not grant power. Observe return to sustainable farm.
- **Two thirds:** first reset count reaching full atributos divided by required resets/MR;
  test near 2/3 using nearest feasible integer and report rounding error. Never
  infer two thirds of elapsed time or automatically trigger MR at full atributos.

Each cell records scenario revision, effective configuration hash, catalogue and
loadout versions, world seed, population, AI profiles, stage, class/build,
manual/Helper mode, hardware, schedule and start/end times. Compare slower,
intermediate and faster scenarios from the same initial conditions and matched
seeds where useful; vary documented XP/monster/respawn/cost/drop parameters, with
no chosen production values. Independent worlds are the replication unit for
market/AI feedback; individual kills in one world are not independent careers.

| Question | Measurement and proposed gate |
| --- | --- |
| Full atributos after MR | Hypothesis: median functional-character farm reaches full atributos in 48–72 hours of tempo de mundo at 24/7 availability; report p10/p50/p90 by class and downtime, not only pooled median |
| Reset fraction | Hypothesis: full atributos near 2/3 of required resets; propose tolerance ± one reset, separately from timing |
| Active advantage | Hypothesis: 2–3x useful equipment and jewels per invested hour; measure separately by item class/slot and jewel type, never one Zen-equivalent score |
| Difficult boss | Hypothesis: 180–300 seconds first engagement to kill, trio + two good bots; report successful durations and all failures/timeouts |
| Victory | Kill plus surviving eligible participant and legitimate loot resolution; propose at least 90% prepared wins with confidence interval; compare trio alone and basic vs good at equal attributes |
| Mistakes | Predeclare missed support, interrupted attention and depleted potion trials; record deaths/failure and recovery, without inventing perfect Helper support |
| Event result | BC: objective completed within rules; DS: survival/completion and rewards; CC: last-survivor outcome. Record failure, contested loot, eligibility and timeouts |
| Net cost | Per account/character and trio: opening + sources − sinks = closing stocks; trades reported as transfers; count potions, repairs, warps, entries, resets, upgrades and death/PK losses |
| Useful item | Legal and equippable, improving a predeclared slot/build objective over its baseline; separate first improvement, functional set and optimized target. Sale value alone is not combat utility |
| Recovery | Record death-to-resumed-farm and MR-to-self-funded-farm distributions; propose p90 within 30 minutes for death and 2 hours for MR, subject to owner review |

Active/passive comparisons use the same class, stage, loadout, population and
accounting unit. The primary comparison is trio-hour versus trio-hour; also report
character-hours and the passive solo benchmark. Include travel, queues, waiting,
failed attempts and consumables. Report gross rewards and net stocks separately;
never subtract Zen from jewels or equipment counts. Zero passive yield makes a
ratio undefined, not automatically successful. XP is reported independently.
Repeated upgrades/rolls are not independent when prerequisites or options correlate.

Analytical loot probabilities precede Monte Carlo when available. Follow #1's
minimum 300 independent search runs/cell and 1,000 validation runs/cell; increase
for precision. Proposed precision: 95% intervals, win-rate interval half-width
at most 5 percentage points, median reward/time interval relative half-width at
most 10%. Report rare-item exact tails; no observed rare drop is not zero chance.
Calibrate damage, hit rate, cadence, movement, buffs, pet uptime, healing, AI and
consumption against isolated runtime measurements before interpreting careers.
Timeouts and unfinished careers remain censored observations, never discarded.

## Continuous observation, recovery and selection gates

All durations and operational thresholds here are proposals, not measurements.
After future authorization, use a dedicated database/ports/storage and immutable
baseline backup. Never start from or restore over the current server database.

Propose 7 uninterrupted days at each 15/50/150/300-total stage, extending until
post-MR recovery and at least one complete cycle are observed. A restart/fix
starts a new stability window; preserve failed-window evidence. Run planned
recovery drills separately. Record CPU, RSS, GC, queues, persistence errors,
latency, connected/acting characters, spot occupation, stock, prices and liquidity.

Proposed technical gates: zero crashes, unhandled exceptions, duplicated/lost
items, negative balances or point-accounting violations; CPU p95 below 80% of
allocated capacity; RSS below 80% of its budget with no unexplained growth over
10% between equal-load daily windows; action latency p95 below 200 ms and p99
below 500 ms; no persistently growing work queue. Hardware budgets and measurement
boundaries must be fixed before running. Economic gates: every stock change
reconciles to sources/sinks/transfers, no counterfeit inventory, no class blocked
from basic farm/first MR, and prediction intervals compared with actual progression.
Report inflation, concentration, failed listings and scarcity; price stability
alone is not proof of balance and sustained stock growth needs explanation.

Proposed recovery drills: disconnect/reconnect, clean restart, crash during
reset/MR/purchase, backup restoration into another isolated database. Require
exactly-once committed transactions, no duplicated payment or delivery, retained
autoattribute preference and reconciled ownership/balances. Propose recovery time
at most 30 minutes and backup recovery point at most 15 minutes; acknowledge
rollback to that point explicitly. These are operational hypotheses, not guarantees
of zero loss of all activity since the backup. Verify backup hash and restore log.

Technical failure blocks scale. Statistical uncertainty means inconclusive,
not pass. A balance hypothesis outside its band is reported as failed even if
the owner prefers that scenario; changing targets requires a new contract version.
The owner plays zero/first-MR/post-MR and a boss/event/market session, then records
whether grind, equipment impact, support burden and community behavior are
acceptable. No automatic promotion follows numerical success.

## Owner decision log — review round Q1–Q6

The owner accepted the six recommendations in chat on 2026-10-22. These are
planning decisions only; no runtime change or experiment execution is authorized.
This log supersedes conflicting proposal wording above. Unreviewed details and
all balance numbers remain hypotheses; acceptance is not validation.

- **Q1 / D01:** accept Lorencia, Noria, Devias, Dungeon, Lost Tower, Atlans,
  Tarkan and Icarus, with internal areas and non-master BC/DS/CC exceptions.
  Other maps remain excluded; Box +3 does not grant access to boss Kundun.
- **Q2 / D01:** accept the proposed skill list as the candidate for technical
  verification. DL cape remains open until Icarus access is checked; references
  above to excluding the cape are not an approved exclusion.
- **Q3 / D02:** accept the class-compatible Box +1/+2/+3 base-type ceiling across
  all sources, with allowed progression materials, wings1 and Raven/Horse.
  Sockets, item380, later wings, Fenrir and premium sources remain excluded;
  Ancient and Harmony exclusions stand. MR box +0 remains a starting hypothesis.
  Other proposed pets/option policies still need explicit review.
- **Q4 / D04:** use DK and DW as the initial two reference bots; other world
  compositions remain possible. One manually controlled human character at a
  time, others on existing Helper; no new automation.
- **Q5 / D05:** 15/50/150/300 means total active characters including the trio:
  12/47/147/297 bots. A 15-bot pilot is a separate 18-character case.
- **Q6 / D07:** bots reserve consumables, next-reset costs and planned improvement
  resources before offering surplus. Protect cities and creation/return points,
  with an interval against repeated attacks on the same victim. Reserve amounts,
  price bounds, interval duration and exact zone boundaries remain open.

## Open decisions and hand-off

| ID | Review needed / evidence | Responsible follow-up |
| --- | --- | --- |
| D01 | Approve map/event tiers, skill list, protected zones and DL cape/flight proposal | Owner; T03 extraction, T04 access manifest |
| D02 | Approve finite item/recipe families, pet additions, source filters and MR box +0; resolve any missing class-compatible loadout | Owner; T03, T04, T08/T09 |
| D03 | Select resets/MR, caps, points, reset return level/insufficient-Zen behavior, costs, output weights/Luck/option caps | Owner after T05–T09 and T19 evidence; all values remain hypotheses |
| D04 | Approve bot classes, tier modifiers, control policy and useful-item predicates per slot/build | Owner; T10, T17, T18 |
| D05 | Approve total-active convention versus 15 bots and operational/statistical thresholds, observation/recovery durations | Owner; T02, T19–T23 |
| D06 | Validate boss roles, schedules, reward mapping and no circular equipment requirement | T03, T15, T18; owner reviews roster |
| D07 | Define market reserves/price limits, PvP cooldown and exact protection polygons | T11–T16; owner reviews behavioral choices |

T01's six acceptance criteria map respectively to catalogue sections, source
restrictions, loadouts/control, measurement/operations, population/stages and
hypotheses/open decisions. The deliverable is ready for review; catalogue approval,
manifest extraction, experimental execution and hypothesis validation remain
separate unfinished gates. No runtime readiness or successful test is claimed.

## Owner decision log — review round Q7–Q12

The owner accepted the recommendations in chat on 2026-10-22, retaining all
numeric tuning as hypotheses and the DL cape as open. This log supersedes earlier
conflicting proposals; it authorizes documentation only.

- **Q7 / D03:** plan automatic reset at level 380 to level 1 on the same spot,
  retaining valid build and adding only the budget difference. Without enough
  Zen, preserve state, continue permitted activity and retry when affordable;
  no partial charge or continuous message spam. Numeric settings remain hypotheses.
- **Q8 / D02/D04:** use basic +0 normal, functional +7 normal, good +9/two
  Excellent options and optimized +13 as experimental reference tiers. They are
  not free equipment grants or proven performance categories. Equal-attribute
  combat comparisons are distinct from earned-equipment progression journeys.
- **Q9 / D02/D03:** propose one or two compatible options on Excellent drops,
  with two as the phase ceiling. Direct MR remains chosen pair/no Luck; MR box
  random pair/chance of Luck. Jewels/services cannot add Luck or Excellent
  flags. Rates, weights and ordinary additional-option caps remain experimental;
  uniform pair selection is not selected configuration.
- **Q10 / D06:** Gorgon/Balrog/Hydra are proposed evolution milestones; Dark
  Phoenix is the difficult-encounter candidate pending combat validation.
  First MR/evolution bosses cannot require MR-shop gear or their own rewards.
  The 3–5-minute hypothesis applies only to the difficult encounter.
- **Q11 / D05:** primary active/passive comparison uses trio-hour versus
  trio-hour under matched conditions, with character-hour and solo-passive
  secondary reporting. Jewels and useful equipment remain separate measures;
  include waiting, travel, failures and consumption. The 2–3x target is a hypothesis.
- **Q12 / D07:** bot reserves reflect consumables, next reset and one priority
  improvement rather than fixed percentages of everything. Reference prices
  have bounds and gradual stock/sales adjustments; unsold offers may be repriced
  or withdrawn, never fulfilled by artificial buyers. Amounts/bounds/rates remain
  hypotheses.

## Icarus access — code evidence, not runtime validation

Read-only inspection found that the Season Six defaults let Dark Horse satisfy
Icarus flight access without Cape of Lord. `Version095d/Maps/Icarus.cs` requires
`CanFly >= 1`; `VersionSeasonSix/Items/Pets.cs` grants that attribute through Horse.
`Updates/AddDarkHorseCanFlyPlugIn.cs` supplies the existing-database update.
`WarpAction` and `WarpGateAction` check map requirements, not cape identity.
Paths above are relative to `src/Persistence/Initialization/`, except the actions
under `src/GameLogic/PlayerActions/`. Existing tests: `DarkHorseCanFlyTest.cs`
under `tests/MUnique.OpenMU.Persistence.Initialization.Tests/` (not executed).
The effective database and client have not been tested; retain the cape decision
as open until verification in a separately authorized isolated environment.

## Owner decision log — review round Q13–Q18

Accepted in chat on 2026-10-22 as planning recommendations. All numeric tuning
remains hypothetical; DL cape stays open until isolated validation. No execution
or current-server change is authorized.

- **Q13 / D02:** allow ordinary Angel, Imp, Uniria and Dinorant in addition to
  Raven/Horse, with real allowed sources, requirements, consumption and replacement
  costs. No free bot grants; Fenrir/premium pets remain excluded.
- **Q14 / D03:** post-MR recovery uses retained basic gear or purchases funded by
  retained resources, subject to requirements. Bots prepare recovery before MR.
  No automatic free kit. Two hours to sustainable farm remains a hypothesis.
- **Q15 / D04:** classify useful items separately by combat improvement, sustainable
  farm or concrete economic need. A sale is not automatically an equipment
  improvement. Predeclare the objective of each comparison.
- **Q16 / D05:** retain seven continuous days per population stage as an initial
  observation hypothesis; extend until a complete MR cycle and recovery are
  observed. Preserve failure evidence; relevant fixes start a new stability window.
  Reassess protocol cost and evidence needs before execution.
- **Q17 / D05/D06:** prepared group win rate of at least 90% and encounter duration
  3–5 minutes remain initial hypotheses, not selected acceptance rates. Compare
  trio alone and basic/good equipment; separately test missing support, inattention
  and insufficient resources. Report uncertainty, deaths, consumption and failures.
- **Q18 / D06:** propose BC at :00, DS at :30 and CC at :15 in America/Sao_Paulo.
  Check measured durations/overlap and adjust CC before selecting a calendar.

## Owner decision log — review round Q19–Q24

Accepted in chat on 2026-10-22 for planning only. All numeric settings remain
hypotheses and the DL cape remains open until isolated validation.

- **Q19 / D07:** protect city safe areas and creation/return points, not entire
  beginner maps. Farm spots remain contestable. Verify safe-area exits cannot
  trap characters under attacks at the boundary; exact boundaries need validation.
- **Q20 / D07:** the same bot cannot evade its victim-specific initiation interval
  through death, map change or reconnection. Retaliation is limited to the ongoing
  confrontation, not permanent pursuit permission. Duration, encounter-end rules
  and protection against alternating attackers remain open.
- **Q21 / D03:** explicitly verify post-reset level-one recovery when equipment or
  skills lose requirements. No hidden temporary power or bypass. If existing
  Helper cannot sustain recovery, report the limitation before proposing new
  automation; automatic gear switching has not been authorized.
- **Q22 / D05:** distinguish normal restart preserving confirmed operations from
  backup restoration returning to its declared recovery point. Test purchase,
  MR currency and market reconciliation separately. Fifteen-minute recovery-point
  and thirty-minute recovery-time targets remain hypotheses; unexpected loss or
  duplication fails the drill.
- **Q23 / D03/D04:** attribute allocations are candidates, not mandatory builds:
  compare DL strength/energy/command, physical/magical MG and Elf agility/support
  tradeoffs. Requirements, redistribution costs and total point budgets apply.
- **Q24 / D05:** apply the same criteria to slower/intermediate/faster scenarios.
  Technical integrity is required; owner play evaluation follows explicit
  hypothesis reports. A preferred scenario can motivate a documented target
  revision, never retroactive reclassification of a failed hypothesis.


## Owner decision log — review round Q25–Q29

Accepted in chat on 2026-10-22. All balance numbers remain hypotheses and the DL
cape stays open until isolated validation. The owner requested consolidation,
without changes to the current server.

- **Q25 / D07:** plan temporary protection against new bot aggression after
  repeated defeats, including alternating attackers. It must not grant immunity
  for initiating attacks. Trigger, duration and termination are hypotheses.
- **Q26 / D02/D03:** extract current effective upgrade risks first, including
  failure, level reduction or destruction where applicable. Changes require an
  explicit comparative proposal; no assumed loss protection or guaranteed success.
- **Q27 / D02:** test +16 attack/wizardry/defense additional-option caps where
  applicable. Other units/types use compatible native limits. This is a hypothesis
  to compare with extraction, distinct from Luck and Excellent option counts.
- **Q28 / D05:** integrity is mandatory; operational performance/recovery limits
  remain hypotheses to adapt to declared isolated hardware before execution.
  Unresolved corruption, duplication or persistence failures block population growth.
- **Q29 / D01–D07:** finish planning review with costs, caps, resets/MR, rates,
  prices, PvP intervals and technical boundaries explicitly pending evidence and
  further proposals. Present consolidated understanding for owner confirmation.
  This does not authorize implementation, extraction from the live database,
  simulation, runtime creation or promotion.

## Consolidated planning outcome — owner confirmed

This section and the Q1–Q29 decision logs supersede earlier proposal wording.
Acceptance of a test candidate is not validation or selection of production values.
No experiments, server changes or database changes were performed in this review.

| Decision | Agreed planning direction | Remaining evidence or selection |
| --- | --- | --- |
| D01 | Eight ordinary maps, non-master BC/DS/CC, permitted classes/second evolutions and candidate skills; safe cities/return points | Effective identifiers, gates, skill compatibility and protection boundaries; DL cape remains open until isolated Horse/Icarus/client verification |
| D02 | Box +1/+2/+3 class-compatible equipment ceiling across all sources; wings1, ordinary pets, phase materials/recipes; exclude advanced systems; Luck only from drops/boxes | Expanded manifest, ordinary option caps/rates/weights, upgrade risks and compatible option pairs; MR box +0 remains hypothesis |
| D03 | Reset on spot with valid point budget, no partial payment when unaffordable; voluntary MR and earned recovery without free kits or requirement bypass | Reset level/tuning hypotheses, costs, caps, resets/MR, recovery feasibility and Helper limitations |
| D04 | Trio with one manual character, existing Helper, DK/DW reference bots, physical/magical MG and Elf support comparisons; distinguish combat/farm/economic utility | Exact source-verified loadouts and requirements; tier upgrades and allocations remain hypotheses |
| D05 | Total-active counting includes trio; matched trio-hour comparison plus solo/per-character reports; integrity required and owner evaluation before selection | Slower/intermediate/faster trials, hardware-specific protocol, observation/recovery and statistical precision validation |
| D06 | Evolution boss candidates Gorgon/Balrog/Hydra, difficult candidate Dark Phoenix; no circular gear or MR-shop prerequisite | Encounter behavior, reward/spawn mapping and agenda validation; time/win-rate targets remain hypotheses |
| D07 | Real stock, need-based reserves, bounded price adjustment, no artificial buyers; protected areas, persistent victim intervals and protection against alternating pursuit | Amounts, price bounds, intervals, retaliation end conditions and exact zone geometry |

Core hypotheses retained: functional post-MR full atributos in 2–3 days of 24/7
farm; full near 2/3 of resets required for MR (not time); 2–3x active useful-item/
jewel yield; difficult encounter in 3–5 minutes. Additional experimental candidates
include reference gear tiers, MR box +0, ordinary additions +16, 90% prepared wins,
seven-day windows, event offsets and operational/recovery thresholds. None is a
measured result or selected current-server configuration. Catalogue ceilings such
as +13, wings1 and Box +3 remain content rules; the review does not silently
reopen previously agreed phase boundaries.

Evidence hand-off: T03 (#4) must resolve effective catalogue, requirements and
baseline risks; T02 (#3) concerns the separately authorized isolated environment
and recovery validation; T04 (#5) later applies verified access/source filters.
T05–T09 and T17–T19 must resolve progression, options, combat and economy evidence;
T11–T16 resolve market/PvP details; T20–T23 concern continuous scale validation.
These are follow-up responsibilities, not execution authorization. Isolated
validation of the DL cape/access and post-reset/MR recovery remain explicit gates.


## Final owner confirmation

The owner explicitly confirmed the consolidated Q1–Q29 understanding in chat on
2026-10-22. The interview is complete for this planning scope. Keep all remaining
questions/evidence gates explicit, all balance numbers hypothetical and the DL
cape undecided until isolated validation. Confirmed catalogue boundaries remain
content decisions, not proof of implemented enforcement.

This confirmation authorizes documenting the agreement only. It does not authorize
implementation, simulators, tests/load runs, runtime creation, live database
inspection, server configuration changes or promotion. No such actions were
performed during this interview. Further work requires the corresponding scope
of authorization; technical verification and owner evaluation remain outstanding.
