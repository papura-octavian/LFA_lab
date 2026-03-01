<script>
  const data = [
    { label: 'Ian', value: 35 },
    { label: 'Feb', value: 52 },
    { label: 'Mar', value: 41 },
    { label: 'Apr', value: 68 },
    { label: 'Mai', value: 57 }
  ]

  const maxValue = Math.max(...data.map((item) => item.value))
  const chartHeight = 220
  const barWidth = 60
  const gap = 24
  const leftPadding = 28
  const baseLine = 260
  const chartWidth = leftPadding + data.length * (barWidth + gap)
</script>

<main class="wrap">
  <h1>Grafic simplu</h1>
  <svg viewBox={`0 0 ${chartWidth} 300`} role="img" aria-label="Grafic simplu pe luni">
    <line x1={leftPadding - 8} y1={baseLine} x2={chartWidth} y2={baseLine} class="axis" />

    {#each data as point, i}
      {@const x = leftPadding + i * (barWidth + gap)}
      {@const barHeight = (point.value / maxValue) * chartHeight}
      {@const y = baseLine - barHeight}

      <rect x={x} y={y} width={barWidth} height={barHeight} rx="8" class="bar" />
      <text x={x + barWidth / 2} y={baseLine + 22} class="label">{point.label}</text>
      <text x={x + barWidth / 2} y={y - 8} class="value">{point.value}</text>
    {/each}
  </svg>
</main>

<style>
  .wrap {
    width: min(900px, 92vw);
    margin: 40px auto;
  }

  h1 {
    margin: 0 0 16px;
    font-size: clamp(1.4rem, 2.5vw, 2rem);
    color: #1f2937;
  }

  svg {
    width: 100%;
    height: auto;
    background: #f8fafc;
    border: 1px solid #e2e8f0;
    border-radius: 14px;
    padding: 10px;
    box-sizing: border-box;
  }

  .axis {
    stroke: #94a3b8;
    stroke-width: 1.5;
  }

  .bar {
    fill: #0ea5e9;
  }

  .label,
  .value {
    text-anchor: middle;
    font-family: ui-sans-serif, system-ui, sans-serif;
    fill: #334155;
  }

  .label {
    font-size: 14px;
  }

  .value {
    font-size: 12px;
    font-weight: 600;
  }
</style>
