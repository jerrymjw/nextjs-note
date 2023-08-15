# page.js

**页面**是路由独有的 UI。

```tsx
export default function Page({
  params,
  searchParams,
}: {
  params: { slug: string };
  searchParams: { [key: string]: string | string[] | undefined };
}) {
  return <h1>My Page</h1>;
}
```

## [Props](https://nextjs.org/docs/app/api-reference/file-conventions/page#props)

### [`params` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/page#params-optional)

An object containing the [dynamic route parameters](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes) from the root segment down to that page. For example:
包含从根段向下到该页面的动态路由参数的对象。例如：

![image-20230526170807489](/Users/jerry/Library/Application Support/typora-user-images/image-20230526170807489.png)

### [`searchParams` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/page#searchparams-optional)

An object containing the [search parameters](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL#parameters) of the current URL. For example:
包含当前 URL 的搜索参数的对象。例如：

![image-20230526170904013](/Users/jerry/Library/Application Support/typora-user-images/image-20230526170904013.png)

> **Good to know: 很高兴知道：**
>
> - `searchParams` 是一个**[Dynamic API](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)** ，其值无法提前知道。使用它将在请求时选择页面 **[dynamic rendering](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-rendering)** 。
> - `searchParams` 返回一个普通的 JavaScript 对象，而不是一个 `URLSearchParams` 实例。
