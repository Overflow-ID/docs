# Installation

This guide will walk you through installing the Advanced Progress Bar System on your FiveM server.

## Requirements

Before installing, ensure your server meets these requirements:

- **FiveM Server** - Latest artifact recommended
- **Game Build** - 2189 or higher
- **No dependencies** - This is a standalone resource

## Installation Methods

### Method 1: Manual Installation

1. **Download the resource**

   Clone or download the repository:
   ```bash
   git clone https://github.com/yourusername/overflow_progressbar.git
   ```

2. **Place in resources folder**

   Copy the `overflow_progressbar` folder to your server's resources directory:
   ```
   server-data/
   └── resources/
       └── [standalone]/
           └── overflow_progressbar/
   ```

3. **Add to server.cfg**

   Add the following line to your `server.cfg`:
   ```cfg
   ensure overflow_progressbar
   ```

4. **Restart server**

   Restart your server or use these commands:
   ```bash
   refresh
   restart overflow_progressbar
   ```

### Method 2: Direct Download

1. Download the latest release from [GitHub Releases](https://github.com/yourusername/overflow_progressbar/releases)
2. Extract the ZIP file to your resources folder
3. Add `ensure overflow_progressbar` to your `server.cfg`
4. Restart your server

## Verification

To verify the installation was successful:

1. **Check server console**

   Look for a message indicating the resource started successfully:
   ```
   Started resource overflow_progressbar
   ```

2. **Test in-game**

   Join your server and run this command in F8 console:
   ```lua
   exports['overflow_progressbar']:Progress({
       duration = 5000,
       label = "Testing..."
   })
   ```

   You should see a progress bar appear on your screen.

## File Structure

After installation, your directory should look like this:

```
overflow_progressbar/
├── client/
│   └── cl_main.lua
├── web/
│   ├── index.html
│   ├── scripts.js
│   └── styles.css
├── config.lua
├── fxmanifest.lua
└── README.md
```

## Troubleshooting Installation

### Resource not starting

**Issue:** Resource doesn't show as started in console

**Solutions:**
- Verify the folder name is exactly `overflow_progressbar`
- Check `fxmanifest.lua` exists and is not corrupted
- Ensure `ensure overflow_progressbar` is in `server.cfg`
- Check for typos in resource name

### UI not appearing

**Issue:** Progress bar doesn't show in-game

**Solutions:**
- Verify all files in `web/` folder are present
- Check browser console (F8) for JavaScript errors
- Ensure `ui_page` path is correct in `fxmanifest.lua`
- Clear FiveM cache and restart game

## Next Steps

Now that you've installed the resource, check out:

- [Quick Start](quick-start.md) - Learn basic usage
- [Configuration](../configuration/overview.md) - Customize props and settings
- [API Reference](../api/exports.md) - Explore all available functions
