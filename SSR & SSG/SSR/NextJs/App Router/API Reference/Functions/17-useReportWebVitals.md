# useReportWebVitals

The `useReportWebVitals` hook allows you to report [Core Web Vitals](https://web.dev/vitals/), and can be used in combination with your analytics service.
`useReportWebVitals` hook允许您报告 [Core Web Vitals](https://web.dev/vitals/)，并可与您的分析服务结合使用。

```js
'use client';
 
import { useReportWebVitals } from 'next/web-vitals';
 
export function WebVitals() {
  useReportWebVitals((metric) => {
    console.log(metric);
  });
}
```

```js
import { WebVitals } from './_components/web-vitals';
 
export default function Layout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  );
}
```

> 由于 `useReportWebVitals` 挂钩需要 `"use client"` 指令，因此性能最高的方法是创建根布局导入的单独组件。这会将客户端边界专门限制在 `WebVitals` 组件上。

## [useReportWebVitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals#usereportwebvitals)

作为钩子的参数传递的 `metric` 对象由许多属性组成：

- `id` ：当前网页加载上下文中指标的唯一标识符
- `name` ：性能指标的名称。可能的值包括特定于 Web 应用程序的[Web Vitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals#web-vitals) （TTFB、FCP、LCP、FID、CLS）的名称。
- `delta` ：指标的当前值与上一个值之间的差值。该值通常以毫秒为单位，表示指标值随时间的变化。
- `entries` ：与指标关联的性能条目 [Performance Entries](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEntry)数组。这些条目提供有关与指标相关的性能事件的详细信息。
- `navigationType` ：指示触发指标集合的导航类型 [type of navigation](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming/type) 。可能的值包括 `"navigate"` 、 `"reload"` 、 `"back_forward"` 和 `"prerender"` 。
- `rating` ：指标值的定性评级，提供对性能的评估。可能的值为 `"good"` 、 `"needs-improvement"` 和 `"poor"` 。评级通常是通过将指标值与指示可接受或次优性能的预定义阈值进行比较来确定的。
- `value` ：性能条目的实际值或持续时间，通常以毫秒为单位。该值提供指标所跟踪的性能方面的定量度量。该值的来源取决于要测量的特定指标，并且可以来自各种性能 API [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance_API)s。

## [Web Vitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals#web-vitals)

网页指标[Web Vitals](https://web.dev/vitals/) 是一组有用的指标，旨在捕捉网页的用户体验。以下 Web 指标都包括在内：

- [Time to First Byte](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_first_byte) (TTFB) 到第一个字节的时间
- [First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint) (FCP) 首次内容绘制
- [Largest Contentful Paint](https://web.dev/lcp/) (LCP) 最大内容绘制
- [First Input Delay](https://web.dev/fid/) (FID) 首次输入延迟
- [Cumulative Layout Shift](https://web.dev/cls/) (CLS) 累积布局偏移
- [Interaction to Next Paint](https://web.dev/inp/) (INP) 与NEXT绘制的交互 

您可以使用 `name` 属性处理这些指标的所有结果。

```tsx
'use client';
 
import { useReportWebVitals } from 'next/web-vitals';
 
export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case 'FCP': {
        // handle FCP results
      }
      case 'LCP': {
        // handle LCP results
      }
      // ...
    }
  });
}
```

## [Usage on Vercel](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals#usage-on-vercel)

[Vercel Speed Insights](https://vercel.com/docs/concepts/speed-insights) 在Vercel部署上自动配置，不需要使用 `useReportWebVitals` 。此挂钩在本地开发中很有用，或者如果您使用的是其他分析服务。

## [将结果发送到外部系统](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals#sending-results-to-external-systems)

You can send results to any endpoint to measure and track real user performance on your site. For example:
您可以将结果发送到任何终endpoint，以衡量和跟踪您网站上的真实用户性能。例如：

```js
useReportWebVitals((metric) => {
  const body = JSON.stringify(metric);
  const url = 'https://example.com/analytics';
 
  // Use `navigator.sendBeacon()` if available, falling back to `fetch()`.
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, body);
  } else {
    fetch(url, { body, method: 'POST', keepalive: true });
  }
});
```

> **Note**: 如果您使用 [Google Analytics](https://analytics.google.com/analytics/web/), 则使用 `id` 值可以允许您手动构建指标分布（以计算百分位数等）。

```js
useReportWebVitals(metric => {
  // Use `window.gtag` if you initialized Google Analytics as this example:
  // https://github.com/vercel/next.js/blob/canary/examples/with-google-analytics/pages/_app.js
  window.gtag('event', metric.name, {
    value: Math.round(metric.name === 'CLS' ? metric.value * 1000 : metric.value), // values must be integers
    event_label: metric.id, // id unique to current page load
    non_interaction: true, // avoids affecting bounce rate.
  });
}
```

