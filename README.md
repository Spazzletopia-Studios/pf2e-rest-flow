# PF2e Rest Flow

Runs a GM-controlled party rest on one shared board. Players queue food,
healing, daily choices, and readiness. Treat Wounds heals at once. Players
never execute PF2e rest.

## Install

**The easy way (Windows):** download the [SpazzMods Installer](https://github.com/Spazzletopia-Studios/spazzmods-installer/releases/latest),
run it, and click Install on PF2e Rest Flow. No account needed.

**Without the installer:** paste this into Foundry's **Install Module →
Manifest URL** box:
`https://github.com/Spazzletopia-Studios/pf2e-rest-flow/releases/latest/download/module.json`

## Using it

- The Party table shows **Character / HP**, **Healing**, **Food**, **Daily
  prep**, and **Ready** together. Click a character's name to open their
  plan; one detail row stays open at a time. **Spells**, **Gear**, and
  **Sheet** open that character's native sheet where you need it.
- The GM opens a preparation board (the campground button in the token scene
  controls, or `game.pf2eRestFlow.open()`), and every connected player sees
  the same live board with their own row.
- Each player queues healing, food, and daily-preparation choices, then marks
  **Ready for GM**. These clicks do not change HP, inventory, spell slots,
  staff charges, or PF2e selectors. **Treat Wounds** is the one exception: the
  healing and the immunity land as soon as the healer rolls.
- The next thing to click glows, and a **Next:** line says it in words: food
  first, then healing, then **Ready for GM**. A healer's hurt patients glow
  too, and the GM's **Start Rest** glows when everyone is ready.
- The GM's **Show Players**, **Start Rest**, and **Cancel Rest** controls stay
  below the scrolling party list. Cancel asks for confirmation, then closes the plan
  with zero actor changes. Until every selected character is ready, the board
  names the exact characters that Start Rest is waiting for.
- **Start Rest** is the one commit point. It applies end-of-night work, calls
  PF2e's native Rest for the Night once for the selected party, then applies
  all queued daily choices.
- Chat gets one separate summary per character. A failed actor or choice is
  named on that character's card while the other characters continue.

---

The rest of the party, on one shared board. A free SpazzMods module by
Spazzletopia Studios for the Pathfinder Second Edition system on Foundry VTT.

The GM opens a preparation board and every connected player sees the same
live plan. Checks can roll while planning, but irreversible character changes
wait — except Treat Wounds, which heals the moment it is rolled. **Start
Rest** applies the plan, calls the system rest once for the party, applies
daily preparation, and posts one result card per character.

## The flow, per row

1. **Heal-up** *(optional)* — a live HP chip and, on the row of every party
   member trained in Medicine, a **Treat Wounds** picker with one button per
   patient (themselves included). Clicking "Fumbus" on Valeros's row rolls
   *Valeros's* Medicine through the real check pipeline and treats *Fumbus*:
   RAW Treat Wounds — success 2d8, critical success 4d8, plus the DC-tier
   bonus (trained DC 15 / expert DC 20 +10 / master DC 30 +30 / legendary
   DC 40 +50 — the attempted tier is a world setting, clamped to each
   healer's proficiency); a critical failure deals the patient 1d8. The
   healing dice roll into chat and the GM's client applies them **at once**:
   the HP, the removal of the Wounded condition on a success, and the
   system's **Treat Wounds immunity** effect for any result (1 hour; 10
   minutes when the healer has Continual Recovery). Start Rest never applies
   it again; the character's card still names it. While any such immunity
   effect is present (ours or PF2e Workbench's), that patient's button is
   disabled *before* any roll, naming the effect. A healer without a usable
   **healer's toolkit** (worn or held; Violet Ray and Marvelous Medicines
   count) is disabled too, with a tooltip saying so. Buttons for hurt
   patients who can still be treated glow. **Finish / skip healing** closes
   the optional step by hand; a success closes it for the patient.

   **Queue heal spells** — when the row's own PC can cast healing, an extra
   button saves a commit-time plan. At Start Rest it picks the LOWEST-HP living hurt party member,
   cast, apply the real rolled healing, re-pick, repeat — stopping the moment
   nobody is hurt or the casts run out. It never casts into a full party, and
   and stops when nobody is hurt or the casts run out.

   Recognized healing spells (each amount comes from the spell's own system
   data at the cast rank, never a hardcoded table): **Heal** (2-action
   variant, rank×d8 + 8×rank), **Soothe** (rank×d10 + 4×rank), **Lively
   Flight** (6d8, +2d8/rank; PF2e 8.x only, not in PF2e 7.12.2), **Shock to
   the System** (8d8, +2d8/rank), and
   the focus spells **Lay on Hands** (6/rank), **Rebuke Death** (3d6,
   +1d6/rank) and **Soothing Mist** (2d8, +1d8/rank). Spend order — *heal
   taking priority*: divine-font Heal → Heal from slots (highest rank first)
   → Soothe → the other slotted heals → focus spells (a spontaneous slot
   always casts the highest-priority spell that entry knows). Deliberately
   excluded: over-time/sustained healing (Hymn of Healing, Vital Beacon,
   Regenerate), conjured-food heals (Cornucopia, Nature's Bounty), self-only
   (Wholeness of Body), multi-target compositions (Soothing Ballad),
   reactions (Breath of Life), and rest-equivalents (Moment of Renewal).
2. **Food** — choose one:
   - **Ration** — selects one meal from the real rations in that inventory.
     Start Rest consumes it
     through the system's own consumable bookkeeping (uses tick down, the
     last use opens the next pack or removes the item), and shows the meals
     left.
   - **Forage** — Subsist with Survival, rolled through the system's check
     pipeline against the GM's forage DC. The yield feeds as many people as
     the rules allow (see the table below); the surplus appears on the
     forager's row and any unfed player can eat from it with one click.
   - **Skip** — go without.
3. **Daily prep** — **Daily Preparation** opens Rest Flow's own workspace for
   that character. It reads the actor's PF2e Rule Elements and shows their
   real daily selectors, such as Ancestral Longevity. It also lists every
   actor-owned feature whose source says it is used during daily preparations,
   opens the familiar, links to gear and prepared spells, and queues one staff.
   Fixed prepared casters can save complete slot presets and queue one for the
   next rest. Saving and queueing change no actor data. Start Rest applies the
   chosen preset after PF2e restores daily resources. Missing spells or changed
   slot maps fail honestly instead of partly applying. Old or malformed presets
   stay visible as **Needs update** so they can be repaired or deleted. Each
   preset has an expandable preview of its spellcasting entries, ranks, slot
   numbers, spell names, and missing saved spells. Empty slots collapse into
   one count, fully empty ranks collapse into one summary, and zero-slot ranks
   stay hidden. Opening a preset closes the prior preview so a long list stays
   compact. A
   prepared caster can
   select one real prepared slot to
   add its rank to the staff's charges at Start Rest; only that slot is marked
   expended, the rank's other prepared spells stay. When PF2e Wand & Staff
   Casting is active and that staff has an Item entry, Rest Flow recharges and
   adjusts that exact item-local pool; casting and daily prep never show two
   different counters. Rest Flow keeps its own standalone fallback, so there is
   no required module dependency.
4. **Ready** — the player marks the queued plan ready. This never runs rest.
   Any changed food, healing, or daily choice clears readiness.

Every queued entry lands in a per-row log. Food, healing, and preparation can
be changed before Start Rest; the trash can on a row's daily choices removes
every queued selector, staff and preset. A Treat Wounds that already healed
stays on the board; the GM cannot undo it there. After Start Rest, the
per-character chat cards are the durable record of what applied and what
failed.

## Foraging: the fed-count table

RAW (Subsist, *Player Core* p. 232 — verified against Archives of Nethys and
the installed system's own rules text):

| Result           | People fed                              |
| ---------------- | --------------------------------------- |
| Critical success | 2 (the forager and one more)            |
| Success          | 1 (the forager)                         |
| Failure          | 0 — RAW, going without leaves you fatigued |
| Critical failure | 0 — and −2 to Subsist for the next week |

A character with the **Forager** feat gets its full RAW benefit: any result
worse than a success becomes a success, a success feeds the forager + 4, and
a critical success feeds the forager + 8.

All of these numbers are **world settings** the GM can tune (fed on success,
fed on critical success, the Forager bonus, and the forage DC itself).

**Failure effects are real** (world setting *"Forage: apply failure effects
to the actor"*, on by default): a failed forage applies the actual pf2e
**Fatigued** condition through the system's own condition API, tagged with a
module flag; a critical failure applies a **"Subsist Setback
(house-applied)"** effect item — the RAW −2 circumstance penalty to Subsist
checks for 1 week (7 game days), predicated on the system's own
`action:subsist` roll option so it hits every future Subsist check
automatically. The board row and log say exactly what got applied. And
because RAW fatigue lasts *"until you get sufficient food and shelter"*:
at **Start Rest**, any PC whose queued food succeeds has the module-applied fatigue
removed automatically (only the one we applied — a GM's own Fatigued is
never touched) and the summary card notes it. Turn the setting off to get
warnings only.

## House rule: Salt (GM can disable)

The pf2e system ships no mundane salt item, so this module does — a **Salt**
pouch (**7 uses, 7 cp** — 1 cp per pinch, deliberately far under the ≈5.7 cp
a bought rations meal costs; system-provided icon). It ships in the module's
compendium pack **Rest Flow Items** (`pf2e-rest-flow.rest-flow-items`), so
shops can stock it and GMs can drag it from the compendium; each row also has
the GM can drag it from the pack or stock it through another module.

**Stable item contract** (other modules key on this — Living Shops stocks
Salt from it): compendium id `prfSaltPouch0001`, slug **`salt`**, flag
**`flags.pf2e-rest-flow.salt: true`**. These will not change.

The conversion is **1:1**: each surplus serving costs **one Salt use** and
becomes **one ration charge**. A click converts min(surplus, salt uses, 7)
servings into one real rations item at Start Rest; the button says exactly what it will do
— e.g. *"Salt → rations (3 of 5, limited by salt)"* — and stays visible
(disabled, with a tooltip pointing the GM at the flask) when the forager has
surplus but no pouch.

This is a Spazzletopia house rule, not RAW. Turn it off with the
**"House rule: Salt preserves surplus forage"** world setting.

## Public API and hooks (stable contract)

- `game.pf2eRestFlow.open()` — open the board (also: the campground button in
  the token scene controls, for players and GMs alike).

Three hooks fire with well-shaped payloads. These are a **stable contract**:
other modules (and the future premium Downtime Suite) may build on them.

```js
Hooks.on("pf2eRestFlow.preRest", (payload) => {});
// { actor, row, sessionId, module: "pf2e-rest-flow" }
// Fires on the GM client before the single native party rest call.

Hooks.on("pf2eRestFlow.postRestActor", (payload) => {});
// { actor, row, sessionId, module, result: { hpBefore, hpAfter, hpGained,
//   focusBefore, focusAfter, slots } }
// Fires on the GM client after that actor's result is diffed.

Hooks.on("pf2eRestFlow.postRestParty", (payload) => {});
// { sessionId, module, summary: { lines, counts }, session }
// Fires on the GM client after all actor commits. Chat uses separate actor cards.
```

Queued state lives in the flags of a module-owned world journal
(`flags.pf2e-rest-flow.session`) as keyed objects written with granular
paths — never arrays. Players' clicks relay through the GM client over the
module's socket; the GM side re-validates every request (ownership, current
board state, and recomputed forage yields), so edited payloads gain nothing.

## Daily-preparation boundary

The daily-preparation workspace is part of Rest Flow. It reads PF2e actor,
item, Rule Element, spell-slot, familiar, and inventory data directly. It does
not import, recommend, call, or detect a third-party daily-preparation module.

## Licensing and attribution

This module references rules from *Pathfinder Player Core* (the Subsist
action), © Paizo Inc., available as Open Game Content under the ORC License
(Library of Congress TX 9-307-067, online at paizo.com/orclicense). This
module uses trademarks and/or copyrights owned by Paizo Inc., used under
Paizo's Community Use Policy and Fan Content Policy. We are expressly
prohibited from charging you to use or access this content. This module is
not published, endorsed, or specifically approved by Paizo.

**Not affiliated with or endorsed by Paizo Inc. or Foundry Virtual Tabletop,
LLC.**

**Zero AI-generated assets.** The module ships no image files at all — icons
are Foundry's bundled Font Awesome glyphs plus icon *paths* that already ship
with the pf2e system (the Salt and created-rations items point at system
icons). No generative-AI art, text, or data is included in the distributed
module.

**Fonts.** Headings use Cinzel, bundled in `fonts/` under the SIL Open Font
License (see `fonts/OFL-Cinzel.txt`). Body text stays in Foundry's own sans.

**Theming.** The board ships the SpazzMods midnight look out of the box and
harmonizes with the Spazzletopia Theme module when it is enabled: every color
reads the theme's `--spz-*` variables first and falls back to the same
midnight values when that module is absent.

## SpazzMods

This free module is a taster for a premium **Downtime Suite** — craft
projects, retraining, a party Downtime Director, and an alchemist workbench —
coming to the [SpazzMods Patreon](https://www.patreon.com/user?u=224896501).

## Compatibility

Foundry VTT v13 and v14 (verified 14) · pf2e system 7.12.2 or newer 7.x on
Foundry v13, 8.x on Foundry v14 (verified 8.5.1).

## Get help

[Get Help](https://github.com/Spazzletopia-Studios/spazzmods-support) — report a bug, get install help, ask a question, or suggest an idea.
