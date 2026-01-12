# 💻 Code Examples

Comprehensive code examples for common use cases.

---

## Table of Contents

- [Basic Examples](#basic-examples)
- [Crafting System](#crafting-system)
- [Resource Gathering](#resource-gathering)
- [Vehicle System](#vehicle-system)
- [Medical System](#medical-system)
- [Criminal Activities](#criminal-activities)
- [Job Systems](#job-systems)
- [Shop/Store System](#shopstore-system)
- [Interaction System](#interaction-system)
- [Advanced Patterns](#advanced-patterns)

---

## Basic Examples

### Simple Progress Bar

```lua
-- Minimal example
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Loading..."
}, function(cancelled)
    if not cancelled then
        print("Done!")
    end
end)
```

### With Cancel Option

```lua
exports['overflow_progressbar']:Progress({
    duration = 10000,
    label = "Processing...",
    canCancel = true,  -- Player can press X to cancel
    icon = "fas fa-spinner"
}, function(cancelled)
    if cancelled then
        TriggerEvent('notify', 'Cancelled by user')
    else
        TriggerEvent('notify', 'Completed successfully')
    end
end)
```

### With Animation

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Working...",
    animation = {
        dict = "amb@world_human_hammering@male@base",
        anim = "base"
    }
})
```

### With Prop

```lua
exports['overflow_progressbar']:Progress({
    duration = 3000,
    label = "Drinking water...",
    prop = "water",  -- From config
    animation = {
        dict = "mp_player_intdrink",
        anim = "loop_bottle"
    }
})
```

---

## Crafting System

### Basic Crafting

```lua
-- client.lua
RegisterNetEvent('crafting:start', function(itemName, craftTime, recipe)
    -- Check if player has materials
    TriggerServerCallback('crafting:hasMaterials', recipe, function(hasMaterials)
        if not hasMaterials then
            TriggerEvent('notify', 'Missing materials!')
            return
        end
        
        exports['overflow_progressbar']:Progress({
            duration = craftTime,
            label = "Crafting " .. itemName .. "...",
            icon = "fas fa-hammer",
            canCancel = true,
            useWhileDead = false,
            disableControls = {
                move = true,
                combat = true,
                car = true
            },
            animation = {
                dict = "amb@world_human_hammering@male@base",
                anim = "base"
            },
            prop = {
                model = "prop_tool_hammer",
                bone = 28422,
                x = 0.0, y = 0.0, z = 0.0,
                xR = 0.0, yR = 0.0, zR = 0.0
            },
            onCancel = function()
                TriggerEvent('notify', 'Crafting cancelled')
            end
        }, function(cancelled)
            if not cancelled then
                TriggerServerEvent('crafting:complete', itemName, recipe)
            end
        end)
    end)
end)
```

### Multi-Step Crafting

```lua
RegisterNetEvent('crafting:complexItem', function()
    -- Step 1: Gather materials
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "1/4 - Gathering materials...",
        icon = "fas fa-box",
        canCancel = true,
        allowNested = true,
        animation = {
            scenario = "WORLD_HUMAN_CLIPBOARD"
        }
    })
    
    -- Step 2: Prepare workspace
    exports['overflow_progressbar']:Progress({
        duration = 3000,
        label = "2/4 - Preparing workspace...",
        icon = "fas fa-toolbox",
        canCancel = true,
        allowNested = true,
        animation = {
            dict = "amb@world_human_hammering@male@base",
            anim = "base"
        }
    })
    
    -- Step 3: Assemble
    exports['overflow_progressbar']:Progress({
        duration = 10000,
        label = "3/4 - Assembling components...",
        icon = "fas fa-wrench",
        canCancel = true,
        allowNested = true,
        disableControls = {
            move = true,
            combat = true
        },
        animation = {
            scenario = "WORLD_HUMAN_WELDING"
        },
        particles = {
            {
                assetName = "scr_reconstructionaccident",
                effectName = "scr_sparking_wires",
                bone = 28422,
                scale = 1.0
            }
        }
    })
    
    -- Step 4: Quality check
    exports['overflow_progressbar']:Progress({
        duration = 4000,
        label = "4/4 - Quality inspection...",
        icon = "fas fa-check-circle",
        canCancel = true,
        allowNested = true,
        animation = {
            scenario = "WORLD_HUMAN_CLIPBOARD"
        },
        onFinish = function()
            TriggerServerEvent('crafting:giveComplexItem')
        end
    }, function(cancelled)
        if not cancelled then
            TriggerEvent('notify', 'Item crafted successfully!')
        else
            TriggerEvent('notify', 'Crafting failed!')
        end
    end)
end)
```

---

## Resource Gathering

### Mining

```lua
RegisterNetEvent('mining:start', function(rockCoords)
    local playerPed = PlayerPedId()
    
    -- Face the rock
    TaskTurnPedToFaceCoord(playerPed, rockCoords.x, rockCoords.y, rockCoords.z, 1000)
    Wait(1000)
    
    exports['overflow_progressbar']:Progress({
        duration = 12000,
        label = "Mining ore...",
        icon = "fas fa-gem",
        canCancel = true,
        useWhileDead = false,
        allowRagdoll = false,
        allowFalling = false,
        disableControls = {
            move = false,
            combat = true,
            car = true
        },
        animation = {
            dict = "melee@large_wpn@streamed_core",
            anim = "ground_attack_on_spot",
            flag = 1  -- Repeat
        },
        prop = {
            model = "prop_tool_pickaxe",
            bone = 28422,
            x = 0.08, y = -0.05, z = -0.15,
            xR = 90.0, yR = 0.0, zR = 0.0
        },
        particles = {
            {
                assetName = "scr_reconstructionaccident",
                effectName = "scr_sparking_wires",
                bone = 28422,
                offset = { x = 0.0, y = 0.3, z = 0.0 },
                scale = 1.5
            }
        },
        onCancel = function()
            ClearPedTasks(playerPed)
        end
    }, function(cancelled)
        if not cancelled then
            -- Random ore amount
            local amount = math.random(1, 5)
            TriggerServerEvent('mining:giveOre', amount)
            TriggerEvent('notify', 'Mined ' .. amount .. ' ore')
        end
    end)
end)
```

### Woodcutting

```lua
RegisterNetEvent('woodcutting:chopTree', function(treeEntity)
    exports['overflow_progressbar']:Progress({
        duration = 8000,
        label = "Chopping tree...",
        icon = "fas fa-tree",
        canCancel = true,
        useWhileDead = false,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "melee@hatchet@streamed_core",
            anim = "plyr_front_takedown",
            flag = 1
        },
        prop = {
            model = "prop_w_me_hatchet",
            bone = 28422,
            x = 0.0, y = 0.0, z = 0.0,
            xR = 170.0, yR = 0.0, zR = -130.0
        }
    }, function(cancelled)
        if not cancelled then
            -- Delete tree and give wood
            DeleteEntity(treeEntity)
            TriggerServerEvent('woodcutting:giveWood', math.random(3, 7))
        end
    end)
end)
```

### Fishing

```lua
RegisterNetEvent('fishing:cast', function()
    local playerPed = PlayerPedId()
    
    exports['overflow_progressbar']:Progress({
        duration = math.random(10000, 20000),
        label = "Fishing...",
        icon = "fas fa-fish",
        canCancel = true,
        useWhileDead = false,
        allowSwimming = false,
        disableControls = {
            combat = true,
            car = true
        },
        animation = {
            scenario = "WORLD_HUMAN_STAND_FISHING"
        }
    }, function(cancelled)
        if not cancelled then
            -- Random catch chance
            if math.random(100) <= 70 then
                local fish = GetRandomFish()
                TriggerServerEvent('fishing:caught', fish)
                TriggerEvent('notify', 'Caught a ' .. fish .. '!')
            else
                TriggerEvent('notify', 'The fish got away...')
            end
        end
    end)
end)

function GetRandomFish()
    local fishes = {'Bass', 'Trout', 'Salmon', 'Tuna', 'Shark'}
    return fishes[math.random(#fishes)]
end
```

---

## Vehicle System

### Vehicle Repair

```lua
RegisterNetEvent('mechanic:repairVehicle', function(vehicle)
    local coords = GetEntityCoords(vehicle)
    local playerCoords = GetEntityCoords(PlayerPedId())
    
    if #(coords - playerCoords) > 5.0 then
        TriggerEvent('notify', 'Too far from vehicle!')
        return
    end
    
    exports['overflow_progressbar']:Progress({
        duration = 15000,
        label = "Repairing vehicle...",
        icon = "fas fa-wrench",
        canCancel = true,
        useWhileDead = false,
        disableControls = {
            move = false,
            combat = true,
            car = true
        },
        animation = {
            dict = "mini@repair",
            anim = "fixing_a_player"
        },
        prop = {
            model = "prop_tool_wrench",
            bone = 28422,
            x = 0.0, y = 0.0, z = 0.0,
            xR = 0.0, yR = 0.0, zR = 0.0
        },
        particles = {
            {
                assetName = "scr_reconstructionaccident",
                effectName = "scr_sparking_wires",
                bone = 28422,
                scale = 1.0
            }
        }
    }, function(cancelled)
        if not cancelled then
            SetVehicleFixed(vehicle)
            SetVehicleDeformationFixed(vehicle)
            SetVehicleUndriveable(vehicle, false)
            SetVehicleEngineOn(vehicle, true, true, false)
            TriggerEvent('notify', 'Vehicle repaired!')
        end
    end)
end)
```

### Lockpicking Vehicle

```lua
RegisterNetEvent('criminal:lockpickVehicle', function(vehicle)
    local playerPed = PlayerPedId()
    local lockpickTime = math.random(15000, 25000)
    local failChance = math.random(100)
    
    exports['overflow_progressbar']:Progress({
        duration = lockpickTime,
        label = "Lockpicking vehicle...",
        icon = "fas fa-unlock",
        canCancel = true,
        useWhileDead = false,
        allowRagdoll = false,
        allowFalling = false,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "veh@break_in@0h@p_m_one@",
            anim = "low_force_entry_ds",
            flag = 16
        },
        onCancel = function()
            -- Chance to break lockpick on cancel
            if math.random(100) < 30 then
                TriggerServerEvent('inventory:removeItem', 'lockpick', 1)
                TriggerEvent('notify', 'Lockpick broke!')
            end
        end
    }, function(cancelled)
        if not cancelled then
            if failChance < 80 then  -- 80% success rate
                SetVehicleDoorsLocked(vehicle, 1)
                SetVehicleDoorsLockedForAllPlayers(vehicle, false)
                TriggerEvent('notify', 'Vehicle unlocked!')
                
                -- Alert police
                TriggerServerEvent('police:vehicleTheftAlert', GetEntityCoords(vehicle))
            else
                TriggerEvent('notify', 'Lockpick failed!')
                TriggerServerEvent('inventory:removeItem', 'lockpick', 1)
            end
        end
    end)
end)
```

### Hotwiring

```lua
RegisterNetEvent('criminal:hotwireVehicle', function(vehicle)
    exports['overflow_progressbar']:Progress({
        duration = 10000,
        label = "Hotwiring vehicle...",
        icon = "fas fa-key",
        canCancel = true,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "anim@amb@clubhouse@tutorial@bkr_tut_ig3@",
            anim = "machinic_loop_mechandplayer",
            flag = 49
        }
    }, function(cancelled)
        if not cancelled then
            SetVehicleEngineOn(vehicle, true, false, true)
            TriggerEvent('notify', 'Vehicle hotwired!')
        end
    end)
end)
```

---

## Medical System

### Using Bandage

```lua
RegisterNetEvent('medical:useBandage', function()
    local playerPed = PlayerPedId()
    local currentHealth = GetEntityHealth(playerPed)
    
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "Applying bandage...",
        icon = "fas fa-band-aid",
        canCancel = true,
        useWhileDead = false,
        allowRagdoll = false,
        disableControls = {
            move = false,
            combat = true,
            car = true
        },
        animation = {
            dict = "amb@world_human_clipboard@male@idle_a",
            anim = "idle_c"
        },
        prop = {
            model = "prop_ld_health_pack",
            bone = 28422,
            x = 0.0, y = 0.0, z = 0.0,
            xR = 0.0, yR = 0.0, zR = 0.0
        }
    }, function(cancelled)
        if not cancelled then
            SetEntityHealth(playerPed, math.min(200, currentHealth + 30))
            TriggerServerEvent('inventory:removeItem', 'bandage', 1)
            TriggerEvent('notify', 'Bandage applied (+30 HP)')
        end
    end)
end)
```

### CPR/Revive

```lua
RegisterNetEvent('medical:performCPR', function(targetId)
    local targetPed = GetPlayerPed(GetPlayerFromServerId(targetId))
    local playerPed = PlayerPedId()
    
    -- Position player near target
    local targetCoords = GetEntityCoords(targetPed)
    TaskGoToCoordAnyMeans(playerPed, targetCoords.x, targetCoords.y, targetCoords.z, 1.0, 0, 0, 786603, 0xbf800000)
    Wait(2000)
    
    exports['overflow_progressbar']:Progress({
        duration = 12000,
        label = "Performing CPR...",
        icon = "fas fa-heartbeat",
        canCancel = true,
        useWhileDead = false,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "mini@cpr@char_a@cpr_str",
            anim = "cpr_pumpchest",
            flag = 49
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('medical:revivePlayer', targetId)
            TriggerEvent('notify', 'Patient revived!')
        else
            TriggerEvent('notify', 'CPR cancelled')
        end
        ClearPedTasks(playerPed)
    end)
end)
```

---

## Criminal Activities

### Safe Cracking

```lua
RegisterNetEvent('criminal:crackSafe', function(safeEntity)
    exports['overflow_progressbar']:Progress({
        duration = 20000,
        label = "Cracking safe...",
        icon = "fas fa-lock",
        canCancel = true,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "mini@safe_cracking",
            anim = "dial_turn_clock_normal"
        },
        onCancel = function()
            -- Trigger alarm on cancel
            TriggerServerEvent('police:silentAlarm', GetEntityCoords(safeEntity))
        end
    }, function(cancelled)
        if not cancelled then
            if math.random(100) <= 60 then  -- 60% success
                TriggerServerEvent('criminal:safeCracked', safeEntity)
                TriggerEvent('notify', 'Safe cracked!')
            else
                TriggerEvent('notify', 'Failed to crack safe')
                TriggerServerEvent('police:triggeredAlarm', GetEntityCoords(safeEntity))
            end
        end
    end)
end)
```

### Drug Processing

```lua
RegisterNetEvent('drugs:processCocaine', function(amount)
    exports['overflow_progressbar']:Progress({
        duration = amount * 2000,  -- 2 sec per unit
        label = "Processing cocaine (" .. amount .. " units)...",
        icon = "fas fa-flask",
        canCancel = true,
        useWhileDead = false,
        disableControls = {
            move = false,
            combat = true,
            car = true
        },
        animation = {
            scenario = "WORLD_HUMAN_CLIPBOARD"
        },
        particles = {
            {
                assetName = "core",
                effectName = "ent_sht_steam",
                bone = 60309,
                offset = { x = 0.0, y = 0.0, z = 0.5 },
                scale = 1.0
            }
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('drugs:giveProcessedCocaine', amount)
            TriggerEvent('notify', 'Processed ' .. amount .. ' cocaine')
        end
    end)
end)
```

---

## Job Systems

### Garbage Collector

```lua
RegisterNetEvent('job:collectGarbage', function(bagCoords)
    local playerPed = PlayerPedId()
    
    exports['overflow_progressbar']:Progress({
        duration = 3000,
        label = "Collecting garbage...",
        icon = "fas fa-trash",
        canCancel = true,
        disableControls = {
            move = false,
            combat = true,
            car = true
        },
        animation = {
            dict = "amb@world_human_janitor@male@idle_a",
            anim = "idle_a"
        },
        prop = {
            model = "prop_cs_rub_binbag_01",
            bone = 28422,
            x = 0.0, y = 0.0, z = -0.1,
            xR = 0.0, yR = 0.0, zR = 0.0
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('job:garbageCollected')
            TriggerEvent('notify', 'Garbage collected! +$50')
        end
    end)
end)
```

### Delivery Driver

```lua
RegisterNetEvent('job:deliverPackage', function(houseId)
    exports['overflow_progressbar']:Progress({
        duration = 5000,
        label = "Delivering package...",
        icon = "fas fa-box",
        canCancel = true,
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            dict = "mp_common",
            anim = "givetake1_a"
        },
        prop = {
            model = "prop_cs_cardbox_01",
            bone = 28422,
            x = 0.0, y = 0.0, z = 0.0,
            xR = 0.0, yR = 0.0, zR = 0.0
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('job:packageDelivered', houseId)
        end
    end)
end)
```

---

## Shop/Store System

### Buying Items

```lua
RegisterNetEvent('shop:purchaseItem', function(itemData)
    exports['overflow_progressbar']:Progress({
        duration = 2000,
        label = "Processing payment...",
        icon = "fas fa-credit-card",
        canCancel = false,  -- Cannot cancel purchase
        disableControls = {
            move = true,
            combat = true,
            car = true
        },
        animation = {
            scenario = "WORLD_HUMAN_STAND_MOBILE"
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('shop:completePurchase', itemData)
        end
    end)
end)
```

---

## Interaction System

### Door Unlocking (Keyfob)

```lua
RegisterNetEvent('doors:unlock', function(doorId)
    exports['overflow_progressbar']:Progress({
        duration = 1500,
        label = "Unlocking door...",
        icon = "fas fa-key",
        canCancel = true,
        animation = {
            dict = "anim@heists@keycard@",
            anim = "exit"
        }
    }, function(cancelled)
        if not cancelled then
            TriggerServerEvent('doors:setLocked', doorId, false)
            TriggerEvent('notify', 'Door unlocked')
        end
    end)
end)
```

---

## Advanced Patterns

### Dynamic Duration

```lua
RegisterNetEvent('mining:dynamicOre', function(oreType)
    local durations = {
        iron = 8000,
        gold = 12000,
        diamond = 20000
    }
    
    exports['overflow_progressbar']:Progress({
        duration = durations[oreType] or 10000,
        label = "Mining " .. oreType .. "...",
        icon = "fas fa-gem",
        canCancel = true,
        animation = {
            dict = "melee@large_wpn@streamed_core",
            anim = "ground_attack_on_spot"
        }
    })
end)
```

### Skill-Based Duration

```lua
RegisterNetEvent('crafting:skillBased', function(itemName)
    TriggerServerCallback('player:getSkillLevel', 'crafting', function(skillLevel)
        -- Higher skill = faster crafting
        local baseTime = 10000
        local reduction = skillLevel * 500  -- 500ms per level
        local finalTime = math.max(3000, baseTime - reduction)
        
        exports['overflow_progressbar']:Progress({
            duration = finalTime,
            label = "Crafting " .. itemName .. "...",
            icon = "fas fa-hammer",
            canCancel = true
        }, function(cancelled)
            if not cancelled then
                -- Also increase skill
                TriggerServerEvent('player:increaseSkill', 'crafting', 1)
            end
        end)
    end)
end)
```

### Interrupt on Damage

```lua
local progressActive = false

RegisterNetEvent('action:interruptible', function()
    progressActive = true
    
    exports['overflow_progressbar']:Progress({
        duration = 10000,
        label = "Doing something...",
        canCancel = true,
        onCancel = function()
            progressActive = false
        end
    }, function(cancelled)
        progressActive = false
    end)
end)

-- Monitor for damage
Citizen.CreateThread(function()
    local playerPed = PlayerPedId()
    local lastHealth = GetEntityHealth(playerPed)
    
    while true do
        Citizen.Wait(100)
        
        if progressActive then
            local currentHealth = GetEntityHealth(playerPed)
            if currentHealth < lastHealth then
                -- Took damage, cancel progress
                exports['overflow_progressbar']:Cancel()
                TriggerEvent('notify', 'Interrupted by damage!')
            end
            lastHealth = currentHealth
        end
    end
end)
```

---

<div align="center">

**[⬆ Back to Top](#-code-examples)**

</div>