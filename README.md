# Paradise

Paradise (from "Gunner's Paradise") is a set of difficulties meant to be played solo with the classic Minigun and VB Revolver loadout. They are a training diffs for the mechanics of the VB Gunner: lots of stationaries plus an adjustable wave spawner that spawns VB targets. The two current diffs are built on Haz 5x2 Fire and Haz 6x2 Re with my additions on top. The main additions are the extra stationaries and the VB spawner, so for example Paradise 5x2 = Haz 5x2 Fire + Stationaries + VB spawner.

The philosophy: practise your Gunner mechanics without relying on a Driller. That means softer and less grunt pressure, and more VB targets. You can also increase (or decrease) the difficulty as you improve.

Still the play feeling is different than playing Gunner in a team in a high tier diff. Usually there are less VB targets but more chaos while trying to hit them. I couldn't replicate that in solo quite as well as I wanted to. Any suggestions welcome. 


Playtested solo under these conditions:

- Minigun build: 32223 with Burning Hell
- Revolver build: 23321 with Volatile Bullets
- Bosco build: 31133 (I never use rockets; feel free to use them if you want). Bosco is mainly used for mining.
- Biome: Shallow Grotto
- Missions: 200/250 Morkite and 4/6 Eggs. I haven't tried any other mission types.

## Requirements

The host and every joining player need:

- **Custom Difficulty 2, update 18 or newer**
- **MEV**
- **DEA**

## The VB spawner

The inspiration and first implementation came from Dave's Triple.

- **Fires every 40 s.** The interval is the variable `VBMainIntervalSeconds`, which you can set yourself (default 40 s; Dave's Triple's VB spawner fires every 35 s).
- **Each firing selects one of five enemy families:**

  | Family | Weight |
  | --- | ---: |
  | Bombers | 25 |
  | Wardens | 20 |
  | Grabbers | 20 |
  | Menaces | 15 |
  | Bulks | 5 |

  - If Bombers are selected, it spawns Goo, Ice or Fire Bombers with equal probability (1/3 each).
  - If Bulks are selected, it spawns a Bulk or 2x Mini Bulks with equal probability (1/2 each).

- **The effective HP and attack damage** of every enemy spawned by the VB spawner match the values of 4p LX Pro.
- **Starts 20 s into the mission**, then waits its interval before the first firing.
- **While any dwarf is down, the timer pauses** and resumes when everyone is back up.
- **Spawn location:** one location, at a configured distance of 3,000 game units from the player.

### Spawn amounts

There are two budgets for the number of enemies spawned per firing:

| Family | Budget A | Budget B |
| --- | ---: | ---: |
| Bombers | 2 | 3 |
| Wardens | 2 | 3 |
| Grabbers | 2 | 3 |
| Menaces | 2 | 3 |
| Bulk | 1 | 2 |
| Mini Bulks | 2 | 4 |

The amount spawned per firing is fixed: it does not depend on the dwarves' ammo or on deposited nitra.

The spawner uses `VBExtraSpawnProbability`, a float variable `p` in [0, 1]. With probability `1 − p` the spawner uses Budget A; with probability `p` it uses Budget B. The default is `p = 0`, i.e. Budget A only. More experienced players can increase it.

### Anti-spam rules

- **No consecutive family repeats.** For example, if Bombers spawned now, they can't spawn on the next firing. This helps against too much of the same enemy at once.
- **Alive limits.** On the same topic, each family has an upper limit of alive enemies. For example, if the limit for alive Bombers (of any kind) is 5, there are 4 alive and the spawner rolls Bombers again, only one will spawn. If 5 are alive, Bombers are left out of that roll and another family is picked instead; a firing spawns nothing only when every family is blocked (at its limit, or excluded by the no-repeat rule). Only VB targets spawned by the main VB spawner count toward these limits, not the VB targets spawned by the base diff.

  The limits are the variables `VBMaxBomberAlive`, `VBMaxWardenAlive`, `VBMaxMenaceAlive`, `VBMaxGrabberAlive`, `VBMaxBulkAlive` and `VBMaxMiniBulkAlive`. Their defaults are in the table below.

### VB spawner variables

| Variable | Purpose | 5x2 default | 6x2 default |
| --- | --- | --- | --- |
| `VBMainIntervalSeconds` | Time between VB firings | 40 | 40 |
| `VBExtraSpawnProbability` | Probability of Budget B | 0 | 0 |
| `VBMaxBomberAlive` | Bomber limit | Solo 4 / team 5 | Solo 4 / team 5 |
| `VBMaxWardenAlive` | Warden limit | Solo 3 / team 4 | Solo 3 / team 4 |
| `VBMaxMenaceAlive` | Menace limit | Solo 2 / team 4 | Solo 2 / team 4 |
| `VBMaxGrabberAlive` | Grabber limit | Solo 2 / team 4 | Solo 2 / team 4 |
| `VBMaxBulkAlive` | Bulk limit | Solo 2 / team 3 | Solo 2 / team 3 |
| `VBMaxMiniBulkAlive` | Mini Bulk limit | Solo 4 / team 6 | Solo 4 / team 6 |

## Stationaries

- **Stationary difficulty budget:** 1,150–1,250.
- **Stationary pool (13 enemies):** Nukeballer, Spitballer, Nuke Barrager, Barrage Infector, Brood Nexus, Naedocyte Breeder, Drone Breeder, Cave Leech, Fat Cave Leech, Vartok Scalebramble, Korlok Sprout, Rocket Turret, Tower of Babel.
- Nuke Barrager is absent on Refinery and Escort; Tower of Babel is absent on Escort.
- The pool and its tunings were taken from LX Pro and Interstellar Plus and slightly adjusted.
- The effective HP and attack damage of (most of) the stationaries match the values of 4p LX Pro.
- Brood Nexi spawn swarmers in solo and white slashers in 2p+.

## Misc

- **Enemy caps:** 90 / 150 / 210 / 240.
- **Mini-Bulk spawner:** in every announced wave (and every egg ambush on Egg missions), 2 Mini Bulks spawn 20 s into the wave.

### Dwarf HP/regen

HP and regen taken from Haz 6x2 Re:

- 4.35 HP/s regeneration after 6 seconds without damage.
- Natural regeneration up to 30% HP.
- 30% HP on revive.

### Resupply costs

- Paradise 5x2: 40, 40, 60, ...
- Paradise 6x2: 40, 50, 60, ...

### Lighting without a Scout

Flashlight strength, flare strength, player illumination and environmental lighting all use the following multipliers (taken from Haz 6x2 Re):

| Team | 5x2 | 6x2 |
| --- | ---: | ---: |
| Solo, no Scout | 1.5 | 1.5 |
| Duo, no Scout | 1.5 | 1.5 |
| Trio or four players, no Scout | 1.25 | 1.25 |
| At least one Scout | 1 | 1 |
