# useRouter

`useRouter` 挂钩允许您以编程方式更改客户端组件内的路由。

> **Recommendation:** 使用 `<Link>` 组件进行导航，除非您对使用 `useRouter` 有特定要求。

```tsx
'use client';
 
import { useRouter } from 'next/navigation';
 
export default function Page() {
  const router = useRouter();
 
  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  );
}
```

## [`useRouter()`](https://nextjs.org/docs/app/api-reference/functions/use-router#userouter)

- `router.push(href: string)` ：对提供的路由执行客户端导航。将新条目添加到浏览器的历史记录 [browser’s history](https://developer.mozilla.org/en-US/docs/Web/API/History_API)堆栈中。
- `router.replace(href: string)` ：执行客户端导航到提供的路由，而不向浏览器的历史记录堆栈添加新条目。
- `router.refresh()` ：刷新当前路由。向服务器发出新请求，重新获取数据请求，并重新渲染服务器组件。客户端将合并更新的 React Server 组件有效负载，而不会丢失不受影响的客户端 React （例如 `useState` ）或浏览器状态（例如滚动位置）。
- `router.prefetch(href: string)` ：预取提供的路由以加快客户端转换。
- `router.back()` ：使用软导航导航回到浏览器历史记录堆栈中的上一个路由。
- `router.forward()` ：使用软导航向前导航到浏览器历史记录堆栈中的下一页。

> **Good to know: 很高兴知道：**
>
> - 如果新路由已预取，则 `push()` 和 `replace()` 方法将执行软导航，并且不包含动态段或具有与当前路由相同的动态参数。
> - `next/link` 在路由在视口中可见时自动预取路由。
> - 如果缓存了fetch请求，则 `refresh()` 可能会重现相同的结果。其他动态函数（如 `cookies` 和 `headers` ）也可以更改响应。

## [Examples](https://nextjs.org/docs/app/api-reference/functions/use-router#examples)

### [Router Events](https://nextjs.org/docs/app/api-reference/functions/use-router#router-events)

可以通过组合其他客户端组件挂钩（如` usePathname` 和 `useSearchParams` ）来侦听页面更改。

```js
'use client';
 
import { useEffect } from 'react';
import { usePathname, useSearchParams } from 'next/navigation';
 
export function NavigationEvents() {
  const pathname = usePathname();
  const searchParams = useSearchParams();
 
  useEffect(() => {
    const url = pathname + searchParams.toString();
    console.log(url);
    // You can now use the current URL
    // ...
  }, [pathname, searchParams]);
 
  return null;
}
```

可以导入到布局中。

```js
import { Suspense } from 'react';
import { NavigationEvents } from './components/navigation-events';
 
export default function Layout({ children }) {
  return (
    <html lang="en">
      <body>
        {children}
 
        <Suspense fallback={null}>
          <NavigationEvents />
        </Suspense>
      </body>
    </html>
  );
}
```

> 注意： `<NavigationEvents>` 包装在 `Suspense` 边界中，因为 `useSearchParams()` 会导致客户端渲染在静态渲染期间达到最接近的 `Suspense` 边界。了解更多信息。