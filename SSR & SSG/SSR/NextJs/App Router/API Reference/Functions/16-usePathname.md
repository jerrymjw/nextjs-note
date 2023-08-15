# usePathname

`usePathname` 是一个客户端组件挂钩，允许您读取当前 URL 的路径名。

```tsx
'use client';
 
import { usePathname } from 'next/navigation';
 
export default function ExampleClientComponent() {
  const pathname = usePathname();
  return <>Current pathname: {pathname}</>;
}
```

> **Good to know: **
>
> - `usePathname` 是客户端组件挂钩，**在服务器组件中不受支持**。
> - 兼容模式：
>   - `usePathname` can return `null` when a [fallback route](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths#fallback-true) is being rendered or when a `pages` directory page has been [automatically statically optimized](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization) by Next.js and the router is not ready.
>     当正在渲染回退路由或 Next.js 自动静态优化 `pages` 目录页面时，并且路由器未准备就绪时`usePathname` 可以返回 `null` 。
>   - Next.js如果它在项目中的 `app` 和 `pages` 目录都检测到，它将自动更新您的类型。

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/use-pathname#parameters)

```
const pathname = usePathname();
```

`usePathname` 不采用任何参数。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/use-pathname#returns)

`usePathname` 返回当前 URL 路径名的字符串。例如：

| URL                 | Returned value        |
| ------------------- | --------------------- |
| `/`                 | `'/'`                 |
| `/dashboard`        | `'/dashboard'`        |
| `/dashboard?v=2`    | `'/dashboard'`        |
| `/blog/hello-world` | `'/blog/hello-world'` |

## [Examples](https://nextjs.org/docs/app/api-reference/functions/use-pathname#examples)

### [执行某些操作以响应路由更改](https://nextjs.org/docs/app/api-reference/functions/use-pathname#do-something-in-response-to-a-route-change)

```tsx
'use client';
 
import { usePathname, useSearchParams } from 'next/navigation';
 
function ExampleClientComponent() {
  const pathname = usePathname();
  const searchParams = useSearchParams();
  useEffect(() => {
    // Do something here...
  }, [pathname, searchParams]);
}
```

