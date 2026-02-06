# Switch Button Example

Modern toggle switch with visual feedback and ERa Action trigger.

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
				--accent-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
				--success: #00d9a5;
				--glow-color: rgba(102, 126, 234, 0.6);
			}

			html,
			body {
				height: 100%;
				margin: 0;
				background: linear-gradient(135deg, var(--bg-primary) 0%, var(--bg-secondary) 100%);
				display: flex;
				justify-content: center;
				align-items: center;
			}

			.switch-container {
				display: flex;
				flex-direction: column;
				align-items: center;
				gap: 20px;
			}

			.label {
				font-family: 'Segoe UI', system-ui, sans-serif;
				font-size: 14px;
				color: #a0a0a0;
				text-transform: uppercase;
				letter-spacing: 2px;
			}

			/* Modern Toggle Switch */
			.toggle-switch {
				position: relative;
				width: 80px;
				height: 40px;
				cursor: pointer;
			}

			.toggle-switch input {
				opacity: 0;
				width: 0;
				height: 0;
			}

			.slider {
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				bottom: 0;
				background: #2d2d44;
				border-radius: 40px;
				transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
				box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.3);
			}

			.slider::before {
				content: '';
				position: absolute;
				height: 32px;
				width: 32px;
				left: 4px;
				bottom: 4px;
				background: linear-gradient(135deg, #f5f5f5 0%, #e0e0e0 100%);
				border-radius: 50%;
				transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
				box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
			}

			input:checked + .slider {
				background: var(--accent-gradient);
				box-shadow: 0 0 30px var(--glow-color);
			}

			input:checked + .slider::before {
				transform: translateX(40px);
				background: linear-gradient(135deg, #ffffff 0%, #f0f0f0 100%);
			}

			/* Glow animation when ON */
			@keyframes glow-pulse {
				0%,
				100% {
					box-shadow: 0 0 20px var(--glow-color);
				}
				50% {
					box-shadow:
						0 0 40px var(--glow-color),
						0 0 60px var(--glow-color);
				}
			}

			input:checked + .slider {
				animation: glow-pulse 2s infinite;
			}

			.status {
				font-family: 'Segoe UI', system-ui, sans-serif;
				font-size: 24px;
				font-weight: 700;
				background: var(--accent-gradient);
				-webkit-background-clip: text;
				-webkit-text-fill-color: transparent;
				background-clip: text;
				opacity: 0.5;
				transition: opacity 0.3s ease;
			}

			.status.active {
				opacity: 1;
			}
		</style>
	</head>
	<body>
		<div class="switch-container">
			<span class="label">LED Control</span>

			<label class="toggle-switch">
				<input
					type="checkbox"
					id="toggle"
				/>
				<span class="slider"></span>
			</label>

			<span
				class="status"
				id="status"
				>OFF</span
			>
		</div>

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const toggle = document.getElementById('toggle');
			const status = document.getElementById('status');

			let configLed = null;
			let currentState = null;
			let actions = [];

			const eraWidget = new EraWidget();
			eraWidget.init({
				needRealtimeConfigs: true,
				needHistoryConfigs: false,
				needActions: true,
				maxRealtimeConfigsCount: 1,
				maxActionsCount: 2,
				minActionsCount: 2,
				mobileHeight: 200,

				onConfiguration: (configuration) => {
					configLed = configuration.realtime_configs[0];
					actions = configuration.actions;
				},

				onValues: (values) => {
					if (!configLed) return;

					const newState = values[configLed.id].value;

					if (currentState !== newState) {
						currentState = newState;
						toggle.checked = newState === 1;
						status.textContent = newState === 1 ? 'ON' : 'OFF';
						status.classList.toggle('active', newState === 1);
					}
				},
			});

			toggle.addEventListener('click', () => {
				if (currentState === 1) {
					eraWidget.triggerAction(actions[1]?.action, null);
				} else {
					eraWidget.triggerAction(actions[0]?.action, null);
				}
			});
		</script>
	</body>
</html>
```
