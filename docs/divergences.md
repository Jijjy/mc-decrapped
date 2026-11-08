# Divergences from vanilla 26.3

Every way this fork differs from Minecraft: Java Edition 26.3 is listed here: what was removed, and
what was changed. If it is not in this file, it should still behave like 26.3.

The target is a game close in spirit to Java Edition 1.0.0: the core of survival, building, caves,
the Nether and the End, without the content added since that does not earn its place. 1.0.0 is a
yardstick, not a specification: some later additions stay, some 1.0.0 behaviour is not restored.

## How to record an entry

Add one row per removal or change, in the same commit that makes it. Group related things
(all copper blocks, all of one mob's items) into one row when they go together.

- **What**: the blocks, items, entities, features, commands or settings, by vanilla id where one exists.
- **Why**: one line.
- **Old worlds**: what a 26.3 save containing it gets instead (`air`, another block, the entity dropped, the item
  converted...). Write `n/a` when nothing in a save refers to it.
- **Also gone**: recipes, loot tables, tags, advancements, structures, worldgen features, sounds, translations,
  creative tabs, commands and parity harnesses removed with it. Deleting a parity harness is allowed only for
  a feature listed here.
- **Commit/PR**: where it happened.

Removing a block, item or entity from a registry shifts the numeric ids that follow it, so the
client no longer speaks the vanilla network protocol for that registry. Note it in **Also gone**
when an entry does that.

## Removed

| What | Why | Old worlds | Also gone | Commit/PR |
|---|---|---|---|---|

## Changed

Opinionated tweaks to behaviour that stays in the game.

| What | Vanilla 26.3 | This fork | Why | Commit/PR |
|---|---|---|---|---|
| Music pools (`minecraft:music.*` sound events) | Every pool mixes C418's Volume Alpha/Beta tracks with later ones (Lena Raine, Kumi Tanioka, Aaron Cherof and others, 1.13 onward), plus the dragon fight's `boss` | Each pool keeps only Volume Alpha (12 overworld tracks) and Volume Beta (menu, creative, Nether, End, credits) tracks. A pool left empty plays another pool: `music.dragon` → `music.end`; deep dark, swamp and underwater → `music.game`. `music.nether.warped_forest` stays empty, as in vanilla. Music discs, other sound events and the `.ogg` files are untouched; `/playsound` of a music event plays the cut pool. The cut list is `CUT_MUSIC` in `crates/horizon-client-render/src/sound_events.rs`. Resource-pack tracks outside that list stay | The 1.0/1.6-era soundtrack | "Music: keep only Volume Alpha and Volume Beta" |

Harness notes: the music pool cut is applied by `SoundRegistry::load_game`; parity probes keep
`SoundRegistry::load` (vanilla pools), so `sound_event`, `music` and `sound_engine` still prove vanilla
resolution and mechanics, and the cut is pinned by `load_cuts_music_newer_than_volume_beta`.
