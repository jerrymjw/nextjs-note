# Script Optimization 脚本优化

### [Layout Scripts](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#layout-scripts)

要为多个路由加载第三方脚本，请导入 `next/script` 并将脚本直接包含在布局组件中：

```tsx
import Script from 'next/script';
 
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <>
      <section>{children}</section>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

当文件夹路由（例如 `dashboard/page.js` ） 或任何嵌套路由（例如 `dashboard/settings/page.js` ） 由用户访问。Next.js将确保脚本**仅加载一次**，即使用户在同一布局中的多个路由之间导航也是如此。

### [Application Scripts 应用程序脚本](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#application-scripts)

要为所有路由加载第三方脚本，请导入 `next/script` 并将脚本直接包含在根布局中：

```tsx
import Script from 'next/script';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script src="https://example.com/script.js" />
    </html>
  );
}
```

此脚本将在访问应用程序中的任何路由时加载并执行。Next.js将确保脚本只加载一次，即使用户在多个页面之间导航也是如此。

> 建议：我们建议仅在特定页面或布局中包含第三方脚本，以最大程度地减少对性能的任何不必要影响。

### [Strategy](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#strategy)

尽管 `next/script` 的默认行为允许您在任何页面或布局中加载第三方脚本，但您可以使用 `strategy` 属性微调其加载行为：

- `beforeInteractive` ：在任何 Next.js 代码之前和任何页面冻结发生之前加载脚本。
- `afterInteractive` ：（默认）提前加载脚本，但在页面上发生一些水化之后。
- `lazyOnload` ：稍后在浏览器空闲期间加载脚本。
- `worker` ： （实验性）在 Web worker 中加载脚本。

### [Offloading Scripts To A Web Worker (Experimental) ](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#offloading-scripts-to-a-web-worker-experimental)

> **警告**： `worker` 策略尚不稳定，尚不适用于 `app` 目录。请谨慎使用。

使用 `worker` 策略的脚本在具有 [Partytown](https://partytown.builder.io/) 的 Web worker 中卸载和执行。这可以通过将主线程专用于应用程序的其余部分来提高站点的性能。

此策略仍处于实验阶段，只有在 `next.config.js` 中启用了 `nextScriptWorkers` 标志时才能使用：

```js
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
};
```

然后，运行 `next` （通常为 `npm run dev` 或 `yarn dev` ），Next.js将指导您完成所需软件包的安装以完成设置：

```terminal
npm run dev
```

您将看到如下说明：请通过运行 `npm install @builder.io/partytown` 来安装 Partytown

设置完成后，定义 `strategy="worker"` 将自动在应用程序中实例化 Partytown，并将脚本卸载到 Web worker。

```tsx
import Script from 'next/script';
 
export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="worker" />
    </>
  );
}
```

在 Web worker 中加载第三方脚本时，需要考虑许多权衡。有关详细信息，请参阅 Partytown 的 [tradeoffs](https://partytown.builder.io/trade-offs) 文档。

### [Inline Scripts](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#inline-scripts)

脚本组件还支持内联脚本或未从外部文件加载的脚本。它们可以通过将 JavaScript 放在大括号内来编写：

```html
<Script id="show-banner">
  {`document.getElementById('banner').classList.remove('hidden')`}
</Script>
```

或者使用 `dangerouslySetInnerHTML` 属性：

```html
<Script
  id="show-banner"
  dangerouslySetInnerHTML={{
    __html: `document.getElementById('banner').classList.remove('hidden')`,
  }}
/>
```

> 警告：必须为内联脚本分配 `id` 属性，以便 Next.js 跟踪和优化脚本。

### [执行附加代码](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#executing-additional-code)

Event handlers(事件处理程序)可以与脚本组件一起使用，以便在发生特定事件后执行其他代码：

- `onLoad` ：在脚本加载完成后执行代码。
- `onReady` ：在脚本完成加载后和每次挂载组件时执行代码。
- `onError` ：如果脚本加载失败，则执行代码。

These handlers will only work when `next/script` is imported and used inside of a [Client Component](https://nextjs.org/docs/getting-started/react-essentials) where `"use client"` is defined as the first line of code:
仅当在导入`next/script`和客户端组件中使用时，这些处理程序才有效，其中 `"use client"` 定义为第一行代码：

```tsx
'use client';
 
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onLoad={() => {
          console.log('Script has loaded');
        }}
      />
    </>
  );
}
```

### [Additional Attributes 其他属性](https://nextjs.org/docs/app/building-your-application/optimizing/scripts#additional-attributes)

There are many DOM attributes that can be assigned to a `<script>` element that are not used by the Script component, like [`nonce`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) or [custom data attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*). Including any additional attributes will automatically forward it to the final, optimized `<script>` element that is included in the HTML.
有许多 DOM 属性可以分配给脚本组件不使用的 `<script>` 元素，例如  [`nonce`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) 或 [custom data attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*)。包含任何其他属性会自动将其转发到 HTML 中包含的最终优化 `<script>` 元素。

```tsx
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        id="example-script"
        nonce="XUENAJFW"
        data-test="script"
      />
    </>
  );
}
```

