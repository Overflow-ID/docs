# Advanced Progress Bar System

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![FiveM](https://img.shields.io/badge/FiveM-Ready-green.svg)

A powerful, feature-rich progress bar system for FiveM with animations, props, particles, and queue support.

## Overview

The Advanced Progress Bar System provides a modern, customizable UI component for displaying progress bars in your FiveM server. It includes support for player animations, prop attachments, particle effects, and sequential progress queuing.

## Key Features

- 🎨 **Modern UI Design** - Clean, responsive progress bar with customizable styling
- 🎭 **Animation Support** - Built-in support for GTA V animations and scenarios
- 🎪 **Prop System** - Attach props to player during progress (tools, items, etc.)
- ✨ **Particle Effects** - Add visual effects with particle systems
- 🔄 **Queue System** - Chain multiple progress bars sequentially
- ⚡ **Performance Optimized** - Minimal resource usage and smooth operation
- 🎮 **Control Disabling** - Selectively disable player controls during progress
- 🚫 **Interrupt Detection** - Auto-cancel on death, ragdoll, swimming, etc.
- ⌨️ **Cancellable Progress** - Allow players to cancel with keybind
- 🎯 **State Management** - Integrate with player state systems

## Requirements

- **FiveM Server** - Latest artifact recommended
- **Game Build** - 2189 or higher
- **No dependencies** - Standalone resource

## Quick Example

Here's a simple example to get you started:

```lua
exports['overflow_progressbar']:Progress({
    duration = 5000,
    label = "Loading...",
    canCancel = true,
    animation = {
        dict = "mini@repair",
        anim = "fixing_a_player"
    }
}, function(cancelled)
    if not cancelled then
        print("Progress completed!")
    end
end)
```

## Documentation Structure

This documentation is organized into the following sections:

- **[Getting Started](../getting-started/installation.md)** - Installation and basic usage
- **[Configuration](../configuration/overview.md)** - How to configure props and settings
- **[API Reference](../api/exports.md)** - Complete API documentation
- **[Examples](../examples/basic.md)** - Practical code examples for common use cases
- **[Advanced Topics](../advanced/queue.md)** - Queue system, particles, and best practices

## Getting Started

Ready to get started? Head to the [Installation Guide](../getting-started/installation.md) to begin!

## Support

Need help? We're here to assist:

- 🌐 **Website**: [https://www.overflow-dev.id](https://www.overflow-dev.id)
- 📧 **Email**: support@overflow-dev.id
- 🐛 **Issues**: Report bugs on GitHub

---

**Made with ❤️ by Overflow Development**
