# cookies

`cookies` 函数允许您从服务器组件读取 HTTP 传入请求cookies，或在服务器操作或路由处理程序中写入传出请求cookies。

> **Good to know: 很高兴知道：**
>
> - `cookies()` 是一个动态函数**[Dynamic Function](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)**，其返回值无法提前知道。在布局或页面中使用它将在请求时选择动态渲染**[dynamic rendering](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-rendering)** 的路由。

## [`cookies().get(name)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiesgetname)

采用 cookie 名称并返回具有名称和值的对象的方法。如果未找到 `name` 的 cookies，则返回 `undefined` 。如果多个 cookies 匹配，则只会返回第一个匹配项。

```js
import { cookies } from 'next/headers';
 
export default function Page() {
  const cookieStore = cookies();
  const theme = cookieStore.get('theme');
  return '...';
}
```


On this page 在此页面上

- [`cookies().get(name)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiesgetname)
- [`cookies().getAll()`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiesgetall)
- [`cookies().has(name)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookieshasname)
- [`cookies().set(name, value, options)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiessetname-value-options)
- [Next Steps](https://nextjs.org/docs/app/api-reference/functions/cookies#next-steps)

Scroll to top

[App Router](https://nextjs.org/docs/app)[...](https://nextjs.org/docs/app/api-reference)[Functions](https://nextjs.org/docs/app/api-reference/functions)cookies

# cookies

The `cookies` function allows you to read the HTTP incoming request cookies from a [Server Component](https://nextjs.org/docs/getting-started/react-essentials) or write outgoing request cookies in a [Server Action](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions) or [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/router-handlers).
`cookies` 函数允许您从服务器组件读取 HTTP 传入请求 Cookie，或在服务器操作或路由处理程序中写入传出请求 Cookie。

> **Good to know: 很高兴知道：**
>
> - `cookies()` is a **[Dynamic Function](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)** whose returned values cannot be known ahead of time. Using it in a layout or page will opt a route into **[dynamic rendering](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-rendering)** at request time.
>   `cookies()` 是一个动态函数，其返回值无法提前知道。在布局或页面中使用它将在请求时选择动态呈现的路由。

## [`cookies().get(name)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiesgetname)

A method that takes a cookie name and returns an object with name and value. If a cookie with `name` isn't found, it returns `undefined`. If multiple cookies match, it will only return the first match.
采用 cookie 名称并返回具有名称和值的对象的方法。如果未找到 `name` 的 cookie，则返回 `undefined` 。如果多个 cookie 匹配，则只会返回第一个匹配项。

```js
import { cookies } from 'next/headers';
 
export default function Page() {
  const cookieStore = cookies();
  const theme = cookieStore.get('theme');
  return '...';
}
```

## [`cookies().getAll()`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiesgetall)

类似于 `get` 的方法，但返回具有匹配 `name` 的所有cookies的列表。如果未指定 `name` ，则返回所有可用的cookies。

```js
import { cookies } from 'next/headers';
 
export default function Page() {
  const cookieStore = cookies();
  return cookieStore.getAll().map((cookie) => (
    <div key={cookie.name}>
      <p>Name: {cookie.name}</p>
      <p>Value: {cookie.value}</p>
    </div>
  ));
}
```

## [`cookies().has(name)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookieshasname)

A method that takes a cookie name and returns a `boolean` based on if the cookie exists (`true`) or not (`false`).
一种方法，它采用 cookie name并根据 cookie 是否存在 （ `true` ） 或不存在 （ `false` ） 返回 `boolean` 。

```js
import { cookies } from 'next/headers';
 
export default function Page() {
  const cookiesList = cookies();
  const hasCookie = cookiesList.has('theme');
  return '...';
}
```

## [`cookies().set(name, value, options)`](https://nextjs.org/docs/app/api-reference/functions/cookies#cookiessetname-value-options)

一种获取 Cookie name、值和选项并设置传出请求 Cookie 的方法。此方法仅在 [Server Action](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions) 或 [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/router-handlers).中可用。

```js
import { cookies } from 'next/headers';
 
async function create(data) {
  'use server';
  cookies().set('name', 'lee');
  // or
  cookies().set('name', 'lee', { secure: true });
  // or
  cookies().set({
    name: 'name',
    value: 'lee',
    httpOnly: true,
    path: '/',
  });
}
```

