# useParams

# useParams

`useParams` 是一个**客户端组件**挂钩，可用于读取由当前 URL 填充的路由动态参数。

> **Note:** Next.js 13.3 中添加了对 `useParams` 的支持。

```ts
'use client';
 
import { useParams } from 'next/navigation';
 
export default function ExampleClientComponent() {
  const params = useParams();
 
  // Route -> /shop/[tag]/[item]
  // URL -> /shop/shoes/nike-air-max-97
  // `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
  console.log(params);
 
  return <></>;
}
```

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/use-params#parameters)

```
const params = useParams();
```

`useParams` 不采用任何参数。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/use-params#returns)

`useParams` 返回一个对象，其中包含当前路由填充的动态参数。

- 对象中的每个属性都是一个活跃的动态段。
- 属性名称是段的名称，属性值是段中填充的值。
- 属性值将为 `string` 或 `string` 数组，具体取决于动态段的类型。
- 如果路由包含非动态参数，则 `useParams` 返回空对象。
- 如果在 `pages` 中使用，则 `useParams` 将返回 `null` 。

For example:

| Route                           | URL         | `useParams()`             |
| ------------------------------- | ----------- | ------------------------- |
| `app/shop/page.js`              | `/shop`     | `null`                    |
| `app/shop/[slug]/page.js`       | `/shop/1`   | `{ slug: '1' }`           |
| `app/shop/[tag]/[item]/page.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |
| `app/shop/[...slug]/page.js`    | `/shop/1/2` | `{ slug: ['1', '2'] }`    |