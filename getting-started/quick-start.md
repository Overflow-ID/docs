# Quick Start

Get up and running with the Advanced Progress Bar System in minutes.

## Basic Usage

The simplest way to use the progress bar:

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,        -- 5 seconds
    label = "Loading..."    -- Display text
})
```

## With Callback

Handle completion or cancellation:

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Processing..."
}, function(cancelled)
    if cancelled then
        print("User cancelled the progress")
    else
        print("Progress completed successfully!")
    end
end)
```

## Cancellable Progress

Allow players to cancel with a keybind (default: X):

```lua
exports['overflow_progressbar']:Progress({
    duration = 10000,
    label = "Hold X to cancel",
    canCancel = true
}, function(cancelled)
    if cancelled then
        TriggerEvent('notify', 'Action cancelled')
    end
end)
```

## With Animation

Add player animations to enhance immersion:

```lua
exports['overflow_progressbar']:Progress({
    duration = 8000,
    label = "Repairing vehicle...",
    animation = {
        dict = "mini@repair",
        anim = "fixing_a_player"
    }
})
```

## With Props

Attach props to the player:

```lua
exports['overflow_progressbar']:Progress({
    duration = 3000,
    label = "Drinking water...",
    prop = "water",  -- Uses prop from config
    animation = {
        dict = "mp_player_intdrink",
        anim = "loop_bottle"
    }
})
```

## Disable Controls

Prevent player actions during progress:

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Crafting...",
    disableControls = {
        move = true,     -- Can't move
        combat = true,   -- Can't shoot/fight
        car = true       -- Can't drive
    }
})
```

## Complete Example

Here's a complete example combining multiple features:

```lua
RegisterNetEvent('crafting:startCrafting', function(itemName)
    exports['overflow_progressbar']:Progress({
        -- Basic settings
        duration = 8000,
        label = "Crafting " .. itemName .. "...",
        icon = "fas fa-hammer",
        canCancel = true,

        -- Interrupt conditions
        useWhileDead = false,
        allowRagdoll = false,

        -- Controls
        disableControls = {
            move = true,
            combat = true,
            car = true
        },

        -- Animation
        animation = {
            dict = "amb@world_human_hammering@male@base",
            anim = "base"
        },

        -- Prop
        prop = {
            model = "prop_tool_hammer",
            bone = 28422,
            x = 0.0, y = 0.0, z = 0.0,
            xR = 0.0, yR = 0.0, zR = 0.0
        },

        -- Callbacks
        onFinish = function()
            print("Crafting finished!")
        end,
        onCancel = function()
            print("Crafting cancelled!")
        end

    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('crafting:giveItem', itemName)
            TriggerEvent('notify', 'Crafted ' .. itemName .. '!')
        else
            TriggerEvent('notify', 'Crafting cancelled')
        end
    end)
end)
```

## Testing Commands

Create a test command to try it out:

```lua
RegisterCommand('testprogress', function()
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "Testing progress bar...",
        canCancel = true,
        icon = "fas fa-spinner",
        animation = {
            scenario = "WORLD_HUMAN_CLIPBOARD"
        }
    }, function(cancelled)
        if cancelled then
            print("Test cancelled")
        else
            print("Test completed!")
        end
    end)
end)
```

Run `/testprogress` in-game to see it in action!

## Common Patterns

### Trigger from Server

Server-side:
```lua
TriggerClientEvent('myScript:showProgress', source, itemName, duration)
```

Client-side:
```lua
RegisterNetEvent('myScript:showProgress', function(itemName, duration)
    exports['overflow_progressbar']:Progress({
        duration = duration,
        label = "Processing " .. itemName .. "..."
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('myScript:completed', itemName)
        end
    end)
end)
```

### Check if Busy

Prevent multiple progress bars:

```lua
RegisterCommand('dosomething', function()
    if exports['overflow_progressbar']:IsProgressActive() then
        TriggerEvent('notify', 'You are already busy!')
        return
    end

    -- Start new progress
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "Doing something..."
    })
end)
```

## Next Steps

- [Basic Concepts](basic-concepts.md) - Understanding key concepts
- [Configuration](../configuration/overview.md) - Customize props and settings
- [API Reference](../api/exports.md) - Explore all available options
- [Examples](../examples/basic.md) - More practical examples
