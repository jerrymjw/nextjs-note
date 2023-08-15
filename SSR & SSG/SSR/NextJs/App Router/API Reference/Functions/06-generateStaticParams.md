# generateStaticParams

`generateStaticParams` 函数可与**动态路由段**结合使用，以在构建时**静态生成路由**，而不是在请求时按需生成路由。

```js
// Return a list of `params` to populate the [slug] dynamic segment
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json());
 
  return posts.map((post) => ({
    slug: post.slug,
  }));
}
 
// Multiple versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
export default function Page({ params }) {
  const { slug } = params;
  // ...
}
```

> **Good to know**
>
> - 您可以使用 [`dynamicParams`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamicparams) 段配置选项来控制访问未使用 `generateStaticParams` 生成的动态分段时发生的情况。
> - 在 `next dev` 期间，导航到路由时将调用 `generateStaticParams` 。
> - 在 `next build` 期间，在生成相应的布局或页面之前运行 `generateStaticParams` 。
> - 在重新验证 （ISR） 期间，不会再次调用 `generateStaticParams` 。
> - `generateStaticParams` 替换掉了在Pages Router中的 [`getStaticPaths`](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths) 函数。

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#parameters)

`options.params` (optional) 

If multiple dynamic segments in a route use `generateStaticParams`, the child `generateStaticParams` function is executed once for each set of `params` the parent generates.
如果路由中的多个动态段使用 `generateStaticParams` ，则子 `generateStaticParams` 函数将针对父级生成的每组 `params` 执行一次。

`params` 对象包含父 `generateStaticParams` 中填充的 `params` ，可用于在子段中生成 `params` [generate the `params` in a child segment](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#multiple-dynamic-segments-in-a-route)。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#returns)

`generateStaticParams` 应返回一个对象数组，其中每个对象表示单个路由的填充动态段。

- 对象中的每个属性都是要为路由填充的动态段。
- 属性名称是段的名称，属性值是应填充该段的值。

| Example Route                    | `generateStaticParams` Return Type        |
| -------------------------------- | ----------------------------------------- |
| `/product/[id]`                  | `{ id: string }[]`                        |
| `/products/[category]/[product]` | `{ category: string, product: string }[]` |
| `/products/[...slug]`            | `{ slug: string[] }[]`                    |

## [Single Dynamic Segment](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#single-dynamic-segment)

```tsx
export function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }, { id: '3' }];
}
 
// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /product/1
// - /product/2
// - /product/3
export default function Page({ params }: { params: { id: string } }) {
  const { id } = params;
  // ...
}
```

## [Multiple Dynamic Segments](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#multiple-dynamic-segments)

```tsx
export function generateStaticParams() {
  return [
    { category: 'a', product: '1' },
    { category: 'b', product: '2' },
    { category: 'c', product: '3' },
  ];
}
 
// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /product/a/1
// - /product/b/2
// - /product/c/3
export default function Page({
  params,
}: {
  params: { category: string; product: string };
}) {
  const { category, product } = params;
  // ...
}
```

## [Catch-all Dynamic Segment](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#catch-all-dynamic-segment)

```tsx
export function generateStaticParams() {
  return [{ slug: ['a', '1'] }, { slug: ['b', '2'] }, { slug: ['c', '3'] }];
}
 
// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /product/a/1
// - /product/b/2
// - /product/c/3
export default function Page({ params }: { params: { slug: string[] } }) {
  const { slug } = params;
  // ...
}
```

## [Examples](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#examples)

### [Multiple Dynamic Segments in a Route](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#multiple-dynamic-segments-in-a-route)

您可以为当前布局或页面**上方**的动态区段生成参数，但不能在**下方**生成参数。例如，给定 `app/products/[category]/[product]` 路由：

- `app/products/[category]/[product]/page.js` 可以为 `[category]` 和 `[product]` 生成参数。
- `app/products/[category]/layout.js` **只能**生成 `[category]` 的参数。

有两种方法可以为具有多个动态段的路由生成参数：

### [自下而上生成参数](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#generate-params-from-the-bottom-up)

从子路由段生成多个动态分段。

```tsx
// Generate segments for both [category] and [product]
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) =>
    res.json(),
  );
 
  return products.map((product) => ({
    category: product.category.slug,
    product: product.id,
  }));
}
 
export default function Page({
  params,
}: {
  params: { category: string; product: string };
}) {
  // ...
}
```

### [自上而下生成参数](https://nextjs.org/docs/app/api-reference/functions/generate-static-params#generate-params-from-the-top-down)

Generate the parent segments first and use the result to generate the child segments.
首先生成父区段，然后使用结果生成子区段。

```tsx
// Generate segments for [category]
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) =>
    res.json(),
  );
 
  return products.map((product) => ({
    category: product.category.slug,
  }));
}
 
export default function Layout({ params }: { params: { category: string } }) {
  // ...
}
```

子路由段的 `generateStaticParams` 函数对于父路由`generateStaticParams` 生成的每个分段执行一次。

子 `generateStaticParams` 函数可以使用从父 `generateStaticParams` 函数返回的 `params` 来动态生成自己的段。

```tsx
// Generate segments for [product] using the `params` passed from
// the parent segment's `generateStaticParams` function
export async function generateStaticParams({
  params: { category },
}: {
  params: { category: string };
}) {
  const products = await fetch(
    `https://.../products?category=${category}`,
  ).then((res) => res.json());
 
  return products.map((product) => ({
    product: product.id,
  }));
}
 
export default function Page({
  params,
}: {
  params: { category: string; product: string };
}) {
  // ...
}
```

> **Good to know**: 渲染路由时，Next.js 将自动删除 `generateMetadata` 、 `generateStaticParams` 、布局、页面和服务器组件中相同数据的 `fetch` 请求。如果 `fetch` 不可用，可以使用 React `cache` 。