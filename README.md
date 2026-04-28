# Factorio AutoSplitter

LiveSplit Auto Splitter (`.asl`) for Factorio (Steam, Space Age).

## Features

- **Automatic timer start** based on `Map.tick` — starts on the first non-zero tick of a fresh save and also when LiveSplit attaches to an already-running save.
- **Game-time synchronized with the engine** — timer reads `tick / 60.0`; freezes during pause / loading screens because the tick itself freezes.
- **Per-milestone splits** for the technologies listed in `technologies.lua` (Nauvis + Space Age planets + Solar System Edge).
- **Out-of-order tolerant splitter** — researching technologies in any order auto-skips intermediate segments (filled with `—`) instead of mis-splitting.
- **Per-milestone toggles** — every milestone has its own checkbox in the LiveSplit settings.
- **No baked-in tech IDs** — the script resolves technology IDs at runtime by scanning `TechnologyMap.entries` and reading `prototype.name`, so it works with any mod loadout without regeneration.
- **AOB-anchored** — the `Map*` pointer chain is found through an AOB signature on `GlobalContext::getGameSafe`, so the script survives ASLR and minor patches.

## Install

1. Open LiveSplit → Edit Splits → set Game Name to `Factorio`.
2. Browse and select `factorio.asl` as the auto-splitter script.
3. (Recommended) Load `factorio_splits.lss` for the matching segment list and icons.
4. Set Compare Against → Game Time (the script forces this on startup anyway).

## Customizing splits

You can freely delete, reorder, or skip any segments in `factorio_splits.lss` — the splitter does not care about order or completeness. **However, segment names must be preserved verbatim.** The script matches each segment to a technology by its exact name (e.g. `Steam Power`, `Agricultural Science Pack`); renaming a segment breaks the match and the split for that milestone will silently never fire. The trig ID form (e.g. `trig_steam_power`) is also accepted as a segment name if you prefer that style.

## Tested build

`factorio.exe` Steam Space Age —  `2.0.76`. Other builds may work as long as the AOB signatures resolve; if the timer never starts, the binary likely diverged and the memory map needs an update.
