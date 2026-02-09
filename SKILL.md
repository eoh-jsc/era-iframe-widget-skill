---
name: era-widget-expert
description: Expert at creating premium ERA iFrame widgets for the ERA IoT platform.
  Ensures strict adherence to the ERA Widget SDK, design system, and mandatory structure.
  Generates self-contained HTML widgets with modern aesthetics and micro-animations.
metadata:
  model: sonnet
---

## Use this skill when

- Generating new custom widgets for the ERA IoT platform.
- Refactoring or debugging existing ERA iFrame widgets.
- Needing to ensure compatibility with `@eohjsc/era-widget@1.1.3`.
- Designing UI components that match the ERA dark theme design system.

## Do not use this skill when

- Creating standard web applications unrelated to the ERA platform.
- Using alternative SDKs or methods (e.g., `window.ERaWidgetSDK`, `parent.postMessage`).
- The task does not involve iFrame-based widgets for ERA.

## Instructions

- **Mandatory SDK**: Always include `<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>`.
- **Initialization**: Must have `const eraWidget = new EraWidget();` and `eraWidget.init({...})`.
- **Formatting**: Output ONLY the raw HTML code. Do not include markdown blocks, explanations, or text outside the `<html>` tags unless specifically asked for documentation.
- **Design**: Strictly follow the ERA Design System (CSS variable tokens for dark theme).
- **Process**:
  1. Identify widget type from prompt.
  2. Reference the appropriate template from `examples/`.
  3. Apply color and label customizations.
  4. Ensure all mandatory CSS variables are defined in `:root`.

## Absolute Requirements (VIOLATION = FAILURE)

1. **SDK**: `<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>`
2. **Mandatory Init**:
   ```javascript
   const eraWidget = new EraWidget();
   eraWidget.init({
       needRealtimeConfigs: true,
       needHistoryConfigs: false,
       needActions: true,
       onConfiguration: (config) => { ... },
       onValues: (values) => { ... },
   });
   ```
3. **CSS Tokens**: Must define all variables in the ERA Palette below.

## ERA Design System

### Color Palette (Mandatory Tokens)

```css
:root {
	--bg-primary: #1a1a2e;
	--bg-secondary: #16213e;
	--bg-card: #0f3460;
	--accent-primary: #e94560;
	--accent-secondary: #533483;
	--accent-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
	--text-primary: #ffffff;
	--text-secondary: #a0a0a0;
	--success: #00d9a5;
	--warning: #ffc107;
	--danger: #ff4757;
	--glow-color: rgba(102, 126, 234, 0.5);
}
```

### Typography & Styles

- **Font**: 'Segoe UI', system-ui, sans-serif.
- **Transitions**: Use smooth cubic-bezier animations for a premium feel.
- **Glow**: Active elements should have subtle `box-shadow` pulses using `--glow-color`.

## Widget Architecture

### Standard Skeleton

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta
			name="viewport"
			content="width=device-width, initial-scale=1.0"
		/>
		<style>
			/* ERA Design Tokens */
			:root { ... }

			/* Layout */
			html, body { ... }
			.widget-container { ... }

			/* Custom Widget CSS */
		</style>
	</head>
	<body>
		<div class="widget-container">
			<!-- Widget HTML -->
		</div>

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const eraWidget = new EraWidget();
			// ... Logic
		</script>
	</body>
</html>
```

## AI Generation Workflow

### Step 1: Fetch the Right Example

Refer to the `examples/` directory for foundation templates:

- `switch-button.md` -> Toggle/Switch controls
- `value-display.md` -> Sensor/Value cards
- `gauge.md` -> Circular meters
- `multi-toggle.md` -> Grid of controls
- `control-panel.md` -> Full dashboards

### Step 2: Adapt & Customize

- **Colors**: Override `--accent-gradient` or `--glow-color` if the user specifies a color.
- **Actions**: Map clicks to `eraWidget.triggerAction(actions[i].action, null)`.
- **Values**: Handle realtime updates in `onValues(values)`.

### Step 3: Performance & Feel

- **Loading**: Show a pulse animation before config/values arrive.
- **Mobile**: Adjust `mobileHeight` in `init()` based on widget height.

## Examples

Complete templates can be found in the [examples/](./examples) folder.
