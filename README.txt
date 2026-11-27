AutoPvP — Minecraft Java 1.21.11 — Fabric client mod
====================================================

PROJECT CONTENTS
  build.gradle
  gradle.properties
  settings.gradle
  gradlew / gradlew.bat
  gradle/wrapper/gradle-wrapper.jar
  gradle/wrapper/gradle-wrapper.properties
  src/main/java/dev/autopvp/AutoPvpMod.java
  src/main/resources/fabric.mod.json
  src/main/resources/assets/autopvp/lang/en_us.json
  .github/workflows/build.yml

BUILD (JDK 21 required)
  Windows:  open CMD in this folder and run  gradlew.bat build
  Linux/Mac: ./gradlew build
  Output:    build/libs/autopvp-1.0.0.jar
  Install the non-sources JAR in .minecraft/mods alongside Fabric Loader and Fabric API for 1.21.11.

CONTROLS
  K                         Toggle AutoPvP (changeable in Minecraft Controls)
  /autopvp toggle           Toggle the bot
  /autopvp status           Show enabled state, detected kit, target and current task
  /autopvp profile          Show the detected inventory-based PvP kit
  /autopvp reload           Reload config/autopvp.json
  /autopvp mode retaliate   Only fight players who attacked you (default)
  /autopvp mode all         Fight every eligible player in range
  /autopvp friend add NAME  Never attack this player
  /autopvp friend remove NAME
  /autopvp friend list
  /autopvp help

WHAT IS IN THIS REVISION
  - Preserves the original single-client-class architecture, existing commands, K key,
    friend protection, retaliation/all targeting modes, smooth aim and vanilla reach checks.
  - Adds automatic inventory/equipment kit detection for NethPot, Diamond Pot, NoDebuff/Pot, Debuff,
    UHC, Crystal, Anchor, custom Mole trap kits, Wind-Charge Mace, Pearl-Mace,
    Elytra-Mace, Spear-Mace, Elytra-Spear-Mace, Mace, Spear, Trident, Tank, SMP, Sword, Pearl Fight, Elytra,
    Gapple, Soup, Bow, Crossbow, TNT Minecart and Fireball item kits.
  - Adds /autopvp profile and shows the detected kit in /autopvp status.
  - Adds a retreat-before-healing sequence. It pauses attacks, creates distance from the
    nearest tracked enemy, throws a self splash-heal downward, then eats a golden apple
    when configured/available. Golden apples are prioritized at the default 5-heart
    threshold; at or below five hearts, critical health uses splash healing first when available, then a golden apple when available.
  - Prevents normal combat from running on the same client tick as a task finishes.
  - Changes the new default mace policy: use sword/axe for ground combat and reserve the
    mace for a wind-charge smash. Existing config files without schemaVersion are migrated
    away from the old ground-mace defaults.
  - Adds conservative cobweb placement for detected UHC/Mole kits when a valid target
    feet cell and solid support block exist.
  - Adds conservative End Crystal placement and conditional detonation when the mode is detected,
    the target is above the local player, an obsidian/bedrock base is available, the placement
    volume is clear, a normal visible hit is possible, and a totem or substantial health buffer
    protects against the self-damaging explosion.
  - Adds Spear item detection for material variants (wooden_spear through netherite_spear),
    selects a spear in detected Spear/Spear-Mace profiles, and applies conservative spacing/range logic.
  - Healing thresholds use actual health rather than absorption hearts, so absorption does not
    incorrectly delay an emergency heal.
  - Retains existing water-bucket clutch, totem/shield, buffs, food, Mending XP,
    wind-charge mace and target-priority behavior.

CONFIG
  Location: .minecraft/config/autopvp.json
  New options are written automatically by /autopvp reload.
  Important defaults:
    autoDetectGamemode = true
    healRetreatDistance = 7.0
    appleHealth = 10.0 (5 hearts)
    splashHealHealth = 10.0
    gappleAfterSplash = true
    preferMace = false
    maceWithoutWindCharge = false
    avoidJumpInCrystal = true

LIMITATIONS — READ BEFORE USING
  - Kit detection is heuristic and client-side. Some server kits are custom or share the same
    inventory, so /autopvp profile may report a broad or incorrect profile. It cannot read
    server-side kit names or another player's private inventory.
  - Crystal behavior is intentionally conservative and only covers valid crystal bases near
    elevated targets. Full Crystal PvP placement webs, full Anchor charge/activation, full Mole
    lever traps, Lava/Water bucket denial, full Elytra rocket flight transitions, Pearl Catch,
    spear charge-stage timing, and objective-game pathfinding are NOT fully implemented.
  - Spear's Lunge/charge animation stages are not exposed as a simple stable API signal here;
    this revision recognizes spear variants and uses safe weapon/range behavior but does not
    claim a complete automated Lunge-to-charge combo.
  - There is no full navigation/pathfinding system. It can still fail around complicated
    terrain, holes, doors, blocks and multi-level arenas.
  - A JAR was not rebuilt in this environment: the Gradle wrapper needs to download Gradle
    9.8.2, but network/DNS access was unavailable. The Java syntax check found no parser
    errors, but this is NOT a successful Fabric/Gradle compile and NOT an in-game test.
  - Many multiplayer servers prohibit automated combat. Use only in singleplayer, private
    test worlds or servers where the server owner explicitly allows it.
