# DEAD SILENCE 2.5.0 - new monsters, calmer monsters, unseen unstuck teleport

Minecraft 1.20.1, Forge 47.4.10. Every mod below is downloaded by ATLauncher from Modrinth when you import the pack. No third-party pack is included or re-uploaded.

## What I looked at
- Creepy [ Horror ] 2.0 (CurseForge file 9310301, 60 mods) and 1.10 (file 8521324): manifests read. Neither contains mods called "Mimicer" or "Mimic Dweller". Its entity mods are DontCry (the Twixxel-style stalkers), Minesis, Mimicked, The Knocker, The Parallel.
- From the Fog Reimagined 6.3.2 (file 9191040, 139 mods): manifest read. Entity mods in it: Whisperwoods, The End of Herobrine, From The Caves, Nyctophobia (biomes only), Who's There?: Ghost Players, Eyes in the Darkness, The Other Alex, Whispering Spirits, plus mods the pack already has (GoatMan, The Knocker, The Man From The Fog, Midnight Lurker, Better Cave Dweller, The Anomaly, The One Who Watches).

## Newly installed
| Mod | Version | Source | Needs | What it adds / behaviour |
|---|---|---|---|---|
| DontCry | 1.0.4 | Modrinth `dontcry` | GeckoLib 4.8.4 | 19 stalkers. Only 4 kept spawning: Tree Stalker (stares, vanishes), Window Stalker (lurks at windows/doors), Cave Watcher (watches in caves), Cave Stalker. The other 15 have natural spawns cancelled (spawn eggs and /summon still work). |
| Whisperwoods | 1.20.1-2.1.2 | Modrinth `whisperwoods` | Architectury API | Hidebehind (hides behind trees when seen, hunts when you look away), Hirschgeist (rare forest spirit). Moth (breaks torches) and Zotzpyre natural spawns cancelled. |
| Architectury API | 9.2.14 | Modrinth | - | library for Whisperwoods |
| The End of Herobrine | 1.0.8.2 | Modrinth `endofherobrine` | - | Herobrine: Lurker (watches from a distance), Builder, Herobrine. (Herobrine was not actually in the pack before this.) |
| Face of Horror | 3.3 (MIT) | Modrinth | - | faces appear in dark corners, whispers; not a chaser |
| Whispering Spirits | 1.4.0 | Modrinth | - | glowing eyes that vanish when you look at them |

## Looked at and NOT installed
- Minesis, The Parallel, Eyes in the Darkness, The Other Alex: CurseForge only (cannot be downloaded by an ATLauncher Modrinth-format pack).
- The Blood Mimicer / Rebirth: the author says it can open text files, show pop-up messages and show your PC username. Not safe for a pack you share.
- The one who Slashes, Who's There?: Ghost Players: near-duplicates of Better Cave Dweller / our mimics.
- From The Caves: another stalker demon (duplicate role), flashing lights, edits blocks in the world.
- Nyctophobia (biomes only, no mobs), The Graveyard (structure/undead heavy), Horror Elements, Server-Side Horror: no new stalking entity.
- Siren Head and SCP-096 stay excluded. Herobrine, The Boiled One, Revervox, Better Cave Dweller and Dead Silence Mimics are all kept.

## Less aggressive (config/deadsilence_director.json, 36 creatures)
Per creature: `aggression` (chance a new target is accepted), `observeChance` (stand frozen and watch from a distance, then approach or leave), `maxChaseSeconds`, `encounterCooldownSeconds`, `damageScale`, `hitCooldownSeconds`, `maxHitsPerEncounter`, `searchSeconds` (walk to last known position, then give up), `aggressionAtBaseScale` (calmer near beds), `habitat`. Edit the file, then `/mimicai director reload`. Numbers are first-pass guesses and have NOT been play-tested for feel.
Also covered: GoatMan, Cave Dweller, The One Who Watches, Anomaly, Man From The Fog, Shy Guy, The Forgotten Places, Midnight Lurker, Creeping Woods. Not touched: Revervox, The Boiled One, The Knocker.
Mimics (Dead Silence Mimics 1.0.0-ds.3): rare now. First one only after about 12 minutes, a spawn roll every 15 minutes at 35% chance, 5 minute lifetime, 25 minute rest after an attack, talks about 1 in 10, walks slower. All in deadsilence_ai/ai-settings.json (`mimicCheckMinutes`, `mimicSpawnChance`, ...).

## Unstuck teleport (host side, in Dead Silence Mimics)
Works on any creature in the config file, with no patching of third-party jars. A creature with a target that has not moved 1.5 blocks for 8-12 s while farther than 10 blocks from it counts as stuck (standing and watching close up never does). It then needs: nobody (every player, including the second one) able to see it where it stands (160 degree view cone and an unobstructed ray), then a spot 12-30 blocks from its target that is at least 12 blocks from every player, solid footing, no fluid or fire, room for its whole body, not within 40 blocks of any player's bed, same cave/surface habitat, same dimension, and that nobody can see. If nothing fits it does not teleport: it retries every 10 s and after 3 failures drops the target and rests. 60 s cooldown. After a teleport it keeps the target and recomputes the path.
Creatures with their own teleport code (for example DontCry Fossorial) are unchanged.

## Tested (headless server, a dev copy; FakePlayers instead of people)
- Pack server set with all new mods + 36 profiles: starts, 0 errors from the new mods.
- The 9 kept new creatures all summon and survive 30 s with no crash.
- Husk sealed in a stone box with a target 18 blocks away: stuck detected at about 4-5 s (test setting), one teleport to a spot 28 blocks from the target, not in view, not in a wall, then it walked to the target.
- Same husk in a glass box in the player's view: 10 postponements, 0 teleports.
- Aggression 0: target refused. Observe: frozen about 4-5 s, then released and approached.
- Settings migration for mimic calm-down: old file values replaced.
## NOT tested
Real players, Essential, Simple Voice Chat with two people, whether anything feels scary or fair, natural spawn rates of the new creatures, whether the pruned DontCry creatures really stop spawning (only the Forge spawn path is blocked), cave/surface habitat on real terrain, base distance rule with real beds, and any behaviour of the third-party creatures beyond what is above.
