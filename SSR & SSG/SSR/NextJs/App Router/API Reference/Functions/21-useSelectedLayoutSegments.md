# useSelectedLayoutSegments

`useSelectedLayoutSegments` 是一个客户端组件挂钩，允许您读取从中调用它的布局下方的活动路由段。

It is useful for creating UI in parent Layouts that need knowledge of active child segments such as breadcrumbs.
它可用于在需要了解活动子段（如痕迹导航）的父布局中创建 UI。

```tsx
'use client';
 
import { useSelectedLayoutSegments } from 'next/navigation';
 
export default function ExampleClientComponent() {
  const segments = useSelectedLayoutSegments();
 
  return (
    <ul>
      {segments.map((segment, index) => (
        <li key={index}>{segment}</li>
      ))}
    </ul>
  );
}
```

> **Good to know: 很高兴知道：**
>
> - 由于 `useSelectedLayoutSegments` 是客户端组件挂钩，而布局默认是服务器组件，因此通常通过导入到布局中的客户端组件调用 `useSelectedLayoutSegments` 。
> - 返回的路段包括路由组，您可能不希望将其包含在 UI 中。可以使用 `filter()` 数组方法删除以括号开头的项目。

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segments#parameters)

```
const segments = useSelectedLayoutSegments();
```

`useSelectedLayoutSegments` 不采用任何参数。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segments#returns)

`useSelectedLayoutSegments` 返回一个字符串数组，其中包含从调用挂钩的布局向下一级的活动段。或者一个空数组（如果不存在）。

例如，给定下面的布局和 URL，返回的段将是：

| Layout                    | Visited URL           | Returned Segments           |
| ------------------------- | --------------------- | --------------------------- |
| `app/layout.js`           | `/`                   | `[]`                        |
| `app/layout.js`           | `/dashboard`          | `['dashboard']`             |
| `app/layout.js`           | `/dashboard/settings` | `['dashboard', 'settings']` |
| `app/dashboard/layout.js` | `/dashboard`          | `[]`                        |
| `app/dashboard/layout.js` | `/dashboard/settings` | `['settings']`              |