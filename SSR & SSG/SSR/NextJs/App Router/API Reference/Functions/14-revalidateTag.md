# revalidateTag

`revalidateTag` 允许您重新验证与特定缓存标记关联的数据。这对于想要更新缓存数据而不等待重新验证期到期的情况非常有用

```ts
import { NextRequest, NextResponse, revalidateTag } from 'next/server';
 
export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

> **Good to know: 很高兴知道：**
>
> - `revalidateTag` 在 Node.js 和 Edge 运行时中均可用。

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#parameters)

```js
revalidateTag(tag: string): void;
```

- `tag` ：表示与要重新验证的数据关联的缓存标记的字符串。

You can add tags to `fetch` as follows:
您可以按如下方式将标签添加到 `fetch` ：

```js
fetch(url, { next: { tags: [...] } });
```

## [Returns](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#returns)

`revalidateTag` 不返回任何值。

## [Examples](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#examples)

### [Node.js Runtime](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#nodejs-runtime)

```ts
import { NextRequest, NextResponse } from 'next/server';
import { revalidateTag } from 'next/cache';
 
export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

### [Edge Runtime](https://nextjs.org/docs/app/api-reference/functions/revalidateTag#edge-runtime)

```ts
import { NextRequest, NextResponse } from 'next/server';
import { revalidateTag } from 'next/cache';
 
export const runtime = 'edge';
 
export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

