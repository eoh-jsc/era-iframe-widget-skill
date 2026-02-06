# ERA iFrame Widget Skill

An AI skill for generating custom iFrame widgets for the **E-Ra IoT platform**. Use this as an online skill - no download required!

## 🚀 How to Use

Simply include this repository link in your prompt to any AI assistant:

### Option 1: Direct Skill Reference

```
Read the skill from: https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/refs/heads/main/SKILL.md

Then create a widget that displays temperature with a gauge meter.
```

### Option 2: Repository Link

```
Use https://github.com/eoh-jsc/era-iframe-widget-skill to create a switch button widget for controlling lights.
```

### Option 3: Simple Reference

```
@era-iframe-widget-skill: Create a multi-toggle widget for 4 switches
```

## 📦 What's Included

| File        | Description                                                                   |
| ----------- | ----------------------------------------------------------------------------- |
| `SKILL.md`  | Complete skill instructions with SDK reference, design system, and guidelines |
| `examples/` | 6 ready-to-use widget templates                                               |

## 🎨 Widget Types

- **Switch Button** - Toggle ON/OFF with visual feedback
- **Value Display** - Show sensor values with icons and units
- **Gauge/Meter** - Circular progress for percentage values
- **Action Button** - Trigger actions with confirmation
- **Multi-Toggle** - Multiple switches in grid layout
- **Control Panel** - Full dashboard combining multiple widgets

## 📁 Examples

| Widget                                       | Preview               |
| -------------------------------------------- | --------------------- |
| [Switch Button](examples/switch-button.html) | Modern toggle switch  |
| [Value Display](examples/value-display.html) | Sensor value card     |
| [Gauge](examples/gauge.html)                 | Circular gauge meter  |
| [Action Button](examples/action-button.html) | Action trigger button |
| [Multi-Toggle](examples/multi-toggle.html)   | Multiple toggles grid |
| [Control Panel](examples/control-panel.html) | Full dashboard        |

## 🔧 ERA Widget SDK

All widgets use the official ERA Widget SDK:

```html
<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
```

## 📄 License

MIT License - Feel free to use and modify!
