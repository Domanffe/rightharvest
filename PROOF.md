# PROOF — manual check in the game

**Status: partly run.** Run on 2026-11-10 against the 0.2.0 bundle built by the launcher on
Nightly `fdc87ae` (`native-build`, `native-fdc87ae-d271a36fda83`); rows 1, 2 (block state only),
5, 7 and 10 are PASS below. The rest are still unrun. Earlier: `horizon-mixins
plan`/`apply`, `cargo test`, `cargo clippy` and `cargo fmt`.

Recording: after each step write what you saw (screenshot or the inventory and
the block state), and mark PASS/FAIL next to it.

## Before start

1. `horizon-mixins plan` → `status: ok`, `injections: 2`, `modules: 1`.
2. `horizon-mixins apply` → `status: ok`; `build/stage/…/native_hooks.rs` contains
   `fn rightharvest_right_click_block` inside `fn native_right_click_block` and `mod native_rightharvest`.
3. Build with the launcher (`native-build … --trust-native-code`) → the build
   finishes; `native/build.log` has no error. A compile error here would mean a
   game API moved — the failure is in `harvest.rs`, not in the game.
4. Play the instance, `Options → Mods` shows **rightharvest**, on.

## A flat, lit test plot

Commands (creative; any flat spot with sky above, at `y=64` ground):

```
/gamemode creative
/tp @s 0 65 2
/fill 0 64 0 4 64 0 minecraft:farmland
/setblock 0 65 0 minecraft:wheat[age=7]
/setblock 1 65 0 minecraft:wheat[age=3]
/setblock 2 65 0 minecraft:carrots[age=7]
/setblock 3 65 0 minecraft:potatoes[age=7]
/setblock 4 65 0 minecraft:beetroots[age=7]
/setblock 0 65 1 minecraft:chest
/give @s minecraft:wheat_seeds 10
/give @s minecraft:carrot 5
/give @s minecraft:potato 5
/give @s minecraft:beetroot_seeds 5
```

Keep the **hand empty** (select an empty hotbar slot; check with the inventory
screen that the off hand is empty too).

## Checks

| # | Do | Expect | Result |
|---|---|---|---|
| 1 | Right-click `0 65 0` (ripe wheat) | Wheat block becomes `wheat[age=0]`; wheat and seeds drop and are picked up; the seed count goes down by exactly 1 against what the harvest dropped (e.g. 10 → 9 + 1–3 picked up) | |
| 2 | Right-click `2 65 0` (ripe carrots) | `carrots[age=0]`, carrots drop, **carrot −1** | |
| 3 | Right-click `3 65 0` (ripe potatoes) | `potatoes[age=0]`, potatoes drop, **potato −1** | |
| 4 | Right-click `4 65 0` (ripe beetroots) | `beetroots[age=0]`, beetroot + seeds drop, **beetroot_seeds −1** plus what dropped | |
| 5 | Right-click `1 65 0` (wheat `age=3`) | Nothing happens: no break, no drop, no seed spent, block still `age=3` | |
| 6 | Right-click `0 65 1` (the chest) | The chest opens as usual (nothing harvested) | |
| 7 | Hold a hoe (or any item), right-click a ripe crop | Vanilla use of the item, crop untouched | |
| 8 | Empty both hands of the seed, right-click a ripe crop | Crop is harvested (drops on the ground), the plot is **empty** — no crash, no seed spent | |
| 9 | Put the seeds in the **off hand only**, right-click a ripe crop | Crop harvested, plot empty (documented limit: it sows from the inventory only) | |
| 10 | Creative, right-click a ripe crop | Crop re-sown, no seed taken from the inventory, no drops (vanilla creative break) | |
| 11 | Sneak + right-click a ripe crop | Harvests (documented: an empty hand has no block use to suppress) | |
| 12 | `/gamemode spectator`, right-click a ripe crop | Nothing happens | |
| 13 | In the nether: `/setblock ~ ~-1 ~ minecraft:soul_sand`, `/setblock ~ ~ ~ minecraft:nether_wart[age=3]`, right-click it | Becomes `nether_wart[age=0]`, nether wart −1 | |
| 14 | `Options → Mods` → turn `rightharvest` off, right-click a ripe crop | Vanilla again: nothing happens on an empty hand | |
| 15 | Second player joins (LAN), right-click a ripe crop as the guest | The guest harvests and re-sows; both clients see the crop reset; the drop is visible to both | |
| 16 | Watch the log (`horizon-launcher logs`) after all of it | No `[rightharvest]` lines unless something failed; no panic | |

## Known-good references

- Item counts before/after come from the inventory screen (or `game_inventory`
  through the game tools).
- Block state after a click: F3 crosshair (`wheat[age=0]`).

## Measured (2026-10-30)

- Built with `horizon-launcher native-build … --trust-native-code`; `horizon-mixins apply` reported
  `status: ok` and the game compiled with the bundle.
- Scene: `setblock 215 69 537 minecraft:farmland[moisture=7]`,
  `setblock 215 70 537 minecraft:wheat[age=7]`, empty hand selected, `give @s minecraft:wheat_seeds 5`,
  then `horizon-ctl interact 215 70 537`.
- Result, read back from the game: block `minecraft:wheat[age=7]` → `minecraft:wheat[age=0]`,
  inventory `wheat_seeds: 5` → `4`. One right click, one seed spent, crop replanted.
- With the hand empty and **no** seeds the same click picks the crop and leaves the soil empty, which
  is the code's documented rule (`sown` stays false).
- Not observed in game: carrot, potato, beetroot and nether wart (covered by unit tests only, all five
  are in `runtime.rs`'s table), a LAN guest's click, and whether the drops land before the replant on
  a slow tick.

## Measured (2026-11-10, 0.2.0 on the named `right_click_block` hook)

Bundle `rightharvest-0.2.0.hnmod` (sha256 `0e81979…`), built by the launcher into
`native-fdc87ae-d271a36fda83` together with the other installed mods (31 injections, 14 modules,
`Finished fast profile in 9m 40s`), then played and driven with `horizon-ctl`. Scene: farmland filled
at `-3..3 / 101 / -3..3`, crops at `y=102`, player at `0 102 1`.

- **Row 1, PASS (survival).** `wheat[age=7]` at `0 102 0`, empty hand: one right click →
  `wheat[age=0]`, `inventory_change: {"minecraft:wheat_seeds": -1}`, and the inventory afterwards held
  `wheat: 1` and `wheat_seeds: 12` — the harvest's own drops (1 wheat, 3 seeds) picked up on top of
  10 − 1 sown.
- **Row 2, PASS (block state, creative).** `carrots[age=7]` at `2 102 0` → `carrots[age=0]`; no seed
  spent, nothing dropped (creative break), so the drop half of that row is still unproven.
- **Row 5, PASS.** `wheat[age=3]` at `1 102 0`: `changed: false`, still `age=3`, no drop, no seed.
- **Row 7, PASS.** Holding 10 wheat seeds, right click the same ripe wheat: `changed: false`, block
  stays `age=7`, seeds untouched.
- **Row 10, PASS.** Creative, empty hand, `wheat[age=7]` → `wheat[age=0]` with `inventory_change: {}`.
- Not run: rows 3, 4, 6, 8, 9, 11–16.
