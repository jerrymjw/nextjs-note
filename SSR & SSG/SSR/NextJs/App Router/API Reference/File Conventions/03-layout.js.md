# layout.js

**布局**是在路由之间共享的 UI

```tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return <section>{children}</section>;
}
```

## [Props](https://nextjs.org/docs/app/api-reference/file-conventions/layout#props)

### [`children` (required)](https://nextjs.org/docs/app/api-reference/file-conventions/layout#children-required)

布局组件应接受并使用 `children` 属性。在渲染期间， `children` 将使用正在被布局包装的路由段填充。这些将主要是子布局（如果存在）或页面的组件，但也可能是其他特殊文件，如加载或错误（如果适用）。

### [`params` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/layout#params-optional)

动态路由参数对象从根段向下到该布局。

![image-20230526160606275](/Users/jerry/Library/Application Support/typora-user-images/image-20230526160606275.png)

For example:

```ts
export default function ShopLayout({
  children,
  params,
}: {
  children: React.ReactNode;
  params: {
    tag: string;
    item: string;
  };
}) {
  // URL -> /shop/shoes/nike-air-max-97
  // `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
  return <section>{children}</section>;
}
```

### [Good to know 很高兴知道](https://nextjs.org/docs/app/api-reference/file-conventions/layout#good-to-know)

#### [布局不接收 searchParams `](https://nextjs.org/docs/app/api-reference/file-conventions/layout#layouts-do-not-receive-searchparams)

与页面(page)不同，布局(layout)组件不接收 `searchParams` props。这是因为在导航期间不会重新渲染 [not re-rendered during navigation](https://nextjs.org/docs/app/building-your-application/routing#partial-rendering) 共享布局，这可能会导致导航之间的 `searchParams` 过时。

使用客户端导航时，Next.js 自动仅渲染两个路由之间公共布局(common layout)下方的页面部分。

例如，在以下目录结构中， `dashboard/layout.tsx` 是 `/dashboard` 和 `/dashboard/settings` 的通用布局(common layout)：

![image-20230526161347720](/Users/jerry/Library/Application Support/typora-user-images/image-20230526161347720.png)

从 `/dashboard` 导航到 `/dashboard/settings` 时， `dashboard/settings/page.tsx` 将在服务器上渲染，因为它是更改的段，而 `dashboard/layout.tsx` 不会重新渲染，因为它是两个路由之间的通用布局。

此性能优化允许共享布局的页面之间的导航更快，因为只有页面的数据获取和渲染必须运行，而不是可能包含获取自己数据的共享布局的整个路由。

由于 `dashboard/layout.tsx` 不会重新渲染，因此在导航时读取布局服务器组件中的 `searchParams` 属性将变得过时。

- 相反，请使用客户端组件中的 Page  [`searchParams`](https://nextjs.org/docs/app/api-reference/file-conventions/page) 属性或  [`useSearchParams`](https://nextjs.org/docs/app/api-reference/functions/use-params) 钩子，该钩子使用最新的 `searchParams` 在客户端上重新渲染。

## [Root Layouts](https://nextjs.org/docs/app/api-reference/file-conventions/layout#root-layouts)

- `app` 目录**必须**包含根 `app/layout.js` 。
- 根布局必须定义 `<html>` 和 `<body>` 标记。
  - You should **not** manually add `<head>` tags such as `<title>` and `<meta>` to root layouts. Instead, you should use the [Metadata API](https://nextjs.org/docs/app/api-reference/functions/generate-metadata) which automatically handles advanced requirements such as streaming and de-duplicating `<head>` elements.
    **不应手动**将 `<head>` 标记（如 `<title>` 和 `<meta>` ）添加到根布局。相反，您应该使用 [Metadata API](https://nextjs.org/docs/app/api-reference/functions/generate-metadata) ，它会自动处理**高级要求**，例如**流式处理**和**重复数据删除** `<head>` 元素。
- 您可以使用路由组创建多个根布局。
  - **跨多个根布局导航**将导致**整个页面加载**（与客户端导航相反）。例如，从使用 `app/(shop)/layout.js` 的 `/cart` 导航到使用 `app/(marketing)/layout.js` 的 `/blog` 将导致整个页面加载。这**仅适用于多个根布局**。

