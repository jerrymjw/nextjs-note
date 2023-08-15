# NextRequest

NextRequest 通过其他方便的方法扩展了 [Web Request API](https://developer.mozilla.org/en-US/docs/Web/API/Request) 。

## [`cookies`](https://nextjs.org/docs/app/api-reference/functions/next-request#cookies)

读取或更改请求的 [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) header 。

### [`set(name, value)`](https://nextjs.org/docs/app/api-reference/functions/next-request#setname-value)

给定一个名称，在请求中设置具有给定值的 cookie。

```js
// Given incoming request /home
// Set a cookie to hide the banner
// request will have a `Set-Cookie:show-banner=false;path=/home` header
request.cookies.set('show-banner', 'false');
```

### [`get(name)`](https://nextjs.org/docs/app/api-reference/functions/next-request#getname)

给定一个 cookie 名称，返回 cookie 的值。如果未找到 cookie，则返回 `undefined` 。如果找到多个 Cookie，则返回第一个 Cookie。

```js
// Given incoming request /home
// { name: 'show-banner', value: 'false', Path: '/home' }
request.cookies.get('show-banner');
```

### [`getAll()`](https://nextjs.org/docs/app/api-reference/functions/next-request#getall)

给定一个 cookie 名称，返回 cookie 的值。如果未指定名称，则返回请求中的所有 cookie。

```js
// Given incoming request /home
// [
//   { name: 'experiments', value: 'new-pricing-page', Path: '/home' },
//   { name: 'experiments', value: 'winter-launch', Path: '/home' },
// ]
request.cookies.getAll('experiments');
// Alternatively, get all cookies for the request
request.cookies.getAll();
```

### [`delete(name)`](https://nextjs.org/docs/app/api-reference/functions/next-request#deletename)

给定 Cookie 名称，从请求中删除该 Cookie。

```js
// Returns true for deleted, false is nothing is deleted
request.cookies.delete('experiments');
```

### [`has(name)`](https://nextjs.org/docs/app/api-reference/functions/next-request#hasname)

给定 cookie 名称，如果请求中存在 cookie，则返回 `true` 。

```js
// Returns true if cookie exists, false if it does not
request.cookies.has('experiments');
```

### [`clear()`](https://nextjs.org/docs/app/api-reference/functions/next-request#clear)

Remove the `Set-Cookie` header from the request.
从请求中删除 `Set-Cookie` header。

## [`nextUrl`](https://nextjs.org/docs/app/api-reference/functions/next-request#nexturl)

使用其他方便的方法（包括 Next.js 特定属性）扩展原生 `URL` API。