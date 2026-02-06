<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style>
      :root {
        --bg-primary: #1a1a2e;
        --bg-secondary: #16213e;
        --bg-card: #0f3460;
        --accent-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        --text-primary: #ffffff;
        --text-secondary: #a0a0a0;
      }

      html,
      body {
        height: 100%;
        margin: 0;
        background: linear-gradient(
          135deg,
          var(--bg-primary) 0%,
          var(--bg-secondary) 100%
        );
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: 'Segoe UI', system-ui, sans-serif;
      }

      .card {
        background: var(--bg-card);
        border-radius: 20px;
        padding: 32px 48px;
        text-align: center;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        border: 1px solid rgba(255, 255, 255, 0.1);
        min-width: 200px;
      }

      .icon {
        font-size: 48px;
        margin-bottom: 16px;
      }

      .label {
        font-size: 12px;
        color: var(--text-secondary);
        text-transform: uppercase;
        letter-spacing: 2px;
        margin-bottom: 8px;
      }

      .value {
        font-size: 56px;
        font-weight: 700;
        background: var(--accent-gradient);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        line-height: 1.2;
        transition: transform 0.3s ease;
      }

      .value.updating {
        animation: value-pop 0.3s ease;
      }

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

      .unit {
        font-size: 18px;
        color: var(--text-secondary);
        margin-top: 4px;
      }

      .timestamp {
        font-size: 11px;
        color: rgba(255, 255, 255, 0.3);
        margin-top: 16px;
      }

      /* Loading state */
      .loading .value {
        opacity: 0.3;
        animation: pulse 1.5s infinite;
      }

      @keyframes pulse {
        0%,
        100% {
          opacity: 0.3;
        }
        50% {
          opacity: 0.6;
        }
      }
    </style>
  </head>
  <body>
    <div class="card loading" id="card">
      <div class="icon">🌡️</div>
      <div class="label" id="label">Temperature</div>
      <div class="value" id="value">--</div>
      <div class="unit" id="unit">°C</div>
      <div class="timestamp" id="timestamp">Waiting for data...</div>
    </div>

    <script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
    <script>
      const card = document.getElementById('card');
      const labelEl = document.getElementById('label');
      const valueEl = document.getElementById('value');
      const unitEl = document.getElementById('unit');
      const timestampEl = document.getElementById('timestamp');

      let config = null;

      const eraWidget = new EraWidget();
      eraWidget.init({
        needRealtimeConfigs: true,
        needHistoryConfigs: false,
        needActions: false,
        maxRealtimeConfigsCount: 1,
        minRealtimeConfigsCount: 1,
        mobileHeight: 250,

        onConfiguration: (configuration) => {
          config = configuration.realtime_configs[0];
          if (config) {
            labelEl.textContent = config.name || 'Sensor Value';
            unitEl.textContent = config.unit || '';
          }
        },

        onValues: (values) => {
          if (!config) return;

          card.classList.remove('loading');

          const data = values[config.id];
          const newValue = parseFloat(data.value).toFixed(1);

          // Animate value change
          valueEl.classList.add('updating');
          setTimeout(() => valueEl.classList.remove('updating'), 300);

          valueEl.textContent = newValue;

          // Update timestamp
          const now = new Date();
          timestampEl.textContent = `Updated ${now.toLocaleTimeString()}`;
        },
      });
    </script>
  </body>
</html>
