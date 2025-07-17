# Dungeon Quest - Complete Game Documentation

## Table of Contents
1. [Overview](#overview)
2. [Core Game Systems](#core-game-systems)
3. [Player Character](#player-character)
4. [Monster System](#monster-system)
5. [Combat System](#combat-system)
6. [Equipment System](#equipment-system)
7. [Potion & Consumables System](#potion--consumables-system)
8. [Crafting System](#crafting-system)
9. [Quest System](#quest-system)
10. [World & Map System](#world--map-system)
11. [Economy & Shop System](#economy--shop-system)
12. [Banking System](#banking-system)
13. [Progression & Rewards](#progression--rewards)
14. [Special Features](#special-features)
15. [Technical Architecture](#technical-architecture)

---

## Overview

Dungeon Quest is a browser-based dungeon exploration RPG with ASCII art map interface. Players navigate through a procedurally generated world, fighting monsters, collecting equipment, crafting items, and completing quests. The game features complex mechanics for character progression, equipment management, and strategic combat.

### Core Game Loop
1. **Explore** - Navigate the ASCII world map to discover monsters, resources, and NPCs
2. **Combat** - Fight monsters to gain stats, gold, and equipment
3. **Collect** - Gather resources and loot from defeated enemies
4. **Craft** - Create equipment and consumables from collected materials
5. **Trade** - Buy/sell items in town shops and with NPCs
6. **Progress** - Complete quests and enhance equipment for stronger challenges

---

## Core Game Systems

### Game State Management
- **Save/Load System**: Automatic localStorage persistence with corruption recovery
- **Data Validation**: Comprehensive validation prevents corrupted save data
- **Reborn System**: Complete character reset while preserving banking data
- **Banking System**: Persistent gold storage across deaths and reborns

### User Interface
- **ASCII Map Display**: 16x16 grid showing current area with symbols for different entities
- **Real-time Stats Panel**: Shows current HP, ATK, DEF, LUCK, CRAFT stats with temporary effects
- **Activity Log**: Scrolling combat and event messages
- **Modal System**: Overlays for inventory, crafting, shops, and quest management

---

## Player Character

### Base Stats
- **HP (Health Points)**: Starting 20, increased by leveling and equipment
- **ATK (Attack)**: Starting random 1-8, determines combat damage output
- **DEF (Defense)**: Starting random 1-8, reduces incoming damage
- **LUCK**: Starting random 1-8, affects critical hits and loot quality
- **CRAFT**: Starting 0, determines crafting success rate

### Character Progression
- **Stat Growth**: Defeating monsters permanently increases stats based on monster type
- **Equipment Bonuses**: Equipped items provide stat boosts
- **Temporary Effects**: Potions provide temporary stat modifications with complex priority system
- **Banking**: Store gold safely across deaths with fee system

### Death & Resurrection
- **Death State**: Player cannot move or use healing items when HP ≤ 0
- **Resurrection Charm**: Rare consumable that allows immediate revival with full stats
- **Reborn System**: Complete restart with new random starting stats (1-8 range)
- **Banking Persistence**: Gold stored in bank survives death with fee penalties

---

## Monster System

### Monster Types & Scaling
The game features 10 different monster types, each with 10-15 evolution levels:

#### Basic Monster Types (Levels 1-10)
1. **Goblin** (ATK specialist)
   - Base: 8 HP, 3 ATK, 1 DEF
   - Evolution: Goblin → Hobgoblin → Goblin Warrior → ... → Ancient Goblin

2. **Skeleton** (DEF specialist)
   - Base: 12 HP, 4 ATK, 2 DEF
   - Evolution: Skeleton → Skeleton Warrior → ... → Bone Archlich

3. **Wolf** (HP specialist)
   - Base: 15 HP, 5 ATK, 3 DEF
   - Evolution: Wolf → Wild Wolf → Dark Wolf → ... → Primordial Wolf

4. **Orc** (ATK specialist)
   - Base: 10 HP, 6 ATK, 2 DEF
   - Evolution: Orc → Orc Scout → ... → Orc Destroyer

5. **Ogre** (HP specialist)
   - Base: 12 HP, 4 ATK, 4 DEF
   - Evolution: Ogre → Ogre Brute → ... → Ancient Ogre

6. **Gnoll** (DEF specialist)
   - Base: 9 HP, 5 ATK, 3 DEF
   - Evolution: Gnoll → Gnoll Hunter → ... → Gnoll Demigod

7. **Wyvern** (ATK specialist)
   - Base: 10 HP, 6 ATK, 5 DEF
   - Evolution: Wyvern → Young Wyvern → ... → Wyvern God

#### Legendary Monster Types (Levels 1-15)
8. **Leviathan** (ATK specialist)
   - Base: 12 HP, 8 ATK, 6 DEF
   - Ultimate: Primordial Leviathan God (Level 15)

9. **Death Star** (DEF specialist)
   - Base: 15 HP, 7 ATK, 8 DEF
   - Ultimate: Primordial Death Star God (Level 15)

10. **Susano** (HP specialist)
    - Base: 18 HP, 9 ATK, 7 DEF
    - Ultimate: Primordial Susano God (Level 15)

### Monster Scaling System
- **Exponential Scaling**: Each level multiplies base stats by 2^(level-1)
- **Level 10 Bosses**: 512x multiplier, drop rare equipment (50% + luck bonus)
- **Level 15 Bosses**: 16384x multiplier, drop legendary weapons (90% + luck bonus)

### Monster Distribution
- **Distance-Based Spawning**: Monster difficulty increases with distance from town
- **Area Refresh**: Monsters respawn when player leaves and re-enters an area
- **Boss Concentration**: Higher-level monsters spawn in outer regions

---

## Combat System

### Combat Mechanics
- **Turn-Based**: Player attacks first, then monster, alternating until one dies
- **Damage Calculation**: Random damage from 1 to ATK stat
- **Defense Reduction**: DEF reduces incoming damage by DEF/2
- **Minimum Damage**: All attacks deal at least 1 damage regardless of defense
- **Critical Hits**: LUCK% chance to deal 1.5x damage

### Combat Formula
```
Player Damage = roll(1, totalATK) - floor(monsterDEF/2)
Monster Damage = roll(1, monsterATK) - floor(totalDEF/2)
Final Damage = max(1, calculated_damage)
```

### Equipment in Combat
- **Durability Loss**: Equipped items lose 1 durability per monster attack received
- **Automatic Unequip**: Items break (durability = 0) and are automatically unequipped
- **Stat Application**: Equipment boosts apply immediately to combat calculations

### Combat Rewards
- **Stat Gains**: Based on monster level and type specialty
- **Gold Rewards**: Exponential scaling (baseGold × 2^(level-1))
- **Resource Drops**: 30% + luck bonus chance for monster-specific materials
- **Special Drops**: Rare items from boss monsters

---

## Equipment System

### Equipment Categories
- **Weapons**: Provide ATK bonuses (swords, spears, hammers, etc.)
- **Armor**: Provide DEF and HP bonuses (armor, shields, boots, etc.)
- **Accessories**: Provide mixed stat bonuses (rings, amulets, cloaks, etc.)
- **Legendary Items**: Extremely powerful equipment from high-level content

### Equipment Properties
- **Boost Stats**: ATK, DEF, maxHP improvements
- **Durability**: How many hits the item can take (1-200 range)
- **Level**: Equipment tier (affects combination requirements)
- **Value**: Gold value for selling (some items unsellable)

### Equipment Tiers

#### Basic Equipment (Levels 1-3 monsters)
- Rusty Dagger, Leather Gloves, Iron Boots, etc.
- Low stat boosts (1-5 points)
- Common drops and crafting materials

#### Advanced Equipment (Levels 4-6 monsters)
- Chain Mail, Battle Axe, Magic Ring, etc.
- Moderate stat boosts (15-30 points)
- Rare drops from medium-level monsters

#### Elite Equipment (Levels 7-9 monsters)
- Dragon Scale Armor, Mystic Cloak, Warrior Gauntlets, etc.
- High stat boosts (50-100 points)
- Rare drops from high-level monsters

#### Legendary Equipment (Level 10 bosses)
- Ancient Goblin Crown (+250 ATK, +100 DEF)
- Bone Archlich Staff (+400 ATK)
- Wyvern God Scale (+600 ATK, +250 DEF, +500 HP)
- 50% + luck bonus drop rate from level 10 bosses

#### Ultimate Equipment (Level 15 bosses)
- Tidepiercer (+1000 ATK, +400 DEF) - from Leviathan
- Nova Maul (+600 ATK, +200 DEF, +1000 HP) - from Death Star
- Stormfang Katana (+2000 ATK, +1000 DEF) - from Susano
- 90% + luck bonus drop rate, unsellable

#### Endgame Equipment
- **Apex Predator**: Requires any 4 of 7 legendary items (+800 all stats)
- **Broken Stick**: Ultimate weapon requiring Apex Predator + all 3 ultimate weapons (+50000 ATK)

### Equipment Management
- **Single Equipped Item**: Only one item can be equipped at a time
- **Equipment Combining**: Merge two identical items to create higher-level versions
- **Repair System**: Restore durability using level 1 versions + gold or iron ore + gold
- **Selling**: Most equipment can be sold for gold (legendary items cannot be sold)

---

## Potion & Consumables System

### Healing Items
- **Health Potion** (5g): Restores 10 HP
- **Scroll of Healing** (25g): Restores full HP
- **Death Protection**: Cannot use healing items when dead (HP ≤ 0)

### Temporary Stat Potions
These potions use a sophisticated priority system for effect stacking:

#### Basic Temporary Effects
- **Strength Potion** (10g): +10-20 ATK for 30-90 steps
- **Warrior's Elixir** (30g): +15 ATK for 50 steps
- **Guardian Tonic** (20g): +20 DEF for 60 steps
- **Luck Charm** (50g): +20 LUCK for 20 steps
- **Master Crafter's Brew** (100g): +50 CRAFT for 15 crafting attempts

#### Multi-Effect Potions
- **Berserker's Blend** (50g): +10 ATK, +10 DEF for 40 steps
- **Battle Brew** (75g): +12 ATK, +8 DEF, +5 LUCK for 30 steps

#### Special Effect Potions
- **Orc's Rage** (175g): +200 ATK, -50% DEF for 40 steps
- **Angel's Touch** (250g): +200 max HP, +100 DEF, -25% ATK for 40 steps
- **Dragon Fury** (500g): +800 ATK, -70% DEF, -50% max HP for 10 steps
- **God's Gift** (2500g): +7000 DEF for 5 steps

### Permanent Effect Potions
- **Rage Potion** (100g): Permanent +0.5 ATK, -5 max HP

### Ultra-Rare Consumables
- **Resurrection Charm** (0.001% drop): One-time revival with all stats intact
- **Asura Blood** (0.0001% drop): 100x all stats for 100 steps, then 75% reduction for 50 steps

### Potion Priority System
- **Single Effects**: Priority = effect value
- **Multi-Effects**: Priority = sum of all effects + 100 bonus
- **Overwrite Rules**: Higher priority overwrites lower priority effects
- **Equal Priority**: Extends duration instead of overwriting
- **Weaker Effects**: Ignored if current effect has higher priority

---

## Crafting System

### Crafting Mechanics
- **Success Rate**: (CRAFT skill + LUCK + temporary bonuses)%
- **Skill Growth**: +0.1 CRAFT per crafting attempt (success or failure)
- **Material Consumption**: Materials always consumed on attempt
- **Real-time Updates**: Crafting interface updates success rates dynamically

### Crafting Categories

#### Basic Consumables
- Health Potion: Herb + Bottle
- Strength Potion: 2 Herbs
- Scroll of Healing: Paper + Herb

#### Materials & Components
- Iron Ingot: 2 Iron Ore + Coal
- Basic building block for weapon crafting

#### Weapons & Equipment
- Iron Sword: Iron Ingot + 2 Wood
- Shadowstrike Blade: 2 Iron Ingot + Skeleton Bone + Wood
- Thundercaller Hammer: 3 Iron Ingot + 2 Coal
- Voidreaper Scythe: 2 Iron Ingot + Orc Tusk + 2 Wood
- Dragonbane Spear: Iron Ingot + Wolf Pelt + 3 Wood
- Soulrender Axe: 2 Iron Ingot + Gnoll Claw + Wood

#### Advanced Equipment
- Scale Dagger: 2 Wyvern Scale + 2 Skeleton Bone
- Hide Shield: 5 Orc Tusk + Ogre Hide + 10 Skeleton Bone
- Wild Cape: 5 Ogre Hide + 2 Goblin Ear + 4 Wolf Pelt

#### Legendary Crafting
- **Apex Predator**: Any 4 of 7 legendary items (special "any 4 of 7" requirement)
- **Broken Stick**: Apex Predator + Tidepiercer + Nova Maul + Stormfang Katana

### Resource System
Resources are obtained by exploring the world and defeating monsters:

#### Basic Resources
- **Herb**: Found in world, used for potions
- **Wood**: Found in world, used for weapons
- **Iron Ore**: Found in world, used for metal crafting
- **Coal**: Found in world, used for smelting
- **Paper**: Found in world, used for scrolls
- **Bottle**: Found in world, used for potions

#### Monster-Specific Resources (30% + luck drop chance)
- **Goblin Ear**: From Goblins
- **Skeleton Bone**: From Skeletons
- **Wolf Pelt**: From Wolves
- **Orc Tusk**: From Orcs
- **Ogre Hide**: From Ogres
- **Gnoll Claw**: From Gnolls
- **Wyvern Scale**: From Wyverns

---

## Quest System

### Quest Types
1. **Kill Quests**: Defeat specific numbers of certain monsters
2. **Fetch Quests**: Collect and deliver specific items

### Town Quest System
Organized into difficulty tiers with increasing rewards:

#### Easy Quests (20-40 gold)
- Herb collection, basic monster kills
- Entry-level content for new players

#### Medium Quests (30-60 gold)
- Multiple goblin/skeleton kills
- Mid-tier resource gathering

#### Hard Quests (50-90 gold)
- Hobgoblin, Wyvern hunting
- Rare resource collection

#### Elite Quests (100-160 gold)
- Boss-level monsters (Goblin King, Skeleton Duke, etc.)
- High-tier challenges

#### Legendary Quests (5,000-100,000 gold)
- Level 10 boss monsters (Ancient Goblin, Bone Archlich, etc.)
- Level 15 ultimate bosses (Primordial Gods)
- Endgame content with massive rewards

### Random NPC Quest System
- **Dynamic Generation**: NPCs offer quests based on current area distance from town
- **Tiered Rewards**: Equipment rewards match area difficulty
- **Progress Tracking**: Real-time progress updates as players complete objectives
- **Equipment Rewards**: Instead of gold, provides gear appropriate to quest difficulty

#### NPC Professions
- **Hunter**: Requests Wolf Pelts, rewards Spear
- **Herbalist**: Requests Herbs, rewards Plate Armor
- **Miner**: Requests Iron Ore, rewards Greatsword
- **Scout**: Requests Wood, rewards Shield
- **Mercenary**: Requests Goblin Ears, rewards Iron Sword

### Quest Progress System
- **Kill Progress**: Tracked automatically when monsters are defeated
- **Fetch Progress**: Updated when items are collected
- **Turn-in Requirements**: Must meet all requirements before completion
- **Reward Distribution**: Gold added automatically, equipment goes to inventory

---

## World & Map System

### World Structure
- **Grid Size**: 1024×1024 total world (64×64 areas of 16×16 tiles each)
- **Town Center**: Located at coordinates (32, 32) - the safe starting area
- **Procedural Generation**: Each area generates content based on distance from town

### Area Types & Content

#### Town Area (32, 32)
- **Safe Zone**: No monsters, only NPCs and shops
- **NPCs**: Potion Seller, Blacksmith, Scroll Merchant, Banker, Equipment Upgrader, Repairer
- **Services**: All trading, banking, and equipment services available

#### Near Town (Distance ≤ 3)
- **Monster Levels**: 1-2 (Goblin, Skeleton, Wolf, etc.)
- **Resources**: Basic materials (Herb, Wood, Coal, Iron Ore)
- **Quest NPCs**: Offer easy-tier random quests
- **Chest Rewards**: Small gold amounts

#### Medium Distance (Distance 4-8)
- **Monster Levels**: 2-4 (Hobgoblin, Skeleton Warrior, Wild Wolf, etc.)
- **Resources**: All basic materials with higher frequency
- **Quest NPCs**: Offer medium-tier random quests
- **Better Loot**: Improved resource and equipment drops

#### Far Distance (Distance 9-16)
- **Monster Levels**: 3-7 (Elite variants, mid-tier bosses)
- **Dangerous Content**: Significant challenge increase
- **Quest NPCs**: Offer hard-tier random quests
- **Rare Resources**: Higher chance of valuable materials

#### Extreme Distance (Distance 17+)
- **Monster Levels**: 6-10+ (High-tier bosses, legendary monsters)
- **Level 10 Bosses**: Ancient variants with legendary equipment drops
- **Level 15 Bosses**: Ultimate gods in the furthest regions
- **Elite Quests**: Highest-tier random NPC quests

### Map Symbols & Navigation
- **P**: Player position
- **M**: Regular monsters (levels 1-9)
- **B**: Boss monsters (level 10)
- **G**: God-tier monsters (level 15)
- **R**: Resource nodes
- **C**: Treasure chests
- **N**: NPCs (town or quest-givers)
- **T**: Traps (deal 1-5 damage)

### Area Refresh System
- **Dynamic Regeneration**: Areas regenerate content when player leaves and returns
- **Persistent Progress**: Player position and quest progress maintained
- **Log Clearing**: Activity log clears when entering new areas

---

## Economy & Shop System

### Shop Types & Locations

#### Potion Seller (Town)
- **Health Potion** (5g): Basic healing
- **Strength Potion** (10g): Temporary ATK boost
- **Rage Potion** (100g): Permanent ATK/HP trade-off
- **Advanced Potions**: Various temporary and special effects (20g-2500g)

#### Blacksmith (Town)
- **Iron Sword** (20g): Basic weapon with +3 ATK

#### Scroll Merchant (Town)
- **Scroll of Healing** (25g): Full HP restoration
- **Luck Charm** (50g): Temporary luck boost

### Global Market System
- **Access**: Available everywhere (not just town)
- **Price Premium**: All items cost 2x normal town prices
- **Convenience Fee**: Trade-off for remote access
- **Full Inventory**: Same selection as town shops

### Selling System
- **Item Sales**: Most inventory items sell for 1g each
- **Equipment Sales**: Equipment sells for its defined value
- **Restrictions**: Legendary equipment cannot be sold
- **Equipped Items**: Cannot sell currently equipped items

### Price Scaling
- **Town Prices**: Base rates for all items
- **Global Market**: 2x town prices for convenience
- **Equipment Values**: Range from 5g (basic) to 1000g (Apex Predator)
- **Legendary Items**: Marked as unsellable (value 0)

---

## Banking System

### Banking Services
- **Gold Storage**: Secure storage that survives death and reborn
- **Death Protection**: Stored gold is not lost when player dies
- **Fee System**: Death fees charged on withdrawals if player died with banked gold

### Banking Mechanics
- **Deposit**: Transfer gold from inventory to bank (no fees)
- **Withdrawal**: Transfer gold from bank to inventory (death fees apply)
- **Death Fees**: 10 gold per death charged on first withdrawal
- **Account Closure**: Bank automatically closes if remaining balance after fees < 10g

### Banking Rules
- **Persistent Storage**: Banking data survives reborn (unless account was closed)
- **Death Recording**: Deaths only count if player had gold in bank when they died
- **Fee Accumulation**: Multiple deaths accumulate fees (deaths × 10g)
- **Reset Conditions**: Banking resets if player dies with no banked gold

### Banking Interface
- **Real-time Balance**: Shows current bank balance and outstanding fees
- **Deposit Options**: Specific amounts or all current gold
- **Withdrawal Options**: Specific amounts or all available gold (after fees)
- **Fee Display**: Clear indication of death penalties

---

## Progression & Rewards

### Character Growth Systems

#### Stat Progression from Combat
- **Monster Specialty Bonuses**: Each monster type has a preferred stat
  - ATK Specialists: Goblin, Orc, Wyvern, Leviathan (+ATK on defeat)
  - DEF Specialists: Skeleton, Gnoll, Death Star (+DEF on defeat)
  - HP Specialists: Wolf, Ogre, Susano (+maxHP and HP on defeat)

#### Stat Gain Formula by Monster Level
- **Levels 1-3**: 0.1, 0.2, 0.3 stat gain
- **Levels 4-6**: 0.6, 0.7, 0.8 stat gain
- **Levels 7-9**: 1.6, 1.7, 1.8 stat gain
- **Level 10**: 3.6 stat gain
- **Level 15 Special**: Fixed large bonuses (10 ATK/DEF, +0.2-0.5 LUCK)

#### Equipment Progression
- **Tier System**: Equipment power scales with area difficulty
- **Combination System**: Merge duplicate equipment for higher levels
- **Durability Management**: Repair systems to maintain equipment
- **Legendary Acquisition**: Boss farming for rare equipment

#### Skill Development
- **Crafting Skill**: Grows 0.1 per attempt regardless of success
- **Success Rate**: Crafting + Luck determines success percentage
- **Recipe Unlocking**: Advanced recipes require rare materials

### Reward Systems

#### Combat Rewards
- **Immediate Stat Growth**: Permanent stat increases from monster defeats
- **Gold Scaling**: Exponential gold rewards (2^level scaling)
- **Equipment Drops**: Boss monsters drop rare and legendary equipment
- **Resource Collection**: Materials for crafting and quest completion

#### Quest Rewards
- **Gold Payments**: Substantial gold rewards for quest completion
- **Equipment Rewards**: Random NPC quests provide tier-appropriate equipment
- **Progress Gates**: Higher-tier quests unlock through progression

#### Exploration Rewards
- **Treasure Chests**: Random gold finds with luck multipliers
- **Resource Nodes**: Crafting materials found throughout the world
- **Area Discovery**: New content unlocks with distance from town

---

## Special Features

### Resurrection System
- **Resurrection Charm**: Ultra-rare consumable (0.001% drop rate)
- **Automatic Revival**: Activates immediately on death
- **Full Restoration**: Restores all stats and returns player to town
- **One-Time Use**: Consumed on activation

### Asura Blood System
- **Ultra-Rare Drop**: 0.0001% chance from any monster
- **Phase 1**: 100x stat multiplier for 100 steps
- **Phase 2**: Stats return to normal, then 75% reduction for 50 steps
- **Phase 3**: Complete restoration to original stats
- **High Risk/Reward**: Massive temporary power with severe drawback

### Equipment Combination System
- **Level Progression**: Combine two identical items to create level+1 version
- **Stat Enhancement**: 50% increase in all stat bonuses
- **Durability Merging**: Combined durability (capped at 100)
- **Cost Scaling**: Gold cost increases with equipment level

### Repair System
- **Level 1 Method**: Use level 1 version + 100g for 20-60 durability
- **Iron Ore Method**: Use 10 iron ore + 200g for 10 durability
- **Level Requirement**: Only level 2+ equipment can be repaired
- **Random Restoration**: Level 1 method provides variable repair amounts

### Death and Revival Mechanics
- **Death State Prevention**: Cannot move, use items, or take actions when dead
- **Healing Restriction**: All healing items blocked in death state
- **Shop Restrictions**: Cannot buy healing items when dead
- **Revival Methods**: Only Reborn or Resurrection Charm can restore life

---

## Technical Architecture

### Client-Side Technology
- **Pure HTML/CSS/JavaScript**: No external frameworks or dependencies
- **Single-Page Application**: All functionality in one HTML file
- **Local Storage**: Persistent save data with corruption recovery
- **Real-time Updates**: Dynamic UI updates without page refreshes

### Data Management
- **Player State**: Complete character data including stats, inventory, equipment
- **World Generation**: Procedural area generation based on mathematical formulas
- **Quest Tracking**: Real-time progress monitoring for all quest types
- **Save System**: Automatic saving with data validation and error recovery

### Performance Optimizations
- **Area-Based Loading**: Only current area rendered and calculated
- **Lazy Generation**: World areas generated on-demand when visited
- **Memory Management**: Old area data cleared when moving to new areas
- **Efficient Updates**: Targeted DOM updates rather than full redraws

### User Interface Design
- **ASCII Art Aesthetic**: Monospace font for consistent character alignment
- **Dark Theme**: Atmospheric color scheme for dungeon exploration feel
- **Responsive Layout**: Adapts to different screen sizes
- **Modal System**: Layered interfaces for complex interactions

### Game Balance Philosophy
- **Exponential Scaling**: Both monster difficulty and rewards scale exponentially
- **Risk/Reward Balance**: Higher-risk areas provide proportionally better rewards
- **Multiple Progression Paths**: Combat, crafting, and quest systems all contribute to advancement
- **Player Choice**: Multiple viable strategies for character development

---

This comprehensive documentation covers every major system and mechanic in Dungeon Quest. The game provides a deep, complex experience with multiple interconnected systems that create emergent gameplay and strategic decision-making opportunities for players.