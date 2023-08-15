# error.js

**error**文件定义路由段的error UI 边界。

```tsx
'use client'; // Error components must be Client Components
 
import { useEffect } from 'react';
 
export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error);
  }, [error]);
 
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  );
}
```

## [Props](https://nextjs.org/docs/app/api-reference/file-conventions/error#props)

### [`error`](https://nextjs.org/docs/app/api-reference/file-conventions/error#error)

`Error` 对象的实例。此错误可能发生在服务器或客户端上。

### [`reset`](https://nextjs.org/docs/app/api-reference/file-conventions/error#reset)

用于重置不返回响应的错误边界的函数。

> **Good to know: 很高兴知道：**
>
> - `error.js` 边界必须是**客户端组件**。
> - `error.js` 边界不会处理在同一段的 `layout.js` 组件中引发的错误，因为错误边界嵌套在该布局组件内。
>   - 要处理特定布局的错误，请将 `error.js` 文件放在布局父段中。
>   - 要处理根布局或模板中的错误，请使用名为 `app/global-error.js` 的 `error.js` 变体。

## [`global-error.js`](https://nextjs.org/docs/app/api-reference/file-conventions/error#global-errorjs)

要专门处理根 `layout.js` 中的错误，请使用位于根 `app` 目录中的名为 `app/global-error.js` 的 `error.js` 变体。

```tsx
'use client';
 
export default function GlobalError({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```

> **Good to know: 很高兴知道：**
>
> - `global-error.js` 在活跃时替换根 `layout.js` ，因此必须定义自己的 `<html>` 和 `<body>` 标记。