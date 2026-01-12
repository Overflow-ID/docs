# 📖 API Reference

Complete API documentation for Advanced Progress Bar System.

---

## Table of Contents

- [Exports](#exports)
  - [Progress](#progress)
  - [Cancel](#cancel)
  - [CancelAll](#cancelall)
  - [IsProgressActive](#isprogressactive)
  - [GetQueueLength](#getqueuelength)
  - [ClearQueue](#clearqueue)
- [Data Structures](#data-structures)
- [Animation Reference](#animation-reference)
- [Prop Reference](#prop-reference)
- [Particle Reference](#particle-reference)

---

## Exports

### Progress

Start a new progress bar with specified configuration.

#### Syntax

```lua
exports['overflow_progressbar']:Progress(data, callback)
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `data` | `table` | ✅ Yes | Progress configuration object |
| `callback` | `function` | ❌ No | Callback function executed when progress completes or is cancelled |

#### Returns

| Type | Description |
|------|-------------|
| `boolean` | `true` if progress started or queued successfully, `false` if rejected |

#### Data Object Structure

```lua
{
    -- REQUIRED FIELDS
    duration = number,              -- Duration in milliseconds (ms)
    label = string,                 -- Display text shown on progress bar
    
    -- OPTIONAL FIELDS
    icon = string,                  -- Font Awesome icon class (e.g., "fas fa-hammer")
    canCancel = boolean,            -- Allow cancellation with keybind (default: false)
    allowNested = boolean,          -- Allow queueing when another progress is active (default: false)
    
    -- INTERRUPT CONDITIONS
    useWhileDead = boolean,         -- Continue progress if player dies (default: false)
    allowRagdoll = boolean,         -- Continue if player ragdolls (default: false)
    allowCuffed = boolean,          -- Continue if player is cuffed (default: false)
    allowFalling = boolean,         -- Continue if player is falling (default: false)
    allowSwimming = boolean,        -- Continue if player is swimming (default: false)
    
    -- CONTROL DISABLING
    disableControls = {
        mouse = boolean,            -- Disable camera movement (default: false)
        move = boolean,             -- Disable WASD movement (default: false)
        sprint = boolean,           -- Disable sprint only (default: false)
        car = boolean,              -- Disable vehicle controls (default: false)
        combat = boolean            -- Disable combat actions (default: false)
    },
    
    -- ANIMATION
    animation = {
        -- Dictionary Animation
        dict = string,              -- Animation dictionary name
        anim = string,              -- Animation name
        blendIn = number,           -- Blend in speed (default: 3.0)
        blendOut = number,          -- Blend out speed (default: 1.0)
        duration = number,          -- Animation duration in ms (default: -1 = loop)
        flag = number,              -- Animation flag (default: 49)
        playbackRate = number,      -- Playback rate (default: 0)
        lockX = boolean,            -- Lock X axis (default: false)
        lockY = boolean,            -- Lock Y axis (default: false)
        lockZ = boolean,            -- Lock Z axis (default: false)
        
        -- OR Scenario
        scenario = string,          -- Scenario name (alternative to dict/anim)
        playEnter = boolean         -- Play enter animation (default: true)
    },
    
    -- PROPS
    prop = string,                  -- Single prop from config by name
    -- OR
    prop = {                        -- Custom single prop
        model = string,             -- Prop model hash/name
        bone = number,              -- Bone ID to attach to
        x = number,                 -- X position offset
        y = number,                 -- Y position offset
        z = number,                 -- Z position offset
        xR = number,                -- X rotation in degrees
        yR = number,                -- Y rotation in degrees
        zR = number                 -- Z rotation in degrees
    },
    -- OR
    prop = {                        -- Multiple props (array)
        "propname1",                -- Config prop
        "propname2",                -- Config prop
        {                           -- Custom prop
            model = string,
            bone = number,
            x = number, y = number, z = number,
            xR = number, yR = number, zR = number
        }
    },
    
    -- PARTICLES
    particles = {
        {
            assetName = string,     -- Particle asset name
            effectName = string,    -- Particle effect name
            bone = number,          -- Bone ID (default: 60309)
            offset = {
                x = number,         -- X offset (default: 0.0)
                y = number,         -- Y offset (default: 0.0)
                z = number          -- Z offset (default: 0.0)
            },
            rotation = {
                x = number,         -- X rotation (default: 0.0)
                y = number,         -- Y rotation (default: 0.0)
                z = number          -- Z rotation (default: 0.0)
            },
            scale = number          -- Particle scale (default: 1.0)
        }
    },
    
    -- LIFECYCLE CALLBACKS
    onFinish = function,            -- Called when progress completes successfully
    onCancel = function             -- Called when progress is cancelled
}
```

#### Callback Functions

The Progress function supports two callback approaches:

**Approach 1: Using onFinish and onCancel (Recommended)**
```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing...",
    onFinish = function()
        print("Completed!")
    end,
    onCancel = function()
        print("Cancelled!")
    end
})
```

**Approach 2: Legacy callback parameter**
```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing..."
}, function(cancelled)
    -- cancelled: boolean
    --   true  = Progress was cancelled (by user or interrupt)
    --   false = Progress completed successfully
    if not cancelled then
        print("Completed!")
    end
end)
```

*Note: Using onFinish and onCancel is recommended as it provides clearer code structure.*

#### Examples

**Basic Usage:**
```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing...",
    onFinish = function()
        print("Completed!")
    end
})
```

**With Animation:**
```lua
exports['overflow_progressbar']:Progress({
    duration = 8000,
    label = "Repairing...",
    canCancel = true,
    animation = {
        dict = "mini@repair",
        anim = "fixing_a_player"
    },
    disableControls = {
        move = true,
        combat = true
    }
})
```

**With Props:**
```lua
exports['overflow_progressbar']:Progress({
    duration = 3000,
    label = "Drinking water...",
    prop = "water",
    animation = {
        dict = "mp_player_intdrink",
        anim = "loop_bottle"
    }
})
```

**Queued Progress:**
```lua
-- First progress
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Step 1...",
    allowNested = true
})

-- This will queue and run after first completes
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Step 2...",
    allowNested = true
})
```

---

### Cancel

Cancel the currently active progress bar.

#### Syntax

```lua
exports['overflow_progressbar']:Cancel()
```

#### Parameters

None

#### Returns

None

#### Behavior

- Stops current progress immediately
- Calls `onCancel` callback if defined
- Calls completion callback with `cancelled = true`
- Cleans up animations, props, and particles
- Processes next item in queue if exists

#### Example

```lua
-- Cancel from another script
RegisterCommand('stopprogress', function()
    exports['overflow_progressbar']:Cancel()
end)
```

---

### CancelAll

Cancel all progress bars including queued ones.

#### Syntax

```lua
exports['overflow_progressbar']:CancelAll()
```

#### Parameters

None

#### Returns

None

#### Behavior

- Cancels current progress
- Clears entire queue
- Resets player busy state
- Updates UI queue display

#### Example

```lua
-- Emergency stop all progress
RegisterNetEvent('admin:stopAllProgress', function()
    exports['overflow_progressbar']:CancelAll()
end)
```

---

### IsProgressActive

Check if a progress bar is currently active.

#### Syntax

```lua
local isActive = exports['overflow_progressbar']:IsProgressActive()
```

#### Parameters

None

#### Returns

| Type | Description |
|------|-------------|
| `boolean` | `true` if progress is active, `false` otherwise |

#### Example

```lua
-- Prevent action if busy
RegisterCommand('dosomething', function()
    if exports['overflow_progressbar']:IsProgressActive() then
        TriggerEvent('notify', 'You are busy!')
        return
    end
    
    -- Do something
end)
```

---

### GetQueueLength

Get the number of progress bars currently in queue.

#### Syntax

```lua
local queueLength = exports['overflow_progressbar']:GetQueueLength()
```

#### Parameters

None

#### Returns

| Type | Description |
|------|-------------|
| `number` | Number of queued progress bars (0 = empty queue) |

#### Example

```lua
-- Check queue before adding more
local queueLength = exports['overflow_progressbar']:GetQueueLength()

if queueLength > 5 then
    TriggerEvent('notify', 'Too many tasks queued!')
else
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "Task...",
        allowNested = true
    })
end
```

---

### ClearQueue

Clear all queued progress bars without cancelling the current one.

#### Syntax

```lua
exports['overflow_progressbar']:ClearQueue()
```

#### Parameters

None

#### Returns

None

#### Behavior

- Removes all queued progress bars
- Does NOT cancel current progress
- Updates UI queue counter
- Current progress will complete normally

#### Example

```lua
-- Clear queue but let current finish
RegisterCommand('clearqueue', function()
    exports['overflow_progressbar']:ClearQueue()
    local current = exports['overflow_progressbar']:IsProgressActive()
    print("Current progress still active:", current)
end)
```

---

## Data Structures

### DisableControls Object

```lua
disableControls = {
    mouse = boolean,    -- Camera movement (controls 1, 2, 106)
    move = boolean,     -- Movement (controls 21, 30, 31, 36)
    sprint = boolean,   -- Sprint only (control 21)
    car = boolean,      -- Vehicle controls (63, 64, 71, 72, 75)
    combat = boolean    -- Combat actions (25, 140, 141, 142, 257 + firing)
}
```

**Control IDs Reference:**

| ID | Description |
|----|-------------|
| 1 | Camera Left/Right |
| 2 | Camera Up/Down |
| 21 | Sprint |
| 25 | Aim |
| 30 | Move Left/Right |
| 31 | Move Up/Down |
| 36 | Duck/Sneak |
| 63 | Vehicle Left/Right |
| 64 | Vehicle Forward/Back |
| 71 | Vehicle Accelerate |
| 72 | Vehicle Brake |
| 75 | Exit Vehicle |
| 140 | Melee Light |
| 141 | Melee Heavy |
| 142 | Melee Alternate |
| 257 | Attack 1 |

---

### Animation Object

```lua
-- Dictionary Animation
animation = {
    dict = "anim_dict_name",
    anim = "anim_name",
    blendIn = 3.0,          -- Optional
    blendOut = 1.0,         -- Optional
    duration = -1,          -- Optional (-1 = loop)
    flag = 49,              -- Optional
    playbackRate = 0,       -- Optional
    lockX = false,          -- Optional
    lockY = false,          -- Optional
    lockZ = false           -- Optional
}

-- OR Scenario
animation = {
    scenario = "SCENARIO_NAME",
    playEnter = true        -- Optional
}
```

**Common Animation Flags:**

| Flag | Description |
|------|-------------|
| 0 | Normal |
| 1 | Repeat |
| 2 | Stop on last frame |
| 4 | Only upper body |
| 8 | Allow player control |
| 16 | Cancellable |
| 32 | Enable ragdoll on collision |
| 49 | Default (1 + 16 + 32) |

---

### Prop Object

```lua
-- From Config
prop = "propname"

-- Custom Single Prop
prop = {
    model = "prop_model_name",
    bone = 28422,
    x = 0.0,
    y = 0.0,
    z = 0.0,
    xR = 0.0,
    yR = 0.0,
    zR = 0.0
}

-- Multiple Props
prop = {
    "water",
    "coffee",
    {
        model = "prop_tool_hammer",
        bone = 28422,
        x = 0.0, y = 0.0, z = 0.0,
        xR = 0.0, yR = 0.0, zR = 0.0
    }
}
```

---

### Particle Object

```lua
particles = {
    {
        assetName = "core",
        effectName = "ent_sht_steam",
        bone = 60309,
        offset = {
            x = 0.0,
            y = 0.0,
            z = 0.0
        },
        rotation = {
            x = 0.0,
            y = 0.0,
            z = 0.0
        },
        scale = 1.0
    }
}
```

---

## Animation Reference

### Common Animations

#### Working/Repairing

```lua
-- Hammering
animation = {
    dict = "amb@world_human_hammering@male@base",
    anim = "base"
}

-- Welding
animation = {
    scenario = "WORLD_HUMAN_WELDING"
}

-- Vehicle Repair
animation = {
    dict = "mini@repair",
    anim = "fixing_a_player"
}

-- Mechanic
animation = {
    dict = "amb@world_human_vehicle_mechanic@male@base",
    anim = "base"
}
```

#### Gathering/Harvesting

```lua
-- Gardening
animation = {
    dict = "amb@world_human_gardener_plant@male@base",
    anim = "base"
}

-- Picking
animation = {
    scenario = "WORLD_HUMAN_GARDENER_PLANT"
}

-- Digging
animation = {
    dict = "amb@world_human_const_drill@male@drill@base",
    anim = "base"
}
```

#### Medical

```lua
-- Clipboard
animation = {
    scenario = "WORLD_HUMAN_CLIPBOARD"
}

-- CPR
animation = {
    dict = "mini@cpr@char_a@cpr_str",
    anim = "cpr_pumpchest"
}
```

#### Drinking

```lua
-- Bottle
animation = {
    dict = "mp_player_intdrink",
    anim = "loop_bottle"
}

-- Coffee
animation = {
    dict = "amb@world_human_aa_coffee@base",
    anim = "base"
}
```

#### Eating

```lua
-- Burger
animation = {
    dict = "mp_player_inteat@burger",
    anim = "mp_player_int_eat_burger"
}

-- Sandwich
animation = {
    scenario = "WORLD_HUMAN_SEAT_WALL_EATING"
}
```

#### Smoking

```lua
-- Cigarette
animation = {
    scenario = "WORLD_HUMAN_SMOKING"
}
```

#### Criminal

```lua
-- Lockpicking
animation = {
    dict = "veh@break_in@0h@p_m_one@",
    anim = "low_force_entry_ds"
}

-- Hotwiring
animation = {
    dict = "anim@amb@clubhouse@tutorial@bkr_tut_ig3@",
    anim = "machinic_loop_mechandplayer"
}
```

---

## Prop Reference

### Built-in Props

Props defined in `Config.PropsList`:

#### Beverages

| Prop Name | Model | Description |
|-----------|-------|-------------|
| `cup` | `prop_plastic_cup_02` | Plastic cup |
| `water` | `prop_ld_flow_bottle` | Water bottle |
| `coffee` | `p_amb_coffeecup_01` | Coffee cup |
| `cola` | `prop_ecola_can` | E-Cola can |
| `energydrink` | `prop_energy_drink` | Energy drink |
| `beer` | `prop_beer_bottle` | Beer bottle |
| `whiskey` | `p_whiskey_bottle_s` | Whiskey bottle |
| `vodka` | `prop_vodka_bottle` | Vodka bottle |

#### Food

| Prop Name | Model | Description |
|-----------|-------|-------------|
| `hamburger` | `prop_cs_burger_01` | Hamburger |
| `sandwich` | `prop_sandwich_01` | Sandwich |
| `donut` | `prop_amb_donut` | Donut |
| `taco` | `prop_taco_01` | Taco |

### Custom Props

#### Tools

```lua
-- Hammer
prop = {
    model = "prop_tool_hammer",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}

-- Wrench
prop = {
    model = "prop_tool_wrench",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}

-- Pickaxe
prop = {
    model = "prop_tool_pickaxe",
    bone = 28422,
    x = 0.08, y = -0.05, z = -0.15,
    xR = 90.0, yR = 0.0, zR = 0.0
}
```

#### Medical

```lua
-- First Aid Kit
prop = {
    model = "prop_ld_health_pack",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}

-- Defibrillator
prop = {
    model = "prop_defib_01",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}
```

### Bone IDs

| Bone ID | Bone Name | Description |
|---------|-----------|-------------|
| 28422 | `SKEL_R_Hand` | Right hand (recommended) |
| 18905 | `SKEL_L_Hand` | Left hand |
| 60309 | `IK_Head` | Head |
| 24816 | `SKEL_ROOT` | Pelvis/Root |
| 11816 | `SKEL_R_Foot` | Right foot |
| 2108 | `SKEL_L_Foot` | Left foot |

---

## Particle Reference

### Common Particle Effects

#### Sparks/Welding

```lua
particles = {
    {
        assetName = "scr_reconstructionaccident",
        effectName = "scr_sparking_wires",
        bone = 28422,
        scale = 1.0
    }
}
```

#### Steam

```lua
particles = {
    {
        assetName = "core",
        effectName = "ent_sht_steam",
        bone = 60309,
        scale = 1.0
    }
}
```

#### Electrical

```lua
particles = {
    {
        assetName = "core",
        effectName = "ent_dst_elec_fire_sp",
        bone = 28422,
        scale = 1.0
    }
}
```

#### Fire

```lua
particles = {
    {
        assetName = "core",
        effectName = "fire_wrecked_plane_cockpit",
        bone = 60309,
        scale = 0.5
    }
}
```

#### Smoke

```lua
particles = {
    {
        assetName = "core",
        effectName = "exp_grd_bzgas_smoke",
        bone = 60309,
        scale = 1.0
    }
}
```

---

## Error Codes

| Code | Message | Solution |
|------|---------|----------|
| N/A | Progress blocked - allowNested is false | Set `allowNested = true` or wait for current progress to finish |
| N/A | Prop 'X' not found in config | Add prop to Config.PropsList or use custom prop object |
| N/A | Animation dict not loaded | Verify animation dictionary name is correct |

---

## Best Practices

### Performance

1. **Avoid extremely short durations** (< 1000ms)
2. **Limit particle effects** to 2-3 maximum
3. **Clean up callbacks** - avoid heavy operations
4. **Use scenarios** instead of dict animations when possible

### UX

1. **Provide meaningful labels** - be descriptive
2. **Use appropriate icons** - match the action
3. **Set reasonable durations** - not too fast or slow
4. **Allow cancellation** for long tasks
5. **Use queues wisely** - don't queue too many

### Code

1. **Always handle callbacks** - check `cancelled` parameter
2. **Validate inputs** - check if player can perform action
3. **Use interrupts appropriately** - prevent exploits
4. **Test thoroughly** - verify all edge cases

---

<div align="center">

**[⬆ Back to Top](#-api-reference)**

</div>