# PF2e Rest Flow

## Purpose and features

Rest Flow gives a PF2e group one live board to plan the end of a day. Players queue food, healing, daily selectors, prepared-spell presets, staff charges, and readiness. The GM's **Start Rest** commits the plan and calls PF2e's Rest for the Night once per selected character.

Treat Wounds is different: it rolls and applies healing and immunity at once. Other queued choices do not change character data before Start Rest.

## Setup

Foundry VTT 13 with PF2e 7.12.2 or newer 7.x, or Foundry VTT 14 with PF2e 8.x. The manifest sets Foundry minimum 13 and was verified with PF2e 8.5.1 on Foundry 14.

This is a free module. Install it with the [SpazzMods Installer](https://github.com/Spazzletopia-Studios/spazzmods-installer/releases/latest), or use the [public GitHub release](https://github.com/Spazzletopia-Studios/pf2e-rest-flow/releases/latest). Enable **PF2e Rest Flow** in Manage Modules.

## Quick start

1. Open **Rest Flow** from the campground icon in Token Controls, or run `game.pf2eRestFlow.open()`.
2. Each player opens their character row and queues food, healing, and daily choices.
3. Mark **Ready for GM** when that character's plan is complete.
4. The GM reviews each row and fixes or confirms the queued choices.
5. The GM selects **Start Rest** to commit. **Cancel Rest** closes the plan without committing queued changes.

## Detailed use

### Shared board and player choices

The board shows each character's HP, Healing, Food, Daily prep, and Ready state. Click the character's name to open its detail row. Only one row stays open at a time. **Spells**, **Gear**, and **Sheet** open the character's native PF2e sheet. A **Next:** message and a glow mark the next useful choice.

1. **Healing**
   - For Treat Wounds, choose the healer's row and click the patient's button. The healer rolls Medicine through PF2e. Success heals 2d8 and removes Wounded; critical success heals 4d8; critical failure deals 1d8 damage. The selected DC tier adds +10 at expert (DC 20), +30 at master (DC 30), or +50 at legendary (DC 40); trained is DC 15. A critical result also follows the selected tier's normal rules.
   - The GM client applies HP, Wounded removal on success, and PF2e's Treat Wounds immunity at once. Immunity lasts one hour, or ten minutes with Continual Recovery. A current Treat Wounds immunity from this module or PF2e Workbench disables the button before a roll. A usable healer's toolkit is required; worn or held kits, Violet Ray, and Marvelous Medicines qualify.
   - Use **Finish / skip healing** to close the step. A successful treatment closes it for that patient. Canceling the board does not undo healing already applied.
   - To queue spell healing, use the row's healing-spell choice. At Start Rest, Rest Flow picks the lowest-HP living hurt party member, casts and applies a supported spell, then checks again. It stops when no one is hurt or eligible casts are gone. Supported spells: Heal, Soothe, Lively Flight (PF2e 8.x only), Shock to the System, Lay on Hands, Rebuke Death, and Soothing Mist. Spend order is divine-font Heal, highest-rank Heal slot, Soothe, other slotted healing, then focus spells. Rest Flow excludes sustained, conjured-food, self-only, multi-target, reaction, and rest-equivalent healing.
2. **Food**
   - **Ration** selects one real ration meal in the character's inventory. PF2e's consumable use is spent at Start Rest; the board reports how many remain.
   - **Forage** rolls Survival against the GM's forage DC. By default, success feeds the forager; critical success feeds the forager and one other person; failure feeds nobody; critical failure feeds nobody and applies a one-week −2 circumstance penalty to Subsist. The Forager feat follows its benefit: results below success become success, success feeds four additional people, and critical success feeds eight additional people. The GM can tune these amounts. Surplus appears on the forager's row for unfed characters to claim.
   - If Salt is enabled and present, a player can preserve surplus as rations at Start Rest. One Salt use converts one serving into one ration charge.
   - **Skip** queues no food.
3. **Daily preparation**
   - Click **Daily Preparation** on the character row. Rest Flow shows the actor's real PF2e daily selectors, such as Ancestral Longevity, and lists actor-owned features whose descriptions say they are used during daily preparation. Use each selector's choice list, then click **Queue** or **Update queue**. These choices are not applied until Start Rest.
   - Use **Prepared Spells** to open the character's spell entries. Fixed prepared casters can save their current slots: enter a name and click **Save current slots**. Open a saved preset to review its entries, ranks, slot numbers, spells, empty slots, and missing spells. Click **Queue for rest** to queue a valid preset. Queueing changes no spells now; Start Rest applies it after PF2e restores daily resources.
   - A preset marked **Needs update** no longer matches the actor's fixed prepared slots. It cannot be queued until repaired or replaced. Delete it only after clearing it from the rest board; queued presets cannot be deleted. The board's trash control clears queued selectors, staff, and presets.
   - Under **Staves**, choose an optional prepared spell slot and click **Queue staff**. The slot is expended at Start Rest, not while planning. Only the chosen slot is marked expended; other prepared spells at that rank remain. Rest Flow prepares one queued staff.
   - Use the familiar, **Gear & Investment**, **Prepared Spells**, and **Character Sheet** links to review the actor's choices. These links do not queue changes themselves.
4. **Ready**
   - Click **Ready for GM** after reviewing the queued choices. Changing food, healing, or daily preparation clears readiness. This only marks the plan; it does not run PF2e rest.

Each queued choice appears in the row log. The GM can undo queued entries before commit. Treat Wounds already applied is not undone by that action.

### GM review and commit

The GM sees the same live board. **Show Players** controls whether players see it. **Start Rest** is disabled until every selected character is ready; the board names the characters still needed. **Cancel Rest** asks for confirmation and closes the plan with no queued actor changes.

At Start Rest, Rest Flow applies end-of-night work, calls native PF2e rest once for each selected character, then applies the queued selectors, prepared-spell preset, and staff preparation. Each character gets a separate chat summary. A failure for one actor or choice is named; the other characters continue.

Forage failure effects are real by default: failed Subsist applies Fatigued; a critical failure adds **Subsist Setback (house-applied)** for one week. When a character later gets enough food, Start Rest removes only the Fatigued condition this module added. A GM's own Fatigued condition is not removed. Turn off **Forage: apply failure effects to the actor** for warnings only.

### House rule: Salt

The setting **House rule: Salt preserves surplus forage** is on by default. Rest Flow adds a Salt pouch with seven uses at 7 cp. Each use converts one surplus serving into one ration charge at Start Rest. The pouch is in the **Rest Flow Items** compendium pack. Turn this setting off to use rules as written without Salt.

### Optional Wand & Staff Casting integration

When PF2e Wand & Staff Casting is active and a staff has an Item spellcasting entry, Rest Flow prepares and adjusts that staff's own charge pool. Without it, Rest Flow uses its own charge handling. Neither module requires the other.

## Settings

World settings include forage DC (15), people fed on success (1) and critical success (2), Forager bonus (4), applying forage failure effects (on), Treat Wounds DC tier (trained), and the Salt house rule (on). The GM can tune the forage DC and yields to the campaign.

## Limits and recovery

Player requests route through the GM client. It checks ownership, the current board, and forage yields again before writing. Queued food and daily choices do not change the actor until Start Rest. Treat Wounds is the exception and applies at once. Do not cancel or retry a Treat Wounds roll to undo it.

A preset with missing spells or changed slot maps fails rather than applying partly. Keep **Needs update** presets until you replace or delete them. After Start Rest, use each character's chat summary to see what applied or failed before correcting a plan.

The board session is stored in a module-owned world journal. The daily-preparation workspace reads PF2e actor, item, Rule Element, spell-slot, familiar, and inventory data directly; it does not use or detect another daily-preparation module.

## API, hooks, and development

Open the board with `game.pf2eRestFlow.open()`. Stable hooks are `pf2eRestFlow.preRest`, `pf2eRestFlow.postRestActor`, and `pf2eRestFlow.postRestParty`; they run on the GM client and report per-actor and party results.

Run the source gate with `npm test` in `harness/`. Source tests do not replace an installed smoke or a live check.

## Credits and license

MIT License. This module references *Player Core* Subsist rules as Open Game Content under the ORC License (TX 9-307-067). Paizo trademarks and copyrights are used under its Community Use and Fan Content policies. This module is not published, endorsed, or specifically approved by Paizo. It is not affiliated with or endorsed by Paizo or Foundry Virtual Tabletop, LLC.

**Zero AI-generated assets.** No image files ship with the module. Icons use Foundry's bundled Font Awesome glyphs and PF2e system icon paths. No AI-generated art, text, or data is included in the distributed module.

Headings use Cinzel under the SIL Open Font License. The board uses its SpazzMods colors and can read optional Spazzletopia Theme colors.

## Get help

[SpazzMods Support](https://github.com/Spazzletopia-Studios/spazzmods-support).
