# ⚙️ Configuration Guide

Complete guide for configuring the Advanced Progress Bar System.

---

## Table of Contents

- [Overview](#overview)
- [Config Structure](#config-structure)
- [Prop Configuration](#prop-configuration)
- [Adding Custom Props](#adding-custom-props)
- [Bone IDs Reference](#bone-ids-reference)
- [Keybind Configuration](#keybind-configuration)
- [Best Practices](#best-practices)
- [Common Configurations](#common-configurations)

---

## Overview

The configuration file (`config.lua`) contains all customizable settings for the progress bar system. This includes prop definitions and keybind settings.

### File Location

```
overflow_progressbar/
└── config.lua
```

### Basic Structure

```lua
Config = Config or {}

-- Prop definitions
Config.PropsList = {
    -- Your props here
}

-- Keybinds
Config.CancelBinds = "X"
```

---

## Config Structure

### Config.PropsList

This table stores all prop configurations that can be used with the progress bar system.

**Structure:**

```lua
Config.PropsList = {
    ["propIdentifier"] = {
        model = "prop_model_name",
        bone = 28422,
        x = 0.0,
        y = 0.0,
        z = 0.0,
        xR = 0.0,
        yR = 0.0,
        zR = 0.0
    }
}
```

**Parameters:**

| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `propIdentifier` | `string` | Unique name for the prop | `"water"`, `"hammer"` |
| `model` | `string` | Game model hash or name | `"prop_ld_flow_bottle"` |
| `bone` | `number` | Bone ID to attach to | `28422` (right hand) |
| `x` | `number` | Position offset X axis | `0.0` to `1.0` |
| `y` | `number` | Position offset Y axis | `0.0` to `1.0` |
| `z` | `number` | Position offset Z axis | `-0.5` to `0.5` |
| `xR` | `number` | Rotation X axis (degrees) | `0.0` to `360.0` |
| `yR` | `number` | Rotation Y axis (degrees) | `0.0` to `360.0` |
| `zR` | `number` | Rotation Z axis (degrees) | `0.0` to `360.0` |

---

## Prop Configuration

### Default Props

The system comes with pre-configured props:

#### Beverages - Non-Alcoholic

```lua
["cup"] = {
    model = "prop_plastic_cup_02",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
["water"] = {
    model = "prop_ld_flow_bottle",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
["coffee"] = {
    model = "p_amb_coffeecup_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

#### Beverages - Alcoholic

```lua
["beer"] = {
    model = "prop_beer_bottle",
    bone = 28422,
    x = 0.0, y = 0.0, z = -0.15,
    xR = 0.0, yR = 0.0, zR = 0.0
},
["whiskey"] = {
    model = "p_whiskey_bottle_s",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

#### Food Items

```lua
["hamburger"] = {
    model = "prop_cs_burger_01",
    bone = 18905,  -- Left hand
    x = 0.13, y = 0.07, z = 0.02,
    xR = 120.0, yR = 16.0, zR = 60.0
},
["sandwich"] = {
    model = "prop_sandwich_01",
    bone = 18905,
    x = 0.13, y = 0.05, z = 0.02,
    xR = -50.0, yR = 16.0, zR = 60.0
},
```

---

## Adding Custom Props

### Step-by-Step Guide

#### 1. Find the Prop Model

Use a model viewer or check the [GTA V Model List](https://forge.plebmasters.de/objects).

#### 2. Add to Config

```lua
Config.PropsList = {
    -- Existing props...
    
    -- Your new prop
    ["yourpropname"] = {
        model = "prop_tool_hammer",
        bone = 28422,
        x = 0.0,
        y = 0.0,
        z = 0.0,
        xR = 0.0,
        yR = 0.0,
        zR = 0.0
    }
}
```

#### 3. Test and Adjust

```lua
-- Test in-game
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Testing prop...",
    prop = "yourpropname"
})
```

#### 4. Fine-Tune Position

Adjust `x`, `y`, `z`, `xR`, `yR`, `zR` values until the prop looks correct.

### Position Guidelines

**X Axis** (Left/Right):
- Negative = Left
- Positive = Right
- Range: `-0.5` to `0.5`

**Y Axis** (Forward/Back):
- Negative = Backward
- Positive = Forward
- Range: `-0.5` to `0.5`

**Z Axis** (Up/Down):
- Negative = Down
- Positive = Up
- Range: `-0.5` to `0.5`

**Rotations** (degrees):
- `xR` = Pitch (up/down tilt)
- `yR` = Yaw (left/right turn)
- `zR` = Roll (side tilt)
- Range: `0.0` to `360.0`

### Example Configurations

#### Tool Props

```lua
-- Hammer
["hammer"] = {
    model = "prop_tool_hammer",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Wrench
["wrench"] = {
    model = "prop_tool_wrench",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Pickaxe
["pickaxe"] = {
    model = "prop_tool_pickaxe",
    bone = 28422,
    x = 0.08, y = -0.05, z = -0.15,
    xR = 90.0, yR = 0.0, zR = 0.0
},

-- Crowbar
["crowbar"] = {
    model = "prop_cs_crowbar",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

#### Medical Props

```lua
-- First Aid Kit
["medkit"] = {
    model = "prop_ld_health_pack",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Defibrillator
["defib"] = {
    model = "prop_defib_01",
    bone = 28422,
    x = 0.1, y = 0.03, z = 0.0,
    xR = -60.0, yR = 0.0, zR = 0.0
},

-- Pills
["pills"] = {
    model = "prop_pill_bottle_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

#### Electronics

```lua
-- Phone
["phone"] = {
    model = "prop_npc_phone_02",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Tablet
["tablet"] = {
    model = "prop_cs_tablet",
    bone = 28422,
    x = 0.03, y = 0.0, z = 0.03,
    xR = -130.0, yR = -50.0, zR = 0.0
},

-- Laptop
["laptop"] = {
    model = "prop_laptop_01a",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

#### Weapons/Criminal

```lua
-- Lockpick (visual)
["lockpick"] = {
    model = "prop_tool_screwdvr01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Drill
["drill"] = {
    model = "prop_tool_drill",
    bone = 28422,
    x = 0.06, y = 0.0, z = 0.02,
    xR = 90.0, yR = 0.0, zR = 0.0
},
```

#### Miscellaneous

```lua
-- Clipboard
["clipboard"] = {
    model = "p_amb_clipboard_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Trash Bag
["trashbag"] = {
    model = "prop_cs_rub_binbag_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = -0.1,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Package/Box
["package"] = {
    model = "prop_cs_cardbox_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

---

## Bone IDs Reference

### Main Bones

| Bone ID | Bone Name | Description | Common Use |
|---------|-----------|-------------|------------|
| **28422** | `SKEL_R_Hand` | Right hand (IK) | **Most props** |
| **18905** | `SKEL_L_Hand` | Left hand (IK) | Food items |
| **60309** | `IK_Head` | Head | Hats, masks |
| **24816** | `SKEL_ROOT` | Pelvis/Root | Bags, backpacks |
| **24818** | `SKEL_Pelvis` | Pelvis | Belt items |
| **11816** | `SKEL_R_Foot` | Right foot | Ankle items |
| **2108** | `SKEL_L_Foot` | Left foot | Ankle items |

### Hand Bones (Detailed)

| Bone ID | Bone Name | Description |
|---------|-----------|-------------|
| 28422 | `SKEL_R_Hand` | Right hand (primary) |
| 18905 | `SKEL_L_Hand` | Left hand (primary) |
| 57005 | `SKEL_R_Finger00` | Right thumb |
| 58866 | `SKEL_R_Finger01` | Right index |
| 58867 | `SKEL_R_Finger02` | Right middle |
| 58868 | `SKEL_R_Finger10` | Right ring |
| 58869 | `SKEL_R_Finger20` | Right pinky |

### Body Bones

| Bone ID | Bone Name | Description |
|---------|-----------|-------------|
| 24816 | `SKEL_ROOT` | Root bone |
| 24817 | `SKEL_Pelvis` | Pelvis |
| 64729 | `SKEL_L_Thigh` | Left thigh |
| 51826 | `SKEL_R_Thigh` | Right thigh |
| 14201 | `SKEL_Spine0` | Lower spine |
| 14202 | `SKEL_Spine1` | Mid spine |
| 14203 | `SKEL_Spine2` | Upper spine |
| 14209 | `SKEL_Spine3` | Chest |

### Head Bones

| Bone ID | Bone Name | Description |
|---------|-----------|-------------|
| 31086 | `SKEL_Head` | Head center |
| 60309 | `IK_Head` | Head IK target |
| 45509 | `SKEL_Neck_1` | Neck |

---

## Keybind Configuration

### Default Keybind

```lua
Config.CancelBinds = "X"
```

### Changing the Cancel Key

Edit the `Config.CancelBinds` value in `config.lua`:

```lua
-- Use E key
Config.CancelBinds = "E"

-- Use Backspace
Config.CancelBinds = "BACK"

-- Use ESC
Config.CancelBinds = "ESC"

-- Use Delete
Config.CancelBinds = "DELETE"
```

### Supported Keys

Common FiveM key names:

| Key | FiveM Name |
|-----|------------|
| A-Z | `A`, `B`, `C`, etc. |
| F1-F12 | `F1`, `F2`, etc. |
| ESC | `ESC` |
| Enter | `RETURN` |
| Backspace | `BACK` |
| Delete | `DELETE` |
| Space | `SPACE` |
| Arrows | `UP`, `DOWN`, `LEFT`, `RIGHT` |
| Number Row | `1`, `2`, `3`, etc. |
| Numpad | `NUMPAD1`, `NUMPAD2`, etc. |

Full list: [FiveM Key Mappings](https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/)

---

## Best Practices

### Prop Configuration

1. **Always test props in-game** before committing
2. **Use descriptive names** for prop identifiers
3. **Group similar props** together in the config
4. **Comment complex configurations**
5. **Keep backups** of working configurations

### Performance

1. **Limit number of props** in Config.PropsList to what you actually use
2. **Remove unused props** to reduce memory usage
3. **Use existing props** when possible instead of adding duplicates

### Organization

```lua
Config.PropsList = {
    -- ========================================================================
    -- BEVERAGES
    -- ========================================================================
    ["water"] = { ... },
    ["coffee"] = { ... },
    
    -- ========================================================================
    -- TOOLS
    -- ========================================================================
    ["hammer"] = { ... },
    ["wrench"] = { ... },
    
    -- ========================================================================
    -- MEDICAL
    -- ========================================================================
    ["medkit"] = { ... },
    ["bandage"] = { ... },
}
```

---

## Common Configurations

### Job System Props

```lua
-- Garbage Collector
["trashbag"] = {
    model = "prop_cs_rub_binbag_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = -0.1,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Delivery Driver
["package"] = {
    model = "prop_cs_cardbox_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Miner
["pickaxe"] = {
    model = "prop_tool_pickaxe",
    bone = 28422,
    x = 0.08, y = -0.05, z = -0.15,
    xR = 90.0, yR = 0.0, zR = 0.0
},

-- Mechanic
["wrench"] = {
    model = "prop_tool_wrench",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

### Roleplay Props

```lua
-- Cigarette
["cigarette"] = {
    model = "prop_cs_ciggy_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Newspaper
["newspaper"] = {
    model = "prop_news_disp_02a",
    bone = 28422,
    x = 0.1, y = 0.0, z = 0.0,
    xR = -130.0, yR = -50.0, zR = 0.0
},

-- Camera
["camera"] = {
    model = "prop_pap_camera_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

### Criminal Props

```lua
-- Lockpick
["lockpick"] = {
    model = "prop_tool_screwdvr01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},

-- Drill
["drill"] = {
    model = "prop_tool_drill",
    bone = 28422,
    x = 0.06, y = 0.0, z = 0.02,
    xR = 90.0, yR = 0.0, zR = 0.0
},

-- Thermite
["thermite"] = {
    model = "prop_bomb_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
},
```

---

## Troubleshooting

### Prop Not Appearing

**Issue:** Prop doesn't show when called

**Solutions:**
1. Verify model name is correct
2. Check if model exists in game
3. Ensure prop name matches in config and script
4. Test with a known working prop (e.g., "water")

### Prop Position Wrong

**Issue:** Prop is floating or misaligned

**Solutions:**
1. Adjust `x`, `y`, `z` values incrementally
2. Try different bone IDs
3. Adjust rotation values (`xR`, `yR`, `zR`)
4. Reference similar props for positioning

### Prop Rotated Incorrectly

**Issue:** Prop is upside down or sideways

**Solutions:**
1. Adjust rotation values:
   - `xR` for pitch
   - `yR` for yaw
   - `zR` for roll
2. Try values in 90-degree increments (0, 90, 180, 270)
3. Fine-tune with smaller increments (±5, ±10)

---

<div align="center">

**[⬆ Back to Top](#️-configuration-guide)**

</div>