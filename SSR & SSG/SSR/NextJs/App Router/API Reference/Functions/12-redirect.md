# redirect

`redirect` 函数允许您将用户重定向到另一个 URL。如果需要重定向到 404，可以使用 `notFound` 函数。

## [`redirect()`](https://nextjs.org/docs/app/api-reference/functions/redirect#redirect)

调用 `redirect()` 函数会引发 `NEXT_REDIRECT` 错误，并终止引发该错误的路由段的渲染。相对或绝对 URL都可以 调用 `redirect()` 。

```js
import { redirect } from 'next/navigation';
 
async function fetchTeam(id) {
  const res = await fetch('https://...');
  if (!res.ok) return undefined;
  return res.json();
}
 
export default async function Profile({ params }) {
  const team = await fetchTeam(params.id);
  if (!team) {
    redirect('/login');
  }
 
  // ...
}
```

> 注意：由于使用 TypeScript `never` 类型， `notFound()` 不需要您使用 `return notFound()` 。

