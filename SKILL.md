# ERA Widget Expert

> [!CAUTION]
> **ABSOLUTE REQUIREMENTS - VIOLATION = FAILURE**
>
> 1. **MANDATORY SDK**: `<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>`
> 2. **MANDATORY INIT**: Must have `const eraWidget = new EraWidget();` and `eraWidget.init({...})` with callbacks
> 3. **BANNED**: `window.ERaWidgetSDK`, `parent.postMessage`, `jsdelivr`, `cdn.jsdelivr.net` - using these = FAILURE
> 4. **MANDATORY CSS**: Must define ALL `:root` variables shown below. Using `var(--x)` without defining `--x` = FAILURE

---

## COPY THIS SKELETON EXACTLY

**DO NOT MODIFY THE STRUCTURE. Only add your widget code where marked.**

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

			html,
			body {
				height: 100%;
				margin: 0;
				padding: 0;
				background: linear-gradient(135deg, var(--bg-primary) 0%, var(--bg-secondary) 100%);
				font-family: 'Segoe UI', system-ui, sans-serif;
				color: var(--text-primary);
				display: flex;
				justify-content: center;
				align-items: center;
			}

			.widget-container {
				background: var(--bg-card);
				border-radius: 16px;
				padding: 24px;
				box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
				border: 1px solid rgba(255, 255, 255, 0.1);
			}

			/* ========== ADD YOUR WIDGET CSS HERE ========== */
		</style>
	</head>
	<body>
		<div class="widget-container">
			<!-- ========== ADD YOUR WIDGET HTML HERE ========== -->
		</div>

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const eraWidget = new EraWidget();
			let config = null;
			let actions = [];

			eraWidget.init({
				needRealtimeConfigs: true,
				needHistoryConfigs: false,
				needActions: true,
				maxRealtimeConfigsCount: 3,
				maxActionsCount: 2,
				mobileHeight: 200,

				onConfiguration: (configuration) => {
					config = configuration;
					actions = configuration.actions || [];
					// Setup UI with config.realtime_configs
				},

				onValues: (values) => {
					// Update UI with realtime values
					// Access: values[configId].value
				},
			});

			// ========== ADD YOUR WIDGET JAVASCRIPT HERE ==========
			// To trigger action: eraWidget.triggerAction(actions[0]?.action, null);
		</script>
	</body>
</html>
```

---

## Quick Reference

### ERA Widget SDK CDN

```html
<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
```

### Basic Widget Structure

```html
<!DOCTYPE html>
<html>
	<head>
		<meta
			name="viewport"
			content="width=device-width, initial-scale=1"
		/>
		<style>
			/* Dark theme base */
			html,
			body {
				height: 100%;
				margin: 0;
				background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
				font-family: 'Segoe UI', system-ui, sans-serif;
			}
		</style>
	</head>
	<body>
		<!-- Widget HTML here -->

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const eraWidget = new EraWidget();
			eraWidget.init({
				needRealtimeConfigs: true,
				needHistoryConfigs: false,
				needActions: true,
				maxRealtimeConfigsCount: 3,
				maxActionsCount: 2,
				mobileHeight: 300,

				onConfiguration: (configuration) => {
					// Handle config from ERA platform
				},

				onValues: (values) => {
					// Handle realtime values
				},
			});
		</script>
	</body>
</html>
```

---

## Widget Types

### 1. Switch Button

Toggle ON/OFF control with visual feedback.

**Config requirements:**

- `needRealtimeConfigs: true` (1 config for state)
- `needActions: true` (2 actions: ON and OFF)

**Key elements:**

- Glow effect when ON
- Smooth transition animations
- Click handler calls `eraWidget.triggerAction()`

### 2. Value Display

Shows sensor value with icon and unit.

**Config requirements:**

- `needRealtimeConfigs: true` (1+ configs)
- `needActions: false`

**Key elements:**

- Large value text with unit
- Icon representing sensor type
- Color-coded thresholds (optional)
- Pulse animation on value change

### 3. Gauge / Meter

Circular progress indicator for percentage values.

**Config requirements:**

- `needRealtimeConfigs: true` (1 config)
- `needActions: false`

**Key elements:**

- SVG circle with stroke-dasharray animation
- Center value display
- Gradient stroke color

### 4. Action Button

Trigger a single action with confirmation.

**Config requirements:**

- `needRealtimeConfigs: false`
- `needActions: true` (1 action)

**Key elements:**

- Gradient button with hover effect
- Loading state during action
- Success/error feedback

### 5. Multi-Toggle

Multiple switches in a row/grid.

**Config requirements:**

- `needRealtimeConfigs: true` (multiple configs)
- `needActions: true` (multiple action pairs)

**Key elements:**

- Grid layout
- Individual toggle states
- Coordinated styling

### 6. Control Panel

Dashboard combining multiple widget types.

**Config requirements:**

- `needRealtimeConfigs: true`
- `needHistoryConfigs: true` (optional)
- `needActions: true`

**Key elements:**

- CSS Grid layout
- Card-based sections
- Responsive design

---

## Design System

### Color Palette (Dark Theme)

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

### Typography

```css
body {
	font-family: 'Segoe UI', 'Roboto', system-ui, sans-serif;
	color: var(--text-primary);
}

.value-large {
	font-size: 48px;
	font-weight: 700;
	background: var(--accent-gradient);
	-webkit-background-clip: text;
	-webkit-text-fill-color: transparent;
}

.label {
	font-size: 14px;
	color: var(--text-secondary);
	text-transform: uppercase;
	letter-spacing: 1px;
}
```

### Animations

```css
/* Glow pulse for active states */
@keyframes glow-pulse {
	0%,
	100% {
		box-shadow: 0 0 20px var(--glow-color);
	}
	50% {
		box-shadow: 0 0 40px var(--glow-color);
	}
}

/* Value update animation */
@keyframes value-pop {
	0% {
		transform: scale(1);
	}
	50% {
		transform: scale(1.1);
	}
	100% {
		transform: scale(1);
	}
}

/* Button press effect */
@keyframes button-press {
	0% {
		transform: scale(1);
	}
	50% {
		transform: scale(0.95);
	}
	100% {
		transform: scale(1);
	}
}
```

### Common Components

```css
/* Card container */
.card {
	background: var(--bg-card);
	border-radius: 16px;
	padding: 24px;
	box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
	backdrop-filter: blur(10px);
	border: 1px solid rgba(255, 255, 255, 0.1);
}

/* Gradient button */
.btn-gradient {
	background: var(--accent-gradient);
	border: none;
	border-radius: 12px;
	padding: 16px 32px;
	color: white;
	font-weight: 600;
	cursor: pointer;
	transition: all 0.3s ease;
}

.btn-gradient:hover {
	transform: translateY(-2px);
	box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
}

.btn-gradient:active {
	transform: translateY(0);
}
```

---

## ERA Widget SDK API

### Initialization Options

```javascript
eraWidget.init({
	// Required data types
	needRealtimeConfigs: true, // Need current values
	needHistoryConfigs: false, // Need historical data
	needActions: true, // Need action triggers

	// Limits
	maxRealtimeConfigsCount: 3,
	minRealtimeConfigsCount: 1,
	maxHistoryConfigsCount: 1,
	minHistoryConfigsCount: 0,
	maxActionsCount: 2,
	minActionsCount: 1,

	// Mobile app height (pixels)
	mobileHeight: 300,

	// Callbacks
	onConfiguration: (config) => {},
	onValues: (values) => {},
});
```

### Configuration Object

```javascript
onConfiguration: (configuration) => {
	// Realtime configs array
	const configs = configuration.realtime_configs;
	// configs[0].id - Config ID for value lookup
	// configs[0].name - Display name
	// configs[0].unit - Unit string

	// Actions array
	const actions = configuration.actions;
	// actions[0].action - Action object to trigger
	// actions[0].name - Display name
};
```

### Values Object

```javascript
onValues: (values) => {
	// Get value by config ID
	const value = values[configId].value;
	const timestamp = values[configId].timestamp;
};
```

### Trigger Action

```javascript
// Trigger action with optional payload
eraWidget.triggerAction(actionObject, payload);

// Example: Toggle based on state
if (currentState === 1) {
	eraWidget.triggerAction(actions[1]?.action, null); // OFF
} else {
	eraWidget.triggerAction(actions[0]?.action, null); // ON
}
```

---

## Generation Guidelines

When generating a widget:

1. **Always include viewport meta tag** for mobile responsiveness
2. **Use inline CSS** - Widget must be self-contained
3. **Dark theme by default** with the color palette above
4. **Add micro-animations** for premium feel
5. **Include Vietnamese comments** in JavaScript for clarity
6. **Handle loading states** before config is received
7. **Set appropriate mobileHeight** based on widget complexity
8. **Use semantic HTML** with meaningful IDs

### Output Format

**IMPORTANT: Return only the final raw HTML.**

- Do NOT include markdown formatting (no ```html blocks)
- Do NOT include code block markers or backticks
- Do NOT include comments or explanations
- Do NOT include any text before or after the HTML

The output must:

- Start with `<!DOCTYPE html>`
- End with `</html>`
- Be a complete, self-contained HTML file
- Can be saved directly as `.html` and opened in browser
- Works when deployed to GitHub Pages
- Integrates seamlessly with ERA iFrame Widget config

---

## Examples

See the `examples/` directory for complete templates:

- [switch-button.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/switch-button.html) - Modern toggle switch
- [value-display.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/value-display.html) - Sensor value card
- [gauge.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/gauge.html) - Circular gauge meter
- [action-button.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/action-button.html) - Action trigger button
- [multi-toggle.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/multi-toggle.html) - Multiple toggles
- [control-panel.html](https://raw.githubusercontent.com/eoh-jsc/era-iframe-widget-skill/main/examples/control-panel.html) - Full dashboard
