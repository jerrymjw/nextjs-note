# NextResponse

NextResponse 通过其他方便的方法扩展了 [Web Response API](https://developer.mozilla.org/en-US/docs/Web/API/Response)。

## [`cookies`](https://nextjs.org/docs/app/api-reference/functions/next-response#cookies)

读取或改变响应的 [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) header。

### [`set(name, value)`](https://nextjs.org/docs/app/api-reference/functions/next-response#setname-value)

给定一个名称，在响应中设置具有给定值的 cookie。

```js
// Given incoming request /home
let response = NextResponse.next();
// Set a cookie to hide the banner
response.cookies.set('show-banner', 'false');
// Response will have a `Set-Cookie:show-banner=false;path=/home` header
return response;
```

### [`get(name)`](https://nextjs.org/docs/app/api-reference/functions/next-response#getname)

给定一个 cookie 名称，返回 cookie 的值。如果未找到 cookie，则返回 `undefined` 。如果找到多个 Cookie，则返回第一个 Cookie。

```js
// Given incoming request /home
let response = NextResponse.next();
// { name: 'show-banner', value: 'false', Path: '/home' }
response.cookies.get('show-banner');
```

### [`getAll()`](https://nextjs.org/docs/app/api-reference/functions/next-response#getall)

给定一个 cookie 名称，返回 cookie 的值。如果未指定名称，则返回响应上的所有 cookie。

```js
// Given incoming request /home
let response = NextResponse.next();
// [
//   { name: 'experiments', value: 'new-pricing-page', Path: '/home' },
//   { name: 'experiments', value: 'winter-launch', Path: '/home' },
// ]
response.cookies.getAll('experiments');
// Alternatively, get all cookies for the response
response.cookies.getAll();
```

### [`delete(name)`](https://nextjs.org/docs/app/api-reference/functions/next-response#deletename)

给定 Cookie 名称，从响应中删除该 Cookie。

```js
// Given incoming request /home
let response = NextResponse.next();
// Returns true for deleted, false is nothing is deleted
response.cookies.delete('experiments');
```

## [`json()`](https://nextjs.org/docs/app/api-reference/functions/next-response#json)

Produce a response with the given JSON body.
使用给定的 JSON body生成响应。

```js
import { NextResponse } from 'next/server';
 
export async function GET(request: Request) {
  return NextResponse.json({ error: 'Internal Server Error' }, { status: 500 });
}
```

## [`redirect()`](https://nextjs.org/docs/app/api-reference/functions/next-response#redirect)

Produce a response that redirects to a [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL).
生成重定向到 [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)的响应。

```js
import { NextResponse } from 'next/server';
 
return NextResponse.redirect(new URL('/new', request.url));
```

在 `NextResponse.redirect()` 方法中使用 URL 之前，可以创建和修改 [URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) 。例如，可以使用 `request.nextUrl` 属性获取当前 URL，然后将其修改为重定向到其他 URL。

```js
import { NextResponse } from 'next/server';
 
// Given an incoming request...
const loginUrl = new URL('/login', request.url);
// Add ?from=/incoming-url to the /login URL
loginUrl.searchParams.set('from', request.nextUrl.pathname);
// And redirect to the new URL
return NextResponse.redirect(loginUrl);
```

## [`rewrite()`](https://nextjs.org/docs/app/api-reference/functions/next-response#rewrite)

生成重写（代理）给定 URL 的响应，同时保留显示原始 URL。

```js
import { NextResponse } from 'next/server';
 
// Incoming request: /about, browser shows /about
// Rewritten request: /proxy, browser shows /about
return NextResponse.rewrite(new URL('/proxy', request.url));
```

## [`next()`](https://nextjs.org/docs/app/api-reference/functions/next-response#next)

`next()` 方法对中间件很有用，因为它允许您提前返回并继续路由。

```js
import { NextResponse } from 'next/server';
 
return NextResponse.next();
```

您也可以在生成响应时转发 `headers` ：

```js
import { NextResponse } from 'next/server';
 
// Given an incoming request...
const newHeaders = new Headers(request.headers);
// Add a new header
newHeaders.set('x-version', '123');
// And produce a response with the new headers
return NextResponse.next({
  request: {
    // New request headers
    headers: newHeaders,
  },
});
```

