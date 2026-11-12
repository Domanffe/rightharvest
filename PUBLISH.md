# Publishing

The artifact is built and checked. Only the submit form is left, and it needs a GitHub sign-in, which
only you can do.

This version answers the review on [#614](https://github.com/horizon-rust/horizon-mods/pull/614): the
crop table gave beetroots a top `age` of 7 while vanilla's block stops at 3, so a ripe beetroot was
never picked. `max_age: 3` now, tests and `PROOF.md` with the real ages, and the beetroot row of
`PROOF.md` was run in the game this time.

## The file

`C:/Users/world/Documents/HermesWorkspace/mods/rightharvest/dist/rightharvest-0.2.1.hnmod` — 3853 bytes
SHA-256 `a0a08b57100ceee2bab1d7b687867408d3526eb0c6da5c928ac4e50f9601d4b4`

It holds `horizon-native.json` (game 26.3, schema 1, hooks 21, pinned to Horizon `fdc87ae` and
horizon-mixins `e8cd0aa`) and `mixins/rightharvest/`.

## The form

https://playhorizon.dev/mods/submit — GitHub sign-in, then:

- **Kind**: Native mod (the dropdown does not default to it)
- **Project id**: `rightharvest` (free text; the existing id is what makes this an update)
- **Version**: `0.2.1`
- **For Minecraft**: `26.3`
- **Name**: RightClickHarvest
- **Summary** (≤160 chars): Right-click a ripe crop to pick it and sow it again.
- **Description** (first paragraph only is kept): Empty hand, right click a ripe wheat, carrot, potato,
  beetroot or nether wart. You get the usual drops, and the crop goes right back into the soil if you
  carry a seed. Growing crops, other blocks and a hand holding an item are left alone.
- **Authors**: Alexander (Domanffe)
- **Licence**: MIT
- **Source**: https://github.com/Domanffe/rightharvest
- **File**: `rightharvest-0.2.1.hnmod`
- **Changelog**: Ripe beetroots are harvested again. The mod gave them a top age of 7, but vanilla's
  beetroots block has only age 0–3, so a ripe beetroot (age 3) was never picked — a bug from 0.1.0
  that 0.2.0 still carried (review on horizon-mods#614). Wheat, carrots, potatoes and nether wart are
  unchanged.
- Public key and signature: leave empty (unsigned).

Once the 0.2.1 submission is open, close #614 and say there that 0.2.1 replaces it: its staged
`rightharvest-0.2.0.hnmod` has the beetroot bug, and the form is the only writer of the staging file,
so the fix cannot be pushed into that PR.

## What was checked

- `cargo test` in the mod's crate: 5 passed (the beetroot expectations are now `3`); `cargo fmt
  --check` clean; `cargo clippy --all-targets` clean.
- The block states, read from the installed 26.3 resources: `beetroots` and `nether_wart` `age=0..3`,
  `wheat`, `carrots`, `potatoes` `age=0..7` — the table now matches them.
- Launcher build of this exact bundle: `native-build --instance horizon --trust-native-code
  --allow-version-mismatch` → `native-fdc87ae-e18bd806a067`, fast profile in 4m 25s; its
  `native-mods.json` lists `rightharvest 0.2.1` and the staged module has `VERSION: "0.2.1"` with
  `max_age: 3`.
- In the game (that build, `horizon-ctl`): Options → Mods shows **RightClickHarvest 0.2.1**;
  `beetroots[age=3]` right-click → `age=0`, the beetroot and its seeds dropped, one seed taken from the
  inventory; `beetroots[age=1]` untouched; `wheat[age=7]` and `carrots[age=7]` still harvest and
  replant. Details in `PROOF.md`.
- Still unrun: potatoes, nether wart, chest, the no-seed and off-hand cases, sneak, spectator, a LAN
  guest, and the log check (`PROOF.md` rows 3, 6, 8, 9, 11–16).
