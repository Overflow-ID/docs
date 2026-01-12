# Configuration Overview

The Advanced Progress Bar System is highly configurable through the `config.lua` file.

## File Location

```
overflow_progressbar/
└── config.lua
```

## Configuration Structure

The configuration file contains two main sections:

### 1. Props List

Defines all available props that can be attached to players during progress:

```lua
Config.PropsList = {
    ["propname"] = {
        model = "prop_model_name",
        bone = 28422,
        x = 0.0, y = 0.0, z = 0.0,
        xR = 0.0, yR = 0.0, zR = 0.0
    }
}
```

### 2. Keybinds

Defines the key used to cancel progress:

```lua
Config.CancelBinds = "X"
```

### 3. Style Settings

Defines the default progress bar style:

```lua
Config.Style = "bar"  -- Options: "bar" or "circle"
```

## Default Configuration

The system comes pre-configured with:

- **14 built-in props** including:
  - Beverages (water, coffee, beer, etc.)
  - Food items (hamburger, sandwich, etc.)
- **X key** for cancellation
- **Bar style** for progress display

## Customization Options

You can customize:

- ✅ Add new props
- ✅ Modify existing props
- ✅ Change cancel key
- ✅ Change progress bar style
- ✅ Adjust prop positions and rotations

## Quick Examples

### Adding a New Prop

```lua
Config.PropsList["hammer"] = {
    model = "prop_tool_hammer",
    bone = 28422,
    x = 0.0, y = 0.0, z = 0.0,
    xR = 0.0, yR = 0.0, zR = 0.0
}
```

### Changing Cancel Key

```lua
Config.CancelBinds = "E"  -- Change to E key
```

### Changing Progress Style

```lua
Config.Style = "circle"  -- Use circle style instead of bar
```

## Configuration Sections

- [Prop Configuration](props.md) - Detailed guide for configuring props
- [Keybinds](keybinds.md) - Keybind configuration options
- [Style Settings](style.md) - Progress bar style options

## Best Practices

1. **Backup** your config before making changes
2. **Test** props in-game after adding/modifying
3. **Group** similar props together with comments
4. **Document** custom props with comments
5. **Version control** your config changes

## Applying Changes

After modifying the configuration:

1. Save the `config.lua` file
2. Restart the resource:
   ```
   restart overflow_progressbar
   ```

Changes will take effect immediately for all players.

## Next Steps

- [Prop Configuration](props.md) - Learn how to configure props
- [Keybinds](keybinds.md) - Configure keybinds
- [Style Settings](style.md) - Configure progress bar styles
