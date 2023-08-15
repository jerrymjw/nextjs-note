# loading.js

加载文件可以创建基于悬念[Suspense](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)的即时加载状态。

默认情况下，此文件是服务器组件 - 但也可以通过 `"use client"` 指令用作客户端组件。

```tsx
export default function Loading() {
  // Or a custom loading skeleton component
  return 'Loading...';
}
```

加载 UI 组件不接受任何参数。

