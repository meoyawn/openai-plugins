# Charts

Zaidan Chart uses Apache ECharts with SVG rendering. Do not use shadcn/Recharts examples as-is.

## Imports

```tsx
import type { EChartsCoreOption } from "echarts/core"

import {
  ChartContainer,
  type ChartConfig,
  chartColors,
  chartGridDefaults,
  chartTooltipDefaults,
  chartXAxisDefaults,
  chartYAxisDefaults,
} from "@/components/ui/chart"
```

## Basic chart

```tsx
const chartConfig: ChartConfig = {
  desktop: {
    label: "Desktop",
    color: "var(--chart-1)",
  },
  mobile: {
    label: "Mobile",
    color: "var(--chart-2)",
  },
}

const option: EChartsCoreOption = {
  color: chartColors,
  tooltip: {
    ...chartTooltipDefaults,
    trigger: "axis",
  },
  grid: chartGridDefaults,
  xAxis: {
    ...chartXAxisDefaults,
    type: "category",
    data: ["Jan", "Feb", "Mar"],
  },
  yAxis: {
    ...chartYAxisDefaults,
    type: "value",
  },
  series: [
    {
      type: "bar",
      name: "Desktop",
      data: [186, 305, 237],
      itemStyle: { borderRadius: [4, 4, 0, 0] },
    },
  ],
}

<ChartContainer class="min-h-64 w-full" config={chartConfig} option={option} />
```

## Reactive chart data

Use Solid memos for options derived from signals.

```tsx
const option = createMemo<EChartsCoreOption>(() => ({
  color: chartColors,
  tooltip: { ...chartTooltipDefaults, trigger: "axis" },
  xAxis: { ...chartXAxisDefaults, type: "category", data: labels() },
  yAxis: { ...chartYAxisDefaults, type: "value" },
  series: [{ type: "line", data: values(), smooth: true }],
}))

<ChartContainer option={option()} />
```

## Theme colors

Prefer CSS variables or `ChartConfig.theme` for light/dark colors.

```tsx
const chartConfig: ChartConfig = {
  revenue: {
    label: "Revenue",
    theme: {
      light: "oklch(0.646 0.222 41.116)",
      dark: "oklch(0.488 0.243 264.376)",
    },
  },
}
```

## Interactions

```tsx
<ChartContainer
  option={option}
  loading={loading()}
  eventHandlers={{
    click: (params) => {
      console.log(params)
    },
  }}
/>
```

If hover flickers when using CSS variables, set explicit `emphasis` colors or disable emphasis for the series.
