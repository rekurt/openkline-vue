<div align="center">

# @rekurt/openkline-vue

### Vue 3 wrapper for [OpenKline](https://github.com/rekurt/openkline)

An idiomatic `<OHLCVChart>` component and a `useOHLCVChart` composable — full API
parity with the framework-agnostic [`@rekurt/openkline-core`](https://github.com/rekurt/openkline)
charting engine.

[![CI](https://github.com/rekurt/openkline-vue/actions/workflows/ci.yml/badge.svg)](https://github.com/rekurt/openkline-vue/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e.svg)](./LICENSE)
[![Vue](https://img.shields.io/badge/Vue-3.3%2B-42b883.svg)](https://vuejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178c6.svg)](./tsconfig.json)

</div>

---

## Ecosystem

| Package | Repository | Role |
| --- | --- | --- |
| `@rekurt/openkline-core` | [rekurt/openkline](https://github.com/rekurt/openkline) | Engine: rendering, data, interaction, indicators, drawings. |
| `@rekurt/openkline-react` | [rekurt/openkline-react](https://github.com/rekurt/openkline-react) | React 18+/19 wrapper. |
| **`@rekurt/openkline-vue`** | **this repo** | **Vue 3 wrapper.** |

---

## Install

```bash
npm install @rekurt/openkline-core @rekurt/openkline-vue
```

> **Pre-release note:** until the packages are published to npm, this repo
> vendors a built core tarball at `vendor/rekurt-openkline-core.tgz` so
> `npm install` works out of the box. Refresh it with `npm run update:core`.

---

## Usage

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { OHLCVChart } from '@rekurt/openkline-vue';
import type { Candle, IndicatorConfig } from '@rekurt/openkline-core';

defineProps<{ candles: Candle[] }>();

const chartRef = ref<InstanceType<typeof OHLCVChart>>();
const indicators = ref<IndicatorConfig[]>([
  { type: 'sma', period: 20 },
  { type: 'rsi', period: 14 },
]);

function goLive() {
  chartRef.value?.goToLive();
}
</script>

<template>
  <div style="width: 100%; height: 600px">
    <OHLCVChart
      ref="chartRef"
      symbol="BTC/USDT"
      resolution="1H"
      :data="candles"
      theme="auto"
      chart-type="candles"
      v-model:indicators="indicators"
      @hover="(info) => console.log('hovered', info?.index)"
      @error="(err) => console.error('[openkline]', err)"
    />
    <button @click="goLive">Go live</button>
  </div>
</template>
```

The `useOHLCVChart` composable is exported for callers that want to manage the
chart instance imperatively instead of through the component.

---

## Why a wrapper, not a rewrite

All chart logic lives in `@rekurt/openkline-core`. This package owns only the
Vue glue — mounting the canvas, syncing reactive props to engine calls, and
exposing the instance. That is why it tracks the core's features automatically
and stays at **full API parity** with the vanilla and React entry points.

---

## Development

```bash
npm install          # installs deps incl. the vendored core tarball
npm run build        # tsup → dist/ (esm + cjs + d.ts)
npm test             # vitest (jsdom)
npm run lint         # ESLint, --max-warnings 0
npm run typecheck    # strict tsc (run after build — example needs dist/)
npm run dev:example  # vite demo app → http://localhost:5175
npm run update:core  # refresh vendor/rekurt-openkline-core.tgz from the monorepo
```

The `example/` workspace is a full-featured Vite demo (drawing tools,
indicators, live simulation, Heikin-Ashi/Renko transforms, PNG export).

---

## License

[MIT](./LICENSE) © OpenKline contributors
