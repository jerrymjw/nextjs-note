# Static Exports

Next.js允许从静态站点或单页应用程序 （SPA） 开始，然后可以选择升级以使用需要服务器的功能。

运行 `next build` 时，Next.js 为每个路由生成一个 HTML 文件。通过将严格的 SPA 分解为单独的 HTML 文件，Next.js 可以避免在客户端加载不必要的 JavaScript 代码，从而减小捆绑包大小并实现更快的页面加载。

由于 Next.js 支持此静态导出，因此它可以部署和托管在任何可以提供 HTML/CSS/JS 静态资产的 Web 服务器上。

## [Configuration](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#configuration)

要启用静态导出，请在 `next.config.js` 内更改输出模式：

```js
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  output: 'export',
  // Optional: Add a trailing slash to all paths `/about` -> `/about/`
  // trailingSlash: true,
  // Optional: Change the output directory `out` -> `dist`
  // distDir: 'dist',
};
 
module.exports = nextConfig;
```

运行 `next build` 后，Next.js 将生成一个 `out` 文件夹，其中包含应用程序的 HTML/CSS/JS 资产。

## [Supported Features 支持的功能](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#supported-features)

Next.js的核心旨在支持静态导出。

### [Server Components](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#server-components)

When you run `next build` to generate a static export, Server Components consumed inside the `app` directory will run during the build, similar to traditional static-site generation.
运行 `next build` 以生成静态导出时，在 `app` 目录中使用的服务器组件将在生成期间运行，类似于传统的静态站点生成。

生成的组件将渲染为初始页面加载的静态 HTML 和客户端在路由之间导航的静态有效负载。使用静态导出时，服务器组件不需要进行任何更改，除非它们使用动态服务器函数 [dynamic server functions](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#unsupported-features)。

```tsx
export default async function Page() {
  // This fetch will run on the server during `next build`
  const res = await fetch('https://api.example.com/...');
  const data = await res.json();
 
  return <main>...</main>;
}
```

### [Client Components](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#client-components)

如果要在客户端上执行数据获取，可以使用具有 SWR 的客户端组件对请求进行重复数据删除。

```tsx
'use client';
 
import useSWR from 'swr';
 
const fetcher = (url: string) => fetch(url).then((r) => r.json());
 
export default function Page() {
  const { data, error } = useSWR(
    `https://jsonplaceholder.typicode.com/posts/1`,
    fetcher,
  );
  if (error) return 'Failed to load';
  if (!data) return 'Loading...';
 
  return data.title;
}
```

由于路由转换发生在客户端，因此其行为类似于传统的 SPA。例如，以下索引路由允许您导航到客户端上的不同post：

```tsx
import Link from 'next/link';
 
export default function Page() {
  return (
    <>
      <h1>Index Page</h1>
      <hr />
      <ul>
        <li>
          <Link href="/post/1">Post 1</Link>
        </li>
        <li>
          <Link href="/post/2">Post 2</Link>
        </li>
      </ul>
    </>
  );
}
```

### [Image Optimization 图像优化](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#image-optimization)

[Image Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/images) through `next/image` can be used with a static export by defining a custom image loader in `next.config.js`. For example, you can optimize images with a service like Cloudinary:
通过在 `next.config.js` 中定义自定义图像加载器，可以通过 `next/image` 进行图像优化[Image Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/images) 与静态导出一起使用。例如，您可以使用Cloudinary等服务优化图像：

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',
  images: {
    loader: 'custom',
    loaderFile: './app/image.ts',
  },
};
 
module.exports = nextConfig;
```

此自定义加载程序将定义如何从远程源获取图像。例如，以下加载程序将为 Cloudinary 构造 URL：

```ts
export default function cloudinaryLoader({
  src,
  width,
  quality,
}: {
  src: string;
  width: number;
  quality?: number;
}) {
  const params = ['f_auto', 'c_limit', `w_${width}`, `q_${quality || 'auto'}`];
  return `https://res.cloudinary.com/demo/image/upload/${params.join(
    ',',
  )}${src}`;
}
```

然后，您可以在应用程序中使用 `next/image` ，在 Cloudinary 中定义图像的相对路径：

```tsx
import Image from 'next/image';
 
export default function Page() {
  return <Image alt="turtles" src="/turtles.jpg" width={300} height={300} />;
}
```

### [Route Handlers](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#route-handlers)

Route Handlers will render a static response when running `next build`. Only the `GET` HTTP verb is supported. This can be used to generate static HTML, JSON, TXT, or other files from dynamic or static data. For example:
路由处理程序将在运行 `next build` 时呈渲染静态响应。仅支持 `GET` HTTP 谓词。这可用于生成静态 HTML、JSON、TXT 或从动态或静态数据生成其他文件。例如：

```tsx
import { NextResponse } from 'next/server';
 
export async function GET() {
  return NextResponse.json({ name: 'Lee' });
}
```

上面的文件 `app/data.json/route.ts` 将在 `next build` 期间渲染为静态文件，生成包含 `{ name: 'Lee' }` 的 `data.json` 。

如果需要从传入请求中读取动态值，则不能使用静态导出。

### [Browser APIs](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#browser-apis)

客户端组件在 `next build` 期间预呈现为 HTML。由于 `window` 、 `localStorage` 和 `navigator` 等 Web API 在服务器上不可用，因此只有在浏览器中运行时，才需要安全地访问这些 API。例如：

```tsx
'use client';
 
import { useEffect } from 'react';
 
export default function ClientComponent() {
  useEffect(() => {
    // You now have access to `window`
    console.log(window.innerHeight);
  }, [])
 
  return ...;
}
```

## [Unsupported Features 不支持的功能](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#unsupported-features)

启用静态导出 `output` 模式后， `app` 内的所有路由都将选择加入以下路由段配置[Route Segment Config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config):

```tsx
export const dynamic = 'error';
```

使用此配置，如果尝试使用 `headers` 或 `cookies` 等服务器函数，应用程序将生成错误，因为没有运行时服务器。这可确保本地开发与静态导出匹配相同的行为。如果需要使用服务器函数，则不能使用静态导出。

静态导出不支持以下附加动态要素：

- `rewrites` in `next.config.js`
- `redirects` in `next.config.js`
- `headers` in `next.config.js`
- Middleware
- [Incremental Static Regeneration](https://nextjs.org/docs/app/building-your-application/data-fetching/revalidating)

## [Deploying](https://nextjs.org/docs/app/building-your-application/deploying/static-exports#deploying)

通过静态导出，Next.js 可以部署和托管在任何可以提供 HTML/CSS/JS 静态资产的 Web 服务器上。

运行 `next build` 时，Next.js 将静态导出生成到 `out` 文件夹中。不再需要使用 `next export` 。例如，假设您有以下路由：

- `/`
- `/blog/[id]`

运行 `next build` 后，Next.js 将生成以下文件：

- `/out/index.html`
- `/out/404.html`
- `/out/blog/post-1.html`
- `/out/blog/post-2.html`

如果您使用的是像 Nginx 这样的静态主机，则可以配置从传入请求到正确文件的重写：

```conf
server {
  listen 80;
  server_name acme.com;
 
  root /var/www;
 
  location / {
      try_files /out/index.html =404;
  }
 
  location /blog/ {
      rewrite ^/blog/(.*)$ /out/blog/$1.html break;
  }
 
  error_page 404 /out/404.html;
  location = /404.html {
      internal;
  }
}
```

