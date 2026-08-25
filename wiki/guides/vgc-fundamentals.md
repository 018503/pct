# VGC Fundamentals

Source: [VGCguide.com](https://www.vgcguide.com) — Introduction section. Authors: Aaron Traylor, Wolfe Glick, Aaron Zheng.

## What Is VGC?

VGC (Video Game Championships) is the official competitive Pokemon format run by The Pokemon Company. It culminates in annual World Championships with three age divisions (Juniors, Seniors, Masters).

Two core skills define competitive play:
1. **Teambuilding** — selecting and customizing six Pokemon and their moves
2. **Battling** — defeating all four opponent Pokemon using your chosen team

## Rules That Never Change

- **Double Battles** — two Pokemon per side on the field at once
- **Bring 6, Pick 4** — you bring six Pokemon but select only four per battle via Team Preview (you see all six of your opponent's Pokemon before choosing)
- **Current Game** — always played on the latest mainline Pokemon game
- **Species Clause** — one Pokemon per species (shared Pokedex numbers count as same species, e.g. cannot use both Rotom-Frost and Rotom-Heat)
- **Mythicals banned** — Mew, Celebi, Jirachi, etc. are never allowed
- **Region-native** — Pokemon must be caught in the current game or have a battle-ready mark

## Format Types

1. **Regional Dex** — only Pokemon from the current region's Pokedex (usually year 1 of a game)
2. **National Dex** — all available Pokemon except the most powerful legendaries (usually year 2)
3. **Restricted / GS Cup** — powerful legendaries (Mewtwo, Groudon, Kyogre, etc.) allowed, typically two per team (usually year 3)

Formats rotate — used to last over a year, now change every few months on the first of the month.

## How Doubles Differs from Singles

### Your Pokemon Help Each Other
**Double targeting** — both your Pokemon attack one opponent — allows aggressive knockouts before the opponent responds. A Pokemon that loses 1v1 can win with partner support.

### Protect Is Everything
Nearly every Pokemon carries Protect. It prevents damage on one Pokemon while the other acts freely, massively reducing opponent options. See the [Battling Guide](battling.md) for in-battle Protect usage.

### Counters Are Weaker
In Singles, one Pokemon can "counter" another by forcing switches. In Doubles, partner interactions make hard counters rare — a partner can protect or support a threatened Pokemon. You may need multiple Pokemon to handle a single threat.

### Setup Is Harder
Stat-boosting is more dangerous because opponents can double-target the setup Pokemon or its partner. Stall tactics effective in Singles are much less effective in Doubles.

### Spread Moves
Some moves hit both opponents (Heat Wave, Eruption). Multi-target moves deal 75% damage. Some moves like Earthquake and Surf hit your partner too.

### Games Are Faster
VGC games average 10–12 turns (10–15 minutes) vs. 50+ turns in Singles. Entry hazards (Stealth Rock, Spikes) are rarely effective because games are shorter with fewer Pokemon.

## Key Doubles-Specific Moves

| Move | Effect |
|---|---|
| **Protect** | Prevents all damage to the user for one turn |
| **Fake Out** | Priority flinch move; only works on user's first turn on field |
| **Follow Me / Rage Powder** | Redirects all attacks to the user, protecting partner |
| **Helping Hand** | Boosts partner's attack power for the turn |
| **Ally Switch** | Swaps left and right Pokemon positions |
| **Wide Guard** | Protects team from spread moves |
| **Icy Wind / Electroweb** | Low damage but drops both opponents' Speed by 1 stage |
| **Tailwind** | Doubles your team's Speed for 4 turns |
| **Trick Room** | Reverses move order for 5 turns |

## The Stats System

### Base Stats
Species-specific values (1–255) per stat. Same for every Pokemon of that species. Has the largest impact on final stats. Snorlax (base 30 Speed) will never outspeed Garchomp (base 102 Speed) under normal conditions.

### IVs (Individual Values)
Fixed at 31 for all stats in Pokemon Champions. Players don't need to breed or Hyper Train for IVs — they're always maxed. Exception: 0 Speed IV for Trick Room Pokemon (to be slowest = move first under TR) is still set manually.

### Stat Points (replaces EVs)
Pokemon Champions replaces the traditional EV system with Stat Points:
- Maximum **32 points per stat**, **66 points total** per Pokemon
- Each point translates to roughly 1 extra stat point before the nature modifier is applied
- Natures still multiply the final stat by 1.1x or 0.9x, and this applies to the points too — so a +SpA nature makes SpA points slightly more efficient than points in a neutral stat
- Highest level of player customization — how you distribute 66 points across six stats defines your Pokemon's role

This is simpler than the old 510/252 EV system but the strategic decisions are the same: invest offensively for power, defensively for survival, or hit specific speed tiers.

### Natures
25 possible natures. Most boost one stat by 10% and reduce another by 10%. Standard practice: boost a key stat, drop an irrelevant one (e.g. Timid on Gengar: +Speed, -Attack when not using physical moves). Changeable via mints.

### STAB (Same-Type Attack Bonus)
1.5x damage when a Pokemon uses a move matching its own type.

### Damage Rolls
Damage has 16 possible values between a min roll and max roll. A move can sometimes KO and sometimes not depending on the roll.

### Stat Boosts and Drops
Multiplicative modifiers. Memory trick: start at 2/2. Boosts add to numerator, drops add to denominator. +3 = 5/2 = 2.5x. -4 = 2/6 = 0.33x. For Accuracy/Evasion: same trick but start at 3/3. Boosts/drops reset when a Pokemon switches out.

## Pokemon Showdown

An unofficial, free, browser-based battle simulator at [play.pokemonshowdown.com](https://play.pokemonshowdown.com). Takes minutes to build a team vs. hours on cartridge. Most VGC players test ideas here first.

Key warnings:
- Teams are stored in local cookie storage — wiping cookies deletes all teams
- Teams don't sync across computers — keep backups
- Ignore the suggested EV spreads in the teambuilder — they are for Singles
- Ladder rating "doesn't mean very much" — use it to test, not as a skill metric

Useful commands: `/weak [Pokemon]` shows weaknesses, `/data [Type]` shows type info.

## Foundational Mindset

### Everything Is Contextual
"Is this Pokemon good?" and "Does this counter that?" cannot be answered without context. Pokemon strength exists relative to what every other player is doing — this is the **metagame**.

### Learn by Playing, Not Memorizing
Knowledge comes through experience. Stats and mechanics become natural over time through battling. Don't try to memorize everything upfront.

### Everyone Is Still Learning
Even World Champions have to relearn everything when formats change. Never fault yourself for not knowing something. Forgetting things during battle is normal — VGC puts heavy strain on working memory.

### You're in Charge
Other people's opinions are additional information to integrate into your own beliefs. There are no completely correct answers — the game is too complex. Use others' advice as input, not commands.

### Usage Stats Are Descriptive, Not Prescriptive
Stats tell you what IS happening, not what you MUST do. You are the ultimate decider of your own opinions and team choices.
