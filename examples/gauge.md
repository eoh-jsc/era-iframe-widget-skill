<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style>
      :root {
        --bg-primary: #1a1a2e;
        --bg-secondary: #16213e;
        --accent-start: #667eea;
        --accent-end: #764ba2;
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

      .gauge-container {
        position: relative;
        width: 200px;
        height: 200px;
      }

      .gauge-svg {
        transform: rotate(-90deg);
        width: 100%;
        height: 100%;
      }

      .gauge-bg {
        fill: none;
        stroke: #2d2d44;
        stroke-width: 12;
      }

      .gauge-progress {
        fill: none;
        stroke: url(#gradient);
        stroke-width: 12;
        stroke-linecap: round;
        stroke-dasharray: 440;
        stroke-dashoffset: 440;
        transition: stroke-dashoffset 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        filter: drop-shadow(0 0 8px rgba(102, 126, 234, 0.5));
      }

      .gauge-center {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
      }

      .gauge-value {
        font-size: 42px;
        font-weight: 700;
        background: linear-gradient(
          135deg,
          var(--accent-start),
          var(--accent-end)
        );
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        line-height: 1;
      }

      .gauge-unit {
        font-size: 14px;
        color: var(--text-secondary);
        margin-top: 4px;
      }

      .gauge-label {
        font-size: 12px;
        color: var(--text-secondary);
        text-transform: uppercase;
        letter-spacing: 2px;
        margin-top: 20px;
        text-align: center;
      }

      /* Loading animation */
      .loading .gauge-progress {
        animation: loading-spin 1.5s linear infinite;
        stroke-dashoffset: 330;
      }

      @keyframes loading-spin {
        0% {
          stroke-dashoffset: 330;
        }
        50% {
          stroke-dashoffset: 220;
        }
        100% {
          stroke-dashoffset: 330;
        }
      }
    </style>
  </head>
  <body>
    <div>
      <div class="gauge-container loading" id="gauge">
        <svg class="gauge-svg" viewBox="0 0 160 160">
          <defs>
            <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#667eea" />
              <stop offset="100%" stop-color="#764ba2" />
            </linearGradient>
          </defs>
          <circle class="gauge-bg" cx="80" cy="80" r="70" />
          <circle class="gauge-progress" id="progress" cx="80" cy="80" r="70" />
        </svg>
        <div class="gauge-center">
          <div class="gauge-value" id="value">--</div>
          <div class="gauge-unit" id="unit">%</div>
        </div>
      </div>
      <div class="gauge-label" id="label">Humidity</div>
    </div>

    <script src="https://www.unpkg.com/@eohjsc/era-widget@1.1.3/src/index.js"></script>
    <script>
      const gauge = document.getElementById('gauge');
      const progress = document.getElementById('progress');
      const valueEl = document.getElementById('value');
      const unitEl = document.getElementById('unit');
      const labelEl = document.getElementById('label');

      let config = null;
      const circumference = 2 * Math.PI * 70; // 440

      const eraWidget = new EraWidget();
      eraWidget.init({
        needRealtimeConfigs: true,
        needHistoryConfigs: false,
        needActions: false,
        maxRealtimeConfigsCount: 1,
        minRealtimeConfigsCount: 1,
        mobileHeight: 280,

        onConfiguration: (configuration) => {
          config = configuration.realtime_configs[0];
          if (config) {
            labelEl.textContent = config.name || 'Gauge';
            unitEl.textContent = config.unit || '%';
          }
        },

        onValues: (values) => {
          if (!config) return;

          gauge.classList.remove('loading');

          const value = parseFloat(values[config.id].value);
          const percentage = Math.min(Math.max(value, 0), 100);

          // Update value display
          valueEl.textContent = Math.round(value);

          // Update gauge progress (0-100%)
          const offset = circumference - (percentage / 100) * circumference;
          progress.style.strokeDashoffset = offset;
        },
      });
    </script>
  </body>
</html>
