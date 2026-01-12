# Overflow ProgressBar

![Version](https://img.shields.io/badge/version-1.0.0-green.svg)
![Framework](https://img.shields.io/badge/framework-Standalone-blue.svg)
![Performance](https://img.shields.io/badge/performance-0.00ms-brightgreen.svg)

A powerful, feature-rich progress bar system for FiveM with animations, props, particles, and queue support.

## Overview

The **Overflow ProgressBar** is a modern, highly customizable UI component for displaying progress bars in your FiveM server. It provides a seamless way to show players visual feedback during various in-game activities like crafting, gathering resources, repairing vehicles, and much more.

## Key Features

### 🎨 Modern UI Design
- Clean, responsive progress bar with two style options (bar and circle)
- Smooth animations and transitions
- Font Awesome icon support
- Customizable colors and themes
- Mobile-friendly responsive design

### 🎭 Animation System
- Support for GTA V animation dictionaries
- Scenario-based animations
- Configurable animation flags and blend settings
- Automatic cleanup on completion or cancellation

### 🎪 Prop Management
- Attach props to player bones during progress
- Pre-configured prop library in config
- Support for custom prop definitions
- Multiple props simultaneously
- Automatic prop cleanup and deletion

### ✨ Particle Effects
- Integrate particle effects with progress bars
- Custom particle positioning and scaling
- Multiple particles per progress
- Bone-attached particles with rotation control

### 🔄 Queue System
- Sequential progress bar execution
- Queue multiple actions with `allowNested`
- Visual queue counter in UI
- Automatic queue processing (FIFO)
- Individual queue management

### ⚡ Performance Optimized
- Minimal resource usage (~0.00ms idle)
- Efficient NUI communication
- Optimized client-side loops
- Proper resource cleanup
- No memory leaks

### 🎮 Control Management
- Selectively disable player controls
- Separate controls for mouse, movement, combat, vehicles
- Sprint-only control option
- Maintains immersion during activities

### 🚫 Smart Interrupts
- Auto-cancel on player death
- Ragdoll detection
- Swimming detection
- Falling detection
- Cuffed state detection
- Configurable per-progress

### ⌨️ Cancellable Progress
- Optional player cancellation with keybind
- Default key: X (configurable)
- Visual cancel indicator in UI
- `onCancel` callback support

### 🎯 State Management
- Automatic `invBusy` state handling
- Integration with inventory systems
- State persistence during queues
- Export functions for state checking

## Use Cases

Perfect for:
- ✅ Crafting and manufacturing systems
- ✅ Resource gathering (mining, woodcutting, fishing)
- ✅ Vehicle repair and modifications
- ✅ Medical actions (healing, reviving)
- ✅ Criminal activities (lockpicking, hacking)
- ✅ Job systems (garbage collection, deliveries)
- ✅ Shop purchases and transactions
- ✅ Any timed player action

## Requirements

- **FiveM Server**: Latest artifact recommended
- **Game Build**: 2189 or higher
- **Dependencies**: None (standalone)
- **Framework**: Works with ESX, QBCore, and custom frameworks

## Quick Example

```lua
-- Basic progress bar with animation
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Repairing vehicle...",
    icon = "fas fa-wrench",
    canCancel = true,
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
    disableControls = {
        move = true,
        combat = true,
        car = true
    },
    onFinish = function()
        -- Repair complete
        SetVehicleFixed(vehicle)
        print("Vehicle repaired!")
    end,
    onCancel = function()
        -- User cancelled
        print("Repair cancelled")
    end
})
```

## What Makes It Different?

### Compared to Other Progress Bars

| Feature | Overflow ProgressBar | Others |
|---------|---------------------|--------|
| Queue System | ✅ Built-in | ❌ Usually missing |
| Particle Effects | ✅ Full support | ❌ Rarely supported |
| Multiple Props | ✅ Unlimited | ⚠️ Usually single |
| Style Options | ✅ Bar & Circle | ⚠️ Usually fixed |
| Smart Interrupts | ✅ Comprehensive | ⚠️ Basic |
| State Management | ✅ Automatic | ❌ Manual |
| Performance | ✅ 0.00ms idle | ⚠️ Varies |
| Documentation | ✅ Extensive | ⚠️ Limited |

## Performance

Optimized for production servers:
- **Idle**: 0.00ms
- **Active (single)**: ~0.01ms
- **Active (queue)**: ~0.02ms
- **Memory**: <1MB

## Getting Started

Ready to integrate Overflow ProgressBar into your server?

1. **[Installation Guide](getting-started/installation.md)** - Get up and running in 5 minutes
2. **[Quick Start](getting-started/quick-start.md)** - Your first progress bar
3. **[Basic Concepts](getting-started/basic-concepts.md)** - Understand how it works

## Configuration

Customize the progress bar to match your server:

- **[Configuration Overview](configuration/overview.md)** - All config options
- **[Prop Configuration](configuration/props.md)** - Configure prop library
- **[Keybinds](configuration/keybinds.md)** - Customize cancel key
- **[Style Settings](configuration/style.md)** - Choose visual style

## API Reference

Complete API documentation:

- **[Exports](api/exports.md)** - All available exports
- **[Data Structures](api/data-structures.md)** - Configuration objects
- **[Animations](api/animations.md)** - Animation reference
- **[Props](api/props.md)** - Prop system
- **[Particles](api/particles.md)** - Particle effects
- **[Control Disabling](api/controls.md)** - Control options

## Examples

Practical examples for common scenarios:

- **[Basic Examples](examples/basic.md)** - Simple use cases

## Support

Need help?

- **[Common Issues](troubleshooting/common-issues.md)** - Solutions to common problems
- **[FAQ](troubleshooting/faq.md)** - Frequently asked questions
- **[Error Codes](troubleshooting/error-codes.md)** - Error reference

### Contact

- 🌐 **Website**: [https://www.overflow-dev.id](https://www.overflow-dev.id)
- 💬 **Discord**: [https://discord.gg/E8HNtBSZYB](https://discord.gg/E8HNtBSZYB)
- 📧 **Email**: support@overflow-dev.id

## License

This resource is provided under a **Commercial License**.

**Copyright © 2026 Overflow Development (@Overflow_ID)**

See the [Terms of Use](../README.md#terms-of-use) for details.

---

<div align="center">

**Ready to get started?**

[Installation Guide →](getting-started/installation.md)

**Made with ❤️ by Overflow Development**

</div>
