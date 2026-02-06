# Control Panel Example

Full dashboard layout combining sensors, gauges, and device toggles.

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
				--accent-start: #667eea;
				--accent-end: #764ba2;
				--glow-color: rgba(102, 126, 234, 0.5);
				--success: #00d9a5;
				--text-primary: #ffffff;
				--text-secondary: #a0a0a0;
			}

			* {
				box-sizing: border-box;
			}

			html,
			body {
				min-height: 100%;
				margin: 0;
				background: linear-gradient(135deg, var(--bg-primary) 0%, var(--bg-secondary) 100%);
				font-family: 'Segoe UI', system-ui, sans-serif;
				padding: 16px;
			}

			.dashboard {
				display: grid;
				grid-template-columns: repeat(2, 1fr);
				gap: 16px;
				max-width: 400px;
				margin: 0 auto;
			}

			.card {
				background: var(--bg-card);
				border-radius: 16px;
				padding: 16px;
				border: 1px solid rgba(255, 255, 255, 0.1);
			}

			.card.wide {
				grid-column: span 2;
			}

			.card-header {
				display: flex;
				align-items: center;
				gap: 8px;
				margin-bottom: 12px;
			}

			.card-icon {
				font-size: 20px;
			}

			.card-title {
				font-size: 12px;
				color: var(--text-secondary);
				text-transform: uppercase;
				letter-spacing: 1px;
			}

			.value-lg {
				font-size: 36px;
				font-weight: 700;
				background: var(--accent-gradient);
				-webkit-background-clip: text;
				-webkit-text-fill-color: transparent;
				background-clip: text;
			}

			.unit {
				font-size: 14px;
				color: var(--text-secondary);
			}

			.mini-gauge {
				position: relative;
				width: 80px;
				height: 80px;
				margin: 0 auto;
			}

			.mini-gauge svg {
				transform: rotate(-90deg);
			}
			.mini-gauge .bg {
				stroke: #2d2d44;
			}
			.mini-gauge .progress {
				stroke: url(#miniGradient);
				stroke-dasharray: 226;
				stroke-dashoffset: 226;
				transition: stroke-dashoffset 0.5s ease;
			}

			.mini-gauge .center {
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -50%);
				font-size: 18px;
				font-weight: 700;
				color: var(--text-primary);
			}

			.control-row {
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 12px 0;
				border-bottom: 1px solid rgba(255, 255, 255, 0.1);
			}

			.control-row:last-child {
				border-bottom: none;
				padding-bottom: 0;
			}
			.control-label {
				font-size: 14px;
				color: var(--text-primary);
			}

			.switch {
				width: 44px;
				height: 24px;
				position: relative;
				cursor: pointer;
			}

			.switch input {
				opacity: 0;
				width: 0;
				height: 0;
			}
			.switch .slider {
				position: absolute;
				inset: 0;
				background: #2d2d44;
				border-radius: 24px;
				transition: 0.3s;
			}

			.switch .slider::before {
				content: '';
				position: absolute;
				width: 18px;
				height: 18px;
				left: 3px;
				bottom: 3px;
				background: white;
				border-radius: 50%;
				transition: 0.3s;
			}

			.switch input:checked + .slider {
				background: var(--accent-gradient);
			}
			.switch input:checked + .slider::before {
				transform: translateX(20px);
			}
		</style>
	</head>
	<body>
		<div class="dashboard">
			<div class="card">
				<div class="card-header">
					<span class="card-icon">🌡️</span>
					<span
						class="card-title"
						id="tempLabel"
						>Temperature</span
					>
				</div>
				<div
					class="value-lg"
					id="tempValue"
				>
					--
				</div>
				<div
					class="unit"
					id="tempUnit"
				>
					°C
				</div>
			</div>

			<div class="card">
				<div class="card-header">
					<span class="card-icon">💧</span>
					<span
						class="card-title"
						id="humidLabel"
						>Humidity</span
					>
				</div>
				<div class="mini-gauge">
					<svg
						width="80"
						height="80"
						viewBox="0 0 80 80"
					>
						<defs>
							<linearGradient
								id="miniGradient"
								x1="0%"
								y1="0%"
								x2="100%"
								y2="0%"
							>
								<stop
									offset="0%"
									stop-color="#667eea"
								/>
								<stop
									offset="100%"
									stop-color="#764ba2"
								/>
							</linearGradient>
						</defs>
						<circle
							class="bg"
							cx="40"
							cy="40"
							r="36"
							fill="none"
							stroke-width="6"
						/>
						<circle
							class="progress"
							id="humidProgress"
							cx="40"
							cy="40"
							r="36"
							fill="none"
							stroke-width="6"
							stroke-linecap="round"
						/>
					</svg>
					<div
						class="center"
						id="humidValue"
					>
						--%
					</div>
				</div>
			</div>

			<div class="card wide">
				<div class="card-header">
					<span class="card-icon">🎛️</span>
					<span class="card-title">Controls</span>
				</div>
				<div id="controlsContainer">
					<div class="control-row">
						<span
							class="control-label"
							id="control1Label"
							>Device 1</span
						>
						<label class="switch">
							<input
								type="checkbox"
								id="control1"
							/>
							<span class="slider"></span>
						</label>
					</div>
					<div class="control-row">
						<span
							class="control-label"
							id="control2Label"
							>Device 2</span
						>
						<label class="switch">
							<input
								type="checkbox"
								id="control2"
							/>
							<span class="slider"></span>
						</label>
					</div>
				</div>
			</div>
		</div>

		<script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
		<script>
			const tempValue = document.getElementById('tempValue');
			const tempLabel = document.getElementById('tempLabel');
			const humidValue = document.getElementById('humidValue');
			const humidLabel = document.getElementById('humidLabel');
			const humidProgress = document.getElementById('humidProgress');
			const control1 = document.getElementById('control1');
			const control2 = document.getElementById('control2');
			const control1Label = document.getElementById('control1Label');
			const control2Label = document.getElementById('control2Label');

			let configs = [];
			let actions = [];
			let controlStates = [null, null];
			const circumference = 2 * Math.PI * 36;

			const eraWidget = new EraWidget();
			eraWidget.init({
				needRealtimeConfigs: true,
				needHistoryConfigs: false,
				needActions: true,
				maxRealtimeConfigsCount: 4,
				minRealtimeConfigsCount: 2,
				maxActionsCount: 4,
				minActionsCount: 2,
				mobileHeight: 400,

				onConfiguration: (configuration) => {
					configs = configuration.realtime_configs;
					actions = configuration.actions;
					if (configs[0]) tempLabel.textContent = configs[0].name || 'Temperature';
					if (configs[1]) humidLabel.textContent = configs[1].name || 'Humidity';
					if (configs[2]) control1Label.textContent = configs[2].name || 'Device 1';
					if (configs[3]) control2Label.textContent = configs[3].name || 'Device 2';
				},

				onValues: (values) => {
					if (configs[0]) tempValue.textContent = parseFloat(values[configs[0].id].value).toFixed(1);
					if (configs[1]) {
						const humid = parseFloat(values[configs[1].id].value);
						humidValue.textContent = Math.round(humid) + '%';
						const offset = circumference - (Math.min(humid, 100) / 100) * circumference;
						humidProgress.style.strokeDashoffset = offset;
					}
					if (configs[2]) {
						const state1 = values[configs[2].id].value;
						if (controlStates[0] !== state1) {
							controlStates[0] = state1;
							control1.checked = state1 === 1;
						}
					}
					if (configs[3]) {
						const state2 = values[configs[3].id].value;
						if (controlStates[1] !== state2) {
							controlStates[1] = state2;
							control2.checked = state2 === 1;
						}
					}
				},
			});

			control1.addEventListener('click', () => {
				const actionIdx = controlStates[0] === 1 ? 1 : 0;
				if (actions[actionIdx]) eraWidget.triggerAction(actions[actionIdx].action, null);
			});

			control2.addEventListener('click', () => {
				const actionIdx = controlStates[1] === 1 ? 3 : 2;
				if (actions[actionIdx]) eraWidget.triggerAction(actions[actionIdx].action, null);
			});
		</script>
	</body>
</html>
```
