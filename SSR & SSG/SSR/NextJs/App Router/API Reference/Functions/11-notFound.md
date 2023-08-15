# notFound

`notFound` 函数允许您在路由段内渲染 `not-found file` 以及注入 `<meta name="robots" content="noindex" />` 标记。

## [`notFound()`](https://nextjs.org/docs/app/api-reference/functions/not-found#notfound)

Invoking the `notFound()` function throws a `NEXT_NOT_FOUND` error and terminates rendering of the route segment in which it was thrown. Specifying a [**not-found** file](https://nextjs.org/docs/app/api-reference/file-conventions/not-found) allows you to gracefully handle such errors by rendering a Not Found UI within the segment.
调用 `notFound()` 函数会引发 `NEXT_NOT_FOUND` 错误，并终止引发该错误的路由段的渲染。通过指定not-found file，您可以通过在段中渲染Not Found UI 来正常处理此类错误。

```js
import { notFound } from 'next/navigation';
 
async function fetchUsers(id) {
  const res = await fetch('https://...');
  if (!res.ok) return undefined;
  return res.json();
}
 
export default async function Profile({ params }) {
  const user = await fetchUser(params.id);
 
  if (!user) {
    notFound();
  }
 
  // ...
}
```

> 注意：由于使用 TypeScript `never` 类型， `notFound()` 不需要您使用 `return notFound()` 。