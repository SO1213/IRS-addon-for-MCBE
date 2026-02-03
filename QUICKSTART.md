# 🚀 Quick Start Guide

## What You Need to Do

### 1️⃣ Create Your IRS Building Structure
- Build a building in Minecraft
- Save it using a Structure Block
- Name it: `IRS_building`
- Find the .mcstructure file in your world folder

### 2️⃣ Add Structure to Addon
- Copy `IRS_building.mcstructure`
- Place in `/structures/` folder of this addon

### 3️⃣ Install Addon
- Zip the `IRS_Addon_BP` folder
- Rename to `.mcaddon`
- Double-click to install

### 4️⃣ Activate in World
- Create/edit world
- Go to Behavior Packs
- Add "IRS Building Addon"
- Enable experimental features
- Play!

---

## 📁 File Overview

```
IRS_Addon_BP/
├── manifest.json                          → Addon identity & info
├── entities/
│   └── tax_collector.json                → Tax collector mob definition
├── loot_tables/
│   └── entities/
│       ├── tax_collector.json            → Valuable ore drops
│       └── tax_collector_gear.json       → Diamond armor equipment
├── features/
│   └── irs_building_feature.json         → How structure is placed
├── feature_rules/
│   └── irs_building_spawn.json           → Where/when structures spawn
├── spawn_rules/
│   └── tax_collector.json                → Natural mob spawning
└── structures/
    └── [PUT IRS_building.mcstructure HERE]
```

---

## 🎮 In-Game Commands

Summon Tax Collector:
```
/summon irs:tax_collector
```

Give yourself items:
```
/give @s structure_block
```

---

## 💡 Key Settings

**Spawn Rate:** Edit `feature_rules/irs_building_spawn.json`
- Line: `"denominator": 200`
- Lower number = more buildings

**Mob Health:** Edit `entities/tax_collector.json`
- Line: `"value": 40`
- Change to desired HP

**Loot Drops:** Edit `loot_tables/entities/tax_collector.json`
- Adjust weights for different drop rates
- Change count for item quantities

---

See README.md for detailed explanations! 📖
