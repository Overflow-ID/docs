# Keybind Configuration

Configure the key binding used to cancel progress bars.

## Default Keybind

By default, players can press **X** to cancel progress (if `canCancel = true`):

```lua
Config.CancelBinds = "X"
```

## Changing the Cancel Key

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

## Supported Keys

### Alphabet Keys

```lua
Config.CancelBinds = "A"  -- Any letter A-Z
```

### Function Keys

```lua
Config.CancelBinds = "F1"   -- F1 through F12
Config.CancelBinds = "F5"
```

### Special Keys

| Key | FiveM Name |
|-----|------------|
| ESC | `ESC` |
| Enter | `RETURN` |
| Backspace | `BACK` |
| Delete | `DELETE` |
| Space | `SPACE` |
| Tab | `TAB` |

### Arrow Keys

```lua
Config.CancelBinds = "UP"      -- Up arrow
Config.CancelBinds = "DOWN"    -- Down arrow
Config.CancelBinds = "LEFT"    -- Left arrow
Config.CancelBinds = "RIGHT"   -- Right arrow
```

### Number Keys

```lua
Config.CancelBinds = "1"  -- Number row 0-9
Config.CancelBinds = "5"
```

### Numpad Keys

```lua
Config.CancelBinds = "NUMPAD1"  -- Numpad 0-9
Config.CancelBinds = "NUMPAD5"
```

## Complete Key Reference

For a complete list of supported keys, see the [FiveM Key Mappings Documentation](https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/).

## Common Configurations

### Gaming Keyboards

```lua
-- Use a side mouse button or less common key
Config.CancelBinds = "MOUSE4"   -- Side mouse button
Config.CancelBinds = "F6"       -- Function key
```

### Accessibility

```lua
-- Use a larger, easier to reach key
Config.CancelBinds = "SPACE"    -- Spacebar
Config.CancelBinds = "RETURN"   -- Enter key
```

### Avoiding Conflicts

Choose a key that doesn't conflict with:
- Movement controls (WASD)
- Common game actions (F, E, etc.)
- Vehicle controls
- Other script keybinds

## Player Keybind Customization

Players can remap the cancel key using FiveM's built-in keybind system:

1. Press **ESC** in-game
2. Go to **Settings** → **Key Bindings** → **FiveM**
3. Find **"Cancel current progress bar"**
4. Press the key to rebind it

## Applying Changes

After changing the keybind in `config.lua`:

1. Save the file
2. Restart the resource:
   ```
   ensure overflow_progressbar
   ```

## Notes

- Key names are **case-insensitive** (`"x"` and `"X"` are the same)
- Some keys may not work on all keyboard layouts
- Test the key in-game to ensure it works properly
- Avoid using essential game controls as cancel keys

## Examples

### Standard Setup

```lua
Config.CancelBinds = "X"  -- Default, works well
```

### Alternative Setup

```lua
Config.CancelBinds = "BACK"  -- Backspace, less likely to conflict
```

### Advanced Setup

```lua
Config.CancelBinds = "F9"  -- Function key, rarely used
```

## Next Steps

- [Prop Configuration](props.md) - Configure props
- [Style Settings](style.md) - Configure progress bar styles
- [API Reference](../api/exports.md) - Learn about the canCancel option
