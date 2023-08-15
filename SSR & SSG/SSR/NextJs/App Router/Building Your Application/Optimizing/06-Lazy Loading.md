# Lazy Loading

Next.js 中的延迟加载通过减少渲染路由所需的 JavaScript 量来帮助提高应用程序的初始加载性能。

它允许您延迟**客户端组件**和导入库的加载，并且仅在需要时将它们包含在客户端捆绑包中。例如，您可能希望延迟加载模式，直到用户单击以打开它。

有两种方法可以在 Next 中实现延迟加载.js：

1. 将动态导入与 `next/dynamic` 结合使用
2. Using [`React.lazy()`](https://react.dev/reference/react/lazy) with [Suspense](https://react.dev/reference/react/Suspense)
   将 [`React.lazy()`](https://react.dev/reference/react/lazy) 与 [Suspense](https://react.dev/reference/react/Suspense)一起使用

默认情况下，服务器组件会自动进行代码拆分，您可以使用流式处理将 UI 片段从服务器逐步发送到客户端。延迟加载适用于**客户端组件**。

## [`next/dynamic`](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#nextdynamic)

`next/dynamic` 是 [`React.lazy()`](https://react.dev/reference/react/lazy) 和 [Suspense](https://react.dev/reference/react/Suspense)的组合。它在 `app` 和 `pages` 目录中的行为方式相同，以允许增量迁移。

## [Examples](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#examples)

### [导入客户端组件](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#importing-client-components)

```js
'use client';
 
import { useState } from 'react';
import dynamic from 'next/dynamic';
 
// Client Components:
const ComponentA = dynamic(() => import('../components/A'));
const ComponentB = dynamic(() => import('../components/B'));
const ComponentC = dynamic(() => import('../components/C'), { ssr: false });
 
export default function ClientComponentExample() {
  const [showMore, setShowMore] = useState(false);
 
  return (
    <div>
      {/* Load immediately, but in a separate client bundle */}
      <ComponentA />
 
      {/* Load on demand, only when/if the condition is met */}
      {showMore && <ComponentB />}
      <button onClick={() => setShowMore(!showMore)}>Toggle</button>
 
      {/* Load only on the client side */}
      <ComponentC />
    </div>
  );
}
```

### [Skipping SSR](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#skipping-ssr)

使用 `React.lazy()` 和 Suspense 时，默认情况下将预渲染客户端组件 （SSR）。

如果要禁用客户端组件的预渲染，可以使用 `ssr` 选项设置为 `false`：

```jsx
const ComponentC = dynamic(() => import('../components/C'), { ssr: false });
```

### [Importing Server Components 导入服务器组件](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#importing-server-components)

如果动态导入服务器组件，则只有作为服务器组件子级的客户端组件才会延迟加载，而不是服务器组件本身。

```js
import dynamic from 'next/dynamic';
 
// Server Component:
const ServerComponent = dynamic(() => import('../components/ServerComponent'));
 
export default function ServerComponentExample() {
  return (
    <div>
      <ServerComponent />
    </div>
  );
}
```

### [加载外部库](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#loading-external-libraries)

可以使用 `import()` 函数按需加载外部库。此示例使用外部库 `fuse.js` 进行模糊搜索。该模块仅在用户键入搜索输入后加载到客户端上。

```js
'use client';
 
import { useState } from 'react';
 
const names = ['Tim', 'Joe', 'Bel', 'Lee'];
 
export default function Page() {
  const [results, setResults] = useState();
 
  return (
    <div>
      <input
        type="text"
        placeholder="Search"
        onChange={async (e) => {
          const { value } = e.currentTarget;
          // Dynamically load fuse.js
          const Fuse = (await import('fuse.js')).default;
          const fuse = new Fuse(names);
 
          setResults(fuse.search(value));
        }}
      />
      <pre>Results: {JSON.stringify(results, null, 2)}</pre>
    </div>
  );
}
```

[添加自定义加载组件](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#adding-a-custom-loading-component)

```js
import dynamic from 'next/dynamic';
 
const WithCustomLoading = dynamic(
  () => import('../components/WithCustomLoading'),
  {
    loading: () => <p>Loading...</p>,
  },
);
 
export default function Page() {
  return (
    <div>
      {/* The loading component will be rendered while  <WithCustomLoading/> is loading */}
      <WithCustomLoading />
    </div>
  );
}
```

### [Importing Named Exports](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading#importing-named-exports)

To dynamically import a named export, you can return it from the Promise returned by [`import()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) function:
若要动态导入命名导出，可以从 `import()` 函数返回的 Promise 返回它

```js
'use client';
 
export function Hello() {
  return <p>Hello!</p>;
}
```

```js
import dynamic from 'next/dynamic';
 
const ClientComponent = dynamic(() =>
  import('../components/ClientComponent').then((mod) => mod.Hello),
);
```

**注：最后一个不太理解**