# route.js

Route Handlers allow you to create custom request handlers for a given route using the Web [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) and [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) APIs.
路由处理程序允许您使用 Web 请求和响应 API 为给定路由创建自定义请求处理程序。

## [HTTP Methods](https://nextjs.org/docs/app/api-reference/file-conventions/route#http-methods)

路由文件允许您为给定路由创建自定义请求处理程序。支持以下 HTTP 方法： `GET` 、 `POST` 、 `PUT` 、 `PATCH` 、 `DELETE` 、 `HEAD` 和 `OPTIONS` 。

```ts
export async function GET(request: Request) {}
 
export async function HEAD(request: Request) {}
 
export async function POST(request: Request) {}
 
export async function PUT(request: Request) {}
 
export async function DELETE(request: Request) {}
 
export async function PATCH(request: Request) {}
 
// If `OPTIONS` is not defined, Next.js will automatically implement `OPTIONS` and  set the appropriate Response `Allow` header depending on the other methods defined in the route handler.
export async function OPTIONS(request: Request) {}
```

> 温馨提示：路由处理程序仅在 `app` 目录中可用。您**不需要同时使用 API 路由 （ `pages` ） 和路由处理程序 （ `app` ）**，因为路由处理程序应该能够处理所有用例。

## [Parameters](https://nextjs.org/docs/app/api-reference/file-conventions/route#parameters)

### [`request` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/route#request-optional)

The `request` object is a [NextRequest](https://nextjs.org/docs/app/api-reference/functions/next-request) object, which is an extension of the Web [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) API. `NextRequest` gives you further control over the incoming request, including easily accessing `cookies` and an extended, parsed, URL object `nextUrl`.
`request` 对象是 NextRequest 对象，它是 Web 请求 API 的扩展。 `NextRequest` 使您可以进一步控制传入的请求，包括轻松访问 `cookies` 和扩展的、解析的 URL 对象 `nextUrl` 。

### [`context` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/route#context-optional)

```js
export async function GET(request, context: { params }) {
  const team = params.team; // '1'
}
```

目前， `context` 的唯一值是 `params` ，它是一个包含当前路由的动态路由参数的对象。

![image-20230526172352675](/Users/jerry/Library/Application Support/typora-user-images/image-20230526172352675.png)

## [NextResponse](https://nextjs.org/docs/app/api-reference/file-conventions/route#nextresponse)

Route Handlers can extend the Web Response API by returning a `NextResponse` object. This allows you to easily set cookies, headers, redirect, and rewrite. [View the API reference](https://nextjs.org/docs/app/api-reference/functions/next-response).
路由处理程序可以通过返回 `NextResponse` 对象来扩展 Web 响应 API。这使您可以轻松设置 Cookie、标头、重定向和重写。 查看 API 参考 。