# Basic Concepts

Understanding the core concepts behind the Advanced Progress Bar System.

## Progress Bar Lifecycle

Every progress bar goes through these stages:

1. **Initialization** - Progress starts, UI shows
2. **Active** - Progress is running
3. **Completion** - Progress ends (success or cancelled)
4. **Cleanup** - Animations stop, props removed

```
Start → Active → Completion → Cleanup
```

## Data Object

The main configuration object passed to `Progress()`:

```lua
{
    -- Required
    duration = number,      -- Time in milliseconds
    label = string,         -- Display text

    -- Optional
    icon = string,          -- Font Awesome icon
    canCancel = boolean,    -- Allow cancellation
    allowNested = boolean,  -- Enable queueing

    -- Advanced
    animation = {},         -- Animation data
    prop = {},             -- Prop data
    particles = {},        -- Particle effects
    disableControls = {},  -- Control settings

    -- Callbacks
    onFinish = function,   -- Called on success
    onCancel = function    -- Called on cancel
}
```

## Callback Functions

Progress bars support two callback approaches:

### Recommended: onFinish and onCancel

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing...",
    onFinish = function()
        -- Handle success
        TriggerEvent('notify', 'Done!')
    end,
    onCancel = function()
        -- Handle cancellation
        TriggerEvent('notify', 'Cancelled!')
    end
})
```

### Legacy: Single Callback Parameter

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing..."
}, function(cancelled)
    -- cancelled: boolean
    -- true  = Progress was cancelled
    -- false = Progress completed successfully
    if cancelled then
        TriggerEvent('notify', 'Cancelled!')
    else
        TriggerEvent('notify', 'Done!')
    end
end)
```

**Note:** Using `onFinish` and `onCancel` is recommended for clearer code structure.

## Interrupts

Progress can be automatically cancelled when:

- Player dies (unless `useWhileDead = true`)
- Player ragdolls (unless `allowRagdoll = true`)
- Player is cuffed (unless `allowCuffed = true`)
- Player is falling (unless `allowFalling = true`)
- Player is swimming (unless `allowSwimming = true`)

Example:

```lua
exports['overflow_progressbar']:Progress({
    duration = 10000,
    label = "Repairing...",
    useWhileDead = false,    -- Cancel if player dies
    allowRagdoll = false,    -- Cancel if player ragdolls
    allowSwimming = false    -- Cancel if player swims
})
```

## Control Disabling

You can disable specific player controls during progress:

```lua
disableControls = {
    mouse = boolean,    -- Camera movement
    move = boolean,     -- WASD movement
    sprint = boolean,   -- Sprint only
    car = boolean,      -- Vehicle controls
    combat = boolean    -- Combat actions
}
```

Common combinations:

**Full Lockdown:**
```lua
disableControls = {
    mouse = true,
    move = true,
    car = true,
    combat = true
}
```

**Allow Looking:**
```lua
disableControls = {
    move = true,
    combat = true,
    car = true
}
```

**Combat Only:**
```lua
disableControls = {
    combat = true
}
```

## Animations

Two types of animations are supported:

### Dictionary Animation

```lua
animation = {
    dict = "anim_dict_name",
    anim = "anim_name"
}
```

### Scenario

```lua
animation = {
    scenario = "SCENARIO_NAME"
}
```

Scenarios are simpler and don't require loading animation dictionaries.

## Props

Props can be attached to the player in three ways:

### Config Reference

```lua
prop = "water"  -- Uses prop from Config.PropsList
```

### Custom Single Prop

```lua
prop = {
    model = "prop_tool_hammer",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}
```

### Multiple Props

```lua
prop = {
    "water",           -- Config prop
    "coffee",          -- Config prop
    {                  -- Custom prop
        model = "prop_tool_hammer",
        bone = 28422,
        x = 0.0, y = 0.0, z = 0.0,
        xR = 0.0, yR = 0.0, zR = 0.0
    }
}
```

## Queue System

When `allowNested = true`, progress bars will queue:

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

The queue processes items sequentially (FIFO - First In, First Out).

## State Management

The system automatically manages player state:

```lua
-- When progress is active
LocalPlayer.state.invBusy = true

-- When no progress is active
LocalPlayer.state.invBusy = false
```

You can check this state in other scripts:

```lua
if LocalPlayer.state.invBusy then
    print("Player is busy")
else
    print("Player is free")
end
```

## Progress Management

Check progress status:

```lua
-- Is progress currently active?
local isActive = exports['overflow_progressbar']:IsProgressActive()

-- Get queue length
local queueLength = exports['overflow_progressbar']:GetQueueLength()
```

Cancel progress:

```lua
-- Cancel current progress
exports['overflow_progressbar']:Cancel()

-- Cancel all (including queue)
exports['overflow_progressbar']:CancelAll()

-- Clear queue only (keep current)
exports['overflow_progressbar']:ClearQueue()
```

## Best Practices

### Duration Guidelines

- **Quick actions** (1-3 seconds): Picking up items, opening doors
- **Medium actions** (5-10 seconds): Crafting simple items, repairs
- **Long actions** (15-30 seconds): Complex crafting, mining
- **Very long actions** (30+ seconds): Use sparingly, always allow cancellation

### Label Guidelines

- Be descriptive and clear
- Use present continuous tense ("Repairing...", "Crafting...")
- Include item names when relevant ("Crafting Lockpick...")
- Keep it concise (under 30 characters)

### Icon Usage

Use Font Awesome icons to enhance visual feedback:

```lua
icon = "fas fa-wrench"     -- Repair
icon = "fas fa-hammer"     -- Crafting
icon = "fas fa-medkit"     -- Medical
icon = "fas fa-unlock"     -- Lockpicking
icon = "fas fa-spinner"    -- Generic loading
```

## Next Steps

- [Configuration](../configuration/overview.md) - Customize props and settings
- [API Reference](../api/exports.md) - Complete API documentation
- [Examples](../examples/basic.md) - Practical code examples
