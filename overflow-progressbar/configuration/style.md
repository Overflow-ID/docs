# Style Configuration

Configure the visual style of the progress bar.

## Style Options

The progress bar system supports two visual styles:

### Bar Style (Default)

A horizontal progress bar:

```lua
Config.Style = "bar"
```

Features:
- Clean horizontal bar design
- Shows percentage clearly
- Works well with icons
- Traditional progress indicator

### Circle Style

A circular progress indicator:

```lua
Config.Style = "circle"
```

Features:
- Modern circular design
- Radial progress animation
- Compact display
- Elegant appearance

## Configuring Style

Edit the `Config.Style` value in `config.lua`:

```lua
Config = Config or {}

---Default progress bar style
---@type "bar"|"circle"
Config.Style = "bar"  -- Options: "bar" or "circle"
```

## Style Comparison

| Feature | Bar Style | Circle Style |
|---------|-----------|--------------|
| Visual Design | Horizontal bar | Circular ring |
| Space Usage | Wider | More compact |
| Readability | Very clear | Clear |
| Modern Look | Standard | Contemporary |
| Icon Support | Yes | Yes |
| Label Support | Yes | Yes |

## Choosing a Style

### Use Bar Style When:
- You prefer traditional progress bars
- You want maximum readability
- Your UI design uses horizontal elements
- You're familiar with standard progress indicators

### Use Circle Style When:
- You want a modern, sleek appearance
- You need to save screen space
- Your UI design uses circular elements
- You want a more elegant look

## Per-Progress Override

While `Config.Style` sets the default, individual progress calls can override it:

```lua
-- This would require custom implementation
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Loading...",
    mode = "circle"  -- Override default style
})
```

*Note: The above override feature may require custom NUI modifications.*

## Customizing Styles

To customize the appearance further, you can edit the CSS in:

```
web/styles.css
```

### Bar Style Customization

```css
/* Customize bar color */
.progress-bar {
    background-color: #your-color;
}

/* Customize bar height */
.progress-container {
    height: 10px;
}
```

### Circle Style Customization

```css
/* Customize circle color */
.circle-progress {
    stroke: #your-color;
}

/* Customize circle size */
.circle-container {
    width: 100px;
    height: 100px;
}
```

## Applying Changes

After changing the style:

1. Save `config.lua`
2. Restart the resource:
   ```
   restart overflow_progressbar
   ```

For CSS changes:
1. Save `web/styles.css`
2. Restart the resource
3. Players may need to reconnect or press **F5** in-game

## Advanced Customization

For advanced UI customization, you can modify:

- `web/index.html` - Structure
- `web/styles.css` - Styling
- `web/scripts.js` - Behavior

Example color schemes:

### Blue Theme
```css
.progress-bar {
    background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
}
```

### Green Theme
```css
.progress-bar {
    background: linear-gradient(90deg, #56ab2f 0%, #a8e063 100%);
}
```

### Red Theme
```css
.progress-bar {
    background: linear-gradient(90deg, #eb3349 0%, #f45c43 100%);
}
```

## Best Practices

1. **Consistency** - Stick to one style across your server
2. **Testing** - Test the style in-game before committing
3. **Backup** - Keep backups of working configurations
4. **Player Feedback** - Consider player preferences
5. **Theme Matching** - Match your server's overall UI theme

## Troubleshooting

### Style Not Changing

**Issue:** Progress bar style doesn't change after config edit

**Solutions:**
- Ensure you saved `config.lua`
- Restart the resource completely
- Check for typos in style name (must be exactly "bar" or "circle")
- Clear cache and reconnect to server

### Custom CSS Not Applied

**Issue:** CSS changes don't show in-game

**Solutions:**
- Hard refresh in-game (F5)
- Reconnect to server
- Check browser console (F8) for errors
- Verify CSS syntax is correct

## Next Steps

- [Prop Configuration](props.md) - Configure props
- [Keybinds](keybinds.md) - Configure keybinds
- [API Reference](../api/exports.md) - Explore progress options
