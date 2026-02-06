# Multi-Toggle Example

Grid layout for multiple switches with dynamic icons and states.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta
			name="viewport"
			content="width=device-width, initial-scale=1"
		/>
		<style>
			:root {
				--bg-primary: #1a1a2e;
				--bg-secondary: #16213e;
				--bg-card: #0f3460;
				--accent-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
				--glow-color: rgba(102, 126, 234, 0.5);
				--text-primary: #ffffff;
				--text-secondary: #a0a0a0;
			}

			html,
			body {
				height: 100%;
				margin: 0;
				background: linear-gradient(135deg, var(--bg-primary) 0%, var(--bg-secondary) 100%);
				display: flex;
				justify-content: center;
				align-items: center;
				font-family: 'Segoe UI', system-ui, sans-serif;
			}

			.toggle-grid {
				display: grid;
				grid-template-columns: repeat(2, 1fr);
				gap: 16px;
				padding: 20px;
			}

			.toggle-card {
				background: var(--bg-card);
				border-radius: 16px;
				padding: 20px;
				text-align: center;
				border: 1px solid rgba(255, 255, 255, 0.1);
				transition: all 0.3s ease;
			}

			.toggle-card.active {
				box-shadow: 0 0 20px var(--glow-color);
				border-color: rgba(102, 126, 234, 0.5);
			}

			.toggle-icon {
				font-size: 28px;
				margin-bottom: 8px;
			}

			.toggle-label {
				font-size: 12px;
				color: var(--text-secondary);
				margin-bottom: 12px;
				text-transform: uppercase;
				letter-spacing: 1px;
			}

			/* Mini toggle */
			.mini-toggle {
				position: relative;
				width: 50px;
				height: 26px;
				margin: 0 auto;
				cursor: pointer;
			}

			.mini-toggle input {
				opacity: 0;
				width: 0;
				height: 0;
			}

			.mini-slider {
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				bottom: 0;
				background: #2d2d44;
				border-radius: 26px;
				transition: all 0.3s ease;
			}

			.mini-slider::before {
				content: '';
				position: absolute;
				height: 20px;
				width: 20px;
				left: 3px;
				bottom: 3px;
				background: white;
				border-radius: 50%;
				transition: all 0.3s ease;
			}

			input:checked + .mini-slider {
				background: var(--accent-gradient);
			}

			input:checked + .mini-slider::before {
				transform: translateX(24px);
			}
		</style>
	</head>
	<body>
		<div
			class="toggle-grid"
			id="toggleGrid"
		>
			<!-- Toggles will be generated dynamically -->
		</div>

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const toggleGrid = document.getElementById('toggleGrid');
			let configs = [];
			let actions = [];
			let states = {};
			const icons = ['💡', '🔌', '❄️', '🌡️', '💨', '🚿'];

			function createToggleCard(config, index) {
				const card = document.createElement('div');
				card.className = 'toggle-card';
				card.id = `card-${config.id}`;
				card.innerHTML = `
                <div class="toggle-icon">${icons[index % icons.length]}</div>
                <div class="toggle-label">${config.name || 'Device ' + (index + 1)}</div>
                <label class="mini-toggle">
                    <input type="checkbox" id="toggle-${config.id}">
                    <span class="mini-slider"></span>
                </label>
            `;
				const checkbox = card.querySelector('input');
				checkbox.addEventListener('click', () => {
					const currentState = states[config.id];
					const actionIndex = index * 2 + (currentState === 1 ? 1 : 0);
					if (actions[actionIndex]) {
						eraWidget.triggerAction(actions[actionIndex].action, null);
					}
				});
				return card;
			}

			const eraWidget = new EraWidget();
			eraWidget.init({
				needRealtimeConfigs: true,
				needHistoryConfigs: false,
				needActions: true,
				maxRealtimeConfigsCount: 4,
				minRealtimeConfigsCount: 1,
				maxActionsCount: 8,
				minActionsCount: 2,
				mobileHeight: 300,

				onConfiguration: (configuration) => {
					configs = configuration.realtime_configs;
					actions = configuration.actions;
					toggleGrid.innerHTML = '';
					configs.forEach((config, index) => {
						toggleGrid.appendChild(createToggleCard(config, index));
					});
				},

				onValues: (values) => {
					configs.forEach((config) => {
						const newState = values[config.id].value;
						states[config.id] = newState;
						const checkbox = document.getElementById(`toggle-${config.id}`);
						const card = document.getElementById(`card-${config.id}`);
						if (checkbox) checkbox.checked = newState === 1;
						if (card) card.classList.toggle('active', newState === 1);
					});
				},
			});
		</script>
	</body>
</html>
```
