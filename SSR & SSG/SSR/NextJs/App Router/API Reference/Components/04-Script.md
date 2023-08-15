# <Script>

此 API 参考将帮助您了解如何使用脚本组件可用的 props。

```tsx
import Script from 'next/script';
 
export default function Dashboard() {
  return (
    <>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

## [Props](https://nextjs.org/docs/app/api-reference/components/script#props)

以下是可用于Scipt组件的属性摘要：

![image-20230526135906569](/Users/jerry/Library/Application Support/typora-user-images/image-20230526135906569.png)

## [Required Props](https://nextjs.org/docs/app/api-reference/components/script#required-props)

`<Script />` 组件需要以下属性。

### [`src`](https://nextjs.org/docs/app/api-reference/components/script#src)

指定外部脚本的 URL 的路径字符串。这可以是绝对外部 URL 或内部路径。除非使用内联脚本，否则 `src` 属性是必需的。

## [Optional Props](https://nextjs.org/docs/app/api-reference/components/script#optional-props)

The `<Script />` component accepts a number of additional properties beyond those which are required.
`<Script />` 组件接受除所需属性之外的许多其他属性。

### [`strategy`](https://nextjs.org/docs/app/api-reference/components/script#strategy)

脚本的加载策略。可以使用四种不同的策略：

- `beforeInteractive` ：在任何 Next.js 代码之前以及任何页面冻结发生之前加载。
- `afterInteractive` ：（**默认**）提前加载，但在页面上发生一些水化之后。
- `lazyOnload` ：在浏览器空闲期间加载。
- `worker`: (experimental) Load in a web worker.

### [`beforeInteractive`](https://nextjs.org/docs/app/api-reference/components/script#beforeinteractive)

使用 `beforeInteractive` 策略加载的脚本将从服务器注入到初始 HTML 中，在任何 Next.js 模块之前下载，并按照它们在页面上发生任何冻结之前的放置顺序执行。

使用此策略表示的脚本在任何第一方代码之前预加载和获取，但它们的执行不会阻止页面冻结发生。

`beforeInteractive` 脚本必须放置在根布局 （ `app/layout.tsx)` 内，旨在加载整个站点所需的脚本（即，当应用程序中的任何页面在服务器端加载时，脚本将加载）。

**此策略应仅用于需要在页面的任何部分变为交互式之前获取的关键脚本。**

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
      <Script
        src="https://example.com/script.js"
        strategy="beforeInteractive"
      />
    </html>
  );
}
```

> **温馨提示**：带有 `beforeInteractive` 的脚本将始终注入到 HTML 文档的 `head` 中，无论它放置在组件中的哪个位置。

应尽快使用 `beforeInteractive` 加载的一些脚本示例包括：

- Bot detectors
- Cookie consent managers 

### [`afterInteractive`](https://nextjs.org/docs/app/api-reference/components/script#afterinteractive)

使用 `afterInteractive` 策略的脚本将注入到 HTML 客户端，并在页面上发生部分（或全部）冻结后加载。这是脚本组件的**默认策略**，应用于需要尽快加载的任何脚本，但不在任何第一方 Next.js 代码之前加载。

`afterInteractive` 脚本可以放置在任何页面或布局中，并且仅在浏览器中打开该页面（或页面组）时加载和执行。

```js
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="afterInteractive" />
    </>
  );
}
```

一些适合 `afterInteractive` 的脚本示例包括：

- Tag managers
- Analytics

### [`lazyOnload`](https://nextjs.org/docs/app/api-reference/components/script#lazyonload)

使用 `lazyOnload` 策略的脚本在浏览器空闲期间注入到 HTML 客户端，并在获取页面上的所有资源后加载。此策略应用于不需要提前加载的任何后台或低优先级脚本。

`lazyOnload` 脚本可以放置在任何页面或布局中，并且仅在浏览器中打开该页面（或页面组）时加载和执行。

```js
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="lazyOnload" />
    </>
  );
}
```

不需要立即加载且可以使用 `lazyOnload` 获取的脚本示例包括：

- Chat support plugins 聊天支持插件
- Social media widgets 社交媒体小部件

### [`worker`](https://nextjs.org/docs/app/api-reference/components/script#worker)

> **警告**： `worker` 策略尚不稳定，尚不适用于 `app` 目录。请谨慎使用。

使用 `worker` 策略的脚本将卸载到 Web worker 中，以释放主线程并确保仅在其上处理关键的第一方资源。虽然此策略可用于任何脚本，但它是一个高级用例，不能保证支持所有第三方脚本。

若要使用 `worker` 作为策略，必须在 `next.config.js` 中启用 `nextScriptWorkers` 标志：

```js
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
};
```

`worker` 脚本**当前只能在 `pages/` 目录中使用**：

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

### [`onLoad`](https://nextjs.org/docs/app/api-reference/components/script#onload)

> **警告**： `onLoad` 尚不适用于服务器组件，只能在客户端组件中使用。此外， `onLoad` 不能与 `beforeInteractive` 一起使用 – 请考虑改用 `onReady` 。

某些第三方脚本要求用户在脚本完成加载后运行一次 JavaScript 代码，以便实例化内容或调用函数。如果要使用 afterInteractive 或 lazyOnload 作为加载策略加载脚本，则可以在加载代码后使用 onLoad 属性执行代码。

下面是仅在加载库后执行 lodash 方法的示例。

```tsx
'use client';
 
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"
        onLoad={() => {
          console.log(_.sample([1, 2, 3, 4]));
        }}
      />
    </>
  );
}
```

### [`onReady`](https://nextjs.org/docs/app/api-reference/components/script#onready)

> **Warning:** `onReady` 尚不适用于服务器组件，只能在客户端组件中使用。

某些第三方脚本要求用户在脚本完成加载后和每次挂载组件时（例如，在路由导航之后）运行 JavaScript 代码。当它首次加载时，可以在脚本的 load 事件之后执行代码，然后在每次后续组件重新装载后使用 onReady 属性执行代码。

以下是每次挂载组件时如何重新实例化 Google Maps JS 嵌入的示例：

```tsx
'use client';
 
import { useRef } from 'react';
import Script from 'next/script';
 
export default function Page() {
  const mapRef = useRef();
 
  return (
    <>
      <div ref={mapRef}></div>
      <Script
        id="google-maps"
        src="https://maps.googleapis.com/maps/api/js"
        onReady={() => {
          new google.maps.Map(mapRef.current, {
            center: { lat: -34.397, lng: 150.644 },
            zoom: 8,
          });
        }}
      />
    </>
  );
}
```

### [`onError`](https://nextjs.org/docs/app/api-reference/components/script#onerror)

> **Warning:**  `onError` 尚不适用于服务器组件，只能在客户端组件中使用。 `onError` 不能与 `beforeInteractive` 加载策略一起使用。

有时，捕获脚本加载失败会很有帮助。可以使用 onError 属性处理这些错误

```tsx
'use client';
 
import Script from 'next/script';
 
export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onError={(e: Error) => {
          console.error('Script failed to load', e);
        }}
      />
    </>
  );
}
```

