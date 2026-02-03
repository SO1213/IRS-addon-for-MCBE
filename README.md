# IRS Building Addon for Minecraft Bedrock Edition

This addon spawns IRS buildings across the Overworld with Tax Collector entities inside!

## 📋 Features

- **IRS Buildings** spawn approximately every 200 blocks in all overworld biomes
- **Tax Collector entities** - Baby zombies in full diamond armor
- **Valuable loot drops** including diamonds, emeralds, gold, iron, and netherite scraps

---

## 🗂️ File Structure Explanation

### Root Files

#### `manifest.json`
The addon's identity card. Contains:
- **format_version**: Tells Minecraft what version format this manifest uses
- **header**: Basic info (name, description, UUID, version)
  - **uuid**: A unique identifier for your addon (never duplicate this across addons!)
  - **min_engine_version**: Minimum Minecraft version required [1.20.0]
- **modules**: What type of addon this is
  - **type: "data"**: This is a behavior pack (not resource pack)
  - Another unique UUID for the module
- **dependencies**: Other addons this requires (none in this case)

---

### `/entities/` Folder

#### `tax_collector.json`
Defines the Tax Collector mob behavior and properties.

**Key Components:**

```json
"identifier": "irs:tax_collector"
```
- Custom namespace `irs:` prevents conflicts with other addons
- The mob's ID used in commands like `/summon irs:tax_collector`

```json
"minecraft:is_baby": {}
```
- Makes the zombie a baby (smaller, faster)

```json
"minecraft:health": { "value": 40, "max": 40 }
```
- 40 HP (20 hearts) - tougher than regular zombies (20 HP)

```json
"minecraft:attack": { "damage": 5 }
```
- Deals 5 damage (2.5 hearts) per hit

```json
"minecraft:behavior.melee_attack"
```
- Enables the mob to attack in close combat
- **speed_multiplier: 1.2** makes it 20% faster when attacking

```json
"minecraft:behavior.nearest_attackable_target"
```
- Defines what the mob attacks (players and villagers)
- **max_dist: 35** - detection range in blocks

```json
"minecraft:loot": { "table": "loot_tables/entities/tax_collector.json" }
```
- Points to the loot table file for drops

```json
"minecraft:equipment": { "table": "loot_tables/entities/tax_collector_gear.json" }
```
- Points to the equipment table for diamond armor

---

### `/loot_tables/entities/` Folder

#### `tax_collector.json`
Defines what items drop when the Tax Collector dies.

**Structure:**
- **pools**: Groups of items that can drop
- **rolls**: How many times to pick from this pool (1 = one item)
- **entries**: The possible items
- **weight**: Probability (higher = more likely)

**Weights Explained:**
- Diamond (weight 30): Most common valuable drop
- Emerald (weight 25): Second most common
- Gold (weight 20)
- Iron (weight 15)
- Netherite Scrap (weight 10): Rarest drop

**Functions:**
```json
"function": "set_count",
"count": { "min": 1, "max": 3 }
```
- Randomly drops between 1-3 of that item

#### `tax_collector_gear.json`
Equips the Tax Collector with diamond armor.

- Each pool gives one piece of diamond armor
- The mob spawns wearing full diamond armor
- **slot_drop_chance: 0.1** means 10% chance armor drops when killed (in the entity file)

---

### `/features/` Folder

#### `irs_building_feature.json`
Defines how the structure is placed in the world.

```json
"minecraft:structure_template_feature"
```
- Uses a structure file (.mcstructure) to place a building

```json
"structure_name": "mystructure:irs_building"
```
- References the .mcstructure file
- **IMPORTANT**: The file must be in `/structures/` and named `IRS_building.mcstructure`

```json
"adjustment_radius": 4
```
- Area around the structure to adjust terrain (smoothing)

```json
"facing_direction": "random"
```
- Structure can face any direction when spawned

**Constraints:**
```json
"block_allowlist": ["minecraft:air"]
```
- Only places structure where there's air (won't replace existing blocks)

```json
"grounded": {}
```
- Structure must be on the ground

```json
"unburied": {}
```
- Structure won't spawn underground

---

### `/feature_rules/` Folder

#### `irs_building_spawn.json`
Controls WHERE and HOW OFTEN structures spawn.

```json
"places_feature": "irs:irs_building_feature"
```
- Links to the feature file we created

**Conditions:**
```json
"placement_pass": "surface_pass"
```
- Spawns during surface generation (not underground)

```json
"minecraft:biome_filter"
```
- Spawns in all overworld biomes
- Uses tag "overworld" to include all surface biomes

**Distribution:**
```json
"iterations": 1
```
- Attempts to place 1 structure per chunk check

```json
"x": { "distribution": "uniform", "extent": [0, 16] }
"z": { "distribution": "uniform", "extent": [0, 16] }
```
- Random X and Z position within a 16x16 chunk

```json
"y": "q.heightmap(v.worldx, v.worldz)"
```
- Places at surface level (uses heightmap)

**SPAWN RATE:**
```json
"scatter_chance": {
  "numerator": 1,
  "denominator": 200
}
```
- **1/200 chance per chunk**
- Chunks are 16x16 blocks
- Approximately one building every 200 blocks

---

### `/spawn_rules/` Folder

#### `tax_collector.json`
Controls natural spawning of Tax Collectors (in addition to structure spawning).

```json
"population_control": "monster"
```
- Counts toward the monster mob cap

```json
"minecraft:brightness_filter": { "min": 0, "max": 7 }
```
- Only spawns in darkness (light level 0-7)

```json
"minecraft:difficulty_filter": { "min": "easy", "max": "hard" }
```
- Won't spawn on Peaceful difficulty

```json
"minecraft:weight": { "default": 5 }
```
- Spawn weight (5 is moderate - zombies are typically 100)

```json
"minecraft:herd": { "min_size": 1, "max_size": 3 }
```
- Can spawn 1-3 Tax Collectors at a time

---

## 🔧 Installation Instructions

### Step 1: Get Your .mcstructure File

You need to create the IRS building structure file:

1. Open Minecraft and create a new world (Creative mode recommended)
2. Build your IRS building
3. Get a Structure Block: `/give @s structure_block`
4. Place the Structure Block at one corner of your building
5. Right-click the Structure Block
6. Set mode to **"Save"**
7. Enter structure name: **IRS_building** (exactly this name!)
8. Adjust the size boxes to encompass your entire building
9. Click **"Save"**
10. Find the file:
   - **Windows**: `C:\Users\[YourName]\AppData\Local\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\minecraftWorlds\[WorldName]\structures\`
   - **Android**: `/storage/emulated/0/games/com.mojang/minecraftWorlds/[WorldName]/structures/`
   - **iOS**: Use a file manager app to access Minecraft files

### Step 2: Add Structure File to Addon

1. Copy `IRS_building.mcstructure` 
2. Paste it into the `/structures/` folder in this addon

### Step 3: Install the Behavior Pack

#### Method 1: Direct Installation (Recommended)
1. Compress the entire `IRS_Addon_BP` folder into a `.zip` file
2. Rename the `.zip` extension to `.mcaddon`
3. Double-click the `.mcaddon` file
4. Minecraft will import it automatically

#### Method 2: Manual Installation
1. Copy the `IRS_Addon_BP` folder
2. Paste it into your Minecraft behavior packs folder:
   - **Windows**: `C:\Users\[YourName]\AppData\Local\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\behavior_packs\`
   - **Android**: `/storage/emulated/0/games/com.mojang/behavior_packs/`
   - **iOS**: Use file manager to access `behavior_packs` folder

### Step 4: Apply to World

1. Create a new world or edit an existing one
2. Go to "Behavior Packs" in world settings
3. Find "IRS Building Addon" in Available Packs
4. Click + to activate it
5. Enable "Experimental Features" if prompted
6. Create/start the world

---

## 🎮 Testing the Addon

### Test Structure Spawning
Explore your world - buildings should appear approximately every 200 blocks.

### Spawn Tax Collectors Manually
Use command: `/summon irs:tax_collector`

### Check Loot Drops
Kill a Tax Collector and see what valuable ores drop!

---

## 🛠️ Customization Tips

### Change Spawn Rate
Edit `/feature_rules/irs_building_spawn.json`:
```json
"denominator": 200  // Higher = less common, Lower = more common
```

### Adjust Loot Drops
Edit `/loot_tables/entities/tax_collector.json`:
- Change weights to adjust drop probabilities
- Modify count min/max for different quantities
- Add new items to the entries

### Modify Mob Stats
Edit `/entities/tax_collector.json`:
- Change "health" value for more/less HP
- Change "damage" in attack component
- Adjust "speed_multiplier" for faster/slower movement

---

## ⚠️ Troubleshooting

**Structure doesn't spawn:**
- Make sure `IRS_building.mcstructure` is in the `/structures/` folder
- Verify the filename is EXACTLY `IRS_building.mcstructure`
- Enable experimental features in world settings

**Tax Collectors don't have diamond armor:**
- Check that `tax_collector_gear.json` is present
- Verify the equipment table reference in the entity file

**No loot drops:**
- Ensure `tax_collector.json` loot table is in the correct folder
- Check that the loot table path in the entity file is correct

---

## 📝 Notes

- This addon uses custom namespace `irs:` to avoid conflicts
- UUIDs in manifest.json must be unique - don't copy them to other addons
- The .mcstructure file is NOT included - you must create it
- Tax Collectors will despawn if you go far away (standard mob behavior)

---

## 🎯 Future Enhancement Ideas

1. Add a Resource Pack to give Tax Collectors a custom texture
2. Create multiple building variants (small, medium, large IRS buildings)
3. Add custom sounds when Tax Collectors attack
4. Include tradeable items or quests
5. Add a boss Tax Collector with better loot

---

## 📄 License

Feel free to modify and share this addon!

---

**Have fun collecting your "taxes"!** 💎⚔️
