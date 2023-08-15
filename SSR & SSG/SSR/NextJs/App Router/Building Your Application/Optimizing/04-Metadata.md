# Metadata

Next.js有一个元数据 API，可用于定义应用程序元数据（例如 HTML `head` 元素中的 `meta` 和 `link` 标签，用于改进 SEO 和 Web 可共享性。

有两种方法可以将元数据添加到应用程序：

- **基于配置的元数据**：导出静态 `metadata` 对象或 `layout.js` 或 `page.js` 文件中的动态 `generateMetadata` 函数。
- **基于文件的Metadata**：将静态或动态生成的特殊文件添加到路由段。

使用这两个选项，Next.js 将自动生成页面的相关 `<head>` 元素。还可以使用  [`ImageResponse`](https://nextjs.org/docs/app/api-reference/functions/image-response) 构造函数创建动态 OG 图片。

## [Static Metadata](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#static-metadata)

To define static metadata, export a [`Metadata` object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadata-object) from a `layout.js` or static `page.js` file.
若要定义静态元数据，请从 `layout.js` 或静态 `page.js` 文件中导出 `Metadata` 对象。

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: '...',
  description: '...',
};
 
export default function Page() {}
```

## [Dynamic Metadata](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#dynamic-metadata)

You can use `generateMetadata` function to `fetch` metadata that requires dynamic values.
可以使用 `generateMetadata` 函数 `fetch` 需要动态值的元数据。

```tsx
import { Metadata, ResolvingMetadata } from 'next';
 
type Props = {
  params: { id: string };
  searchParams: { [key: string]: string | string[] | undefined };
};
 
export async function generateMetadata(
  { params, searchParams }: Props,
  parent?: ResolvingMetadata,
): Promise<Metadata> {
  // read route params
  const id = params.id;
 
  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json());
 
  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || [];
 
  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  };
}
 
export default function Page({ params, searchParams }: Props) {}
```

> **Good to know: **
>
> - 通过 `generateMetadata` 的静态和动态metadata仅在服务器组件中受支持**。
> - 渲染路由时，Next.js 将自动对 `generateMetadata` 、 `generateStaticParams` 、布局、页面和服务器组件的相同数据的 `fetch` 请求进行重复数据删除 [automatically deduplicate `fetch` requests](https://nextjs.org/docs/app/building-your-application/data-fetching#automatic-fetch-request-deduping)。如果 `fetch` 不可用，可以使用 React `cache` 。
> - Next.js将等待 `generateMetadata` 中的数据提取完成，然后再将 UI 流式传输到客户端。这可确保流响应的第一部分包含 `<head>` 标记。

## [基于文件的**metadata**](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#file-based-metadata)

这些特殊文件可用于metadata：

- [favicon.ico, apple-icon.jpg, and icon.jpg](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons)
- [opengraph-image.jpg and twitter-image.jpg](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image)
- [robots.txt](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots)
- [sitemap.xml](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap)

您可以将这些文件用于静态metadata，也可以使用代码以编程方式生成这些文件。

## [Behavior](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#behavior)

File-based metadata has the higher priority and will override any config-based metadata.
基于文件的metadata具有更高的优先级，并将覆盖任何基于配置的metadata。

### [Default Fields](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#default-fields)

即使路由未定义metadata，也始终会添加两个默认 `meta` 标记：

- 元字符集标记[meta charset tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-charset) 设置网站的字符编码。
- 元视口标签 [meta viewport tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag) 设置网站的视口宽度和比例，以针对不同的设备进行调整。

```
<meta charset="utf-8" /><meta name="viewport" content="width=device-width, initial-scale=1" />
```

> **Note**: 您可以覆盖默认的 `viewport` 元标记。

### [Ordering](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#ordering)

Metadata is evaluated in order, starting from the root segment down to the segment closest to the final `page.js` segment. For example:
元数据按顺序进行评估，从根段开始，到最接近最终 `page.js` 段的段。例如：

1. `app/layout.tsx` (Root Layout) 
2. `app/blog/layout.tsx` (Nested Blog Layout)
3. `app/blog/[slug]/page.tsx` (Blog Page) 

### [Merging](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#merging)

按照求值顺序，从同一路由中的多个段导出的Metadata对象被**浅浅的**（shallowly）合并在一起，形成路由的最终元数据输出。重复的键将根据其顺序进行**替换**。

这意味着在较早段中定义的具有嵌套字段（如[`openGraph`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#opengraph) 和 [`robots`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#robots) ）的metadata将被最后一个段**覆盖**以定义它们。

#### [覆盖字段](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#overwriting-fields)

```js
export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...',
  },
};
```

```js
export const metadata = {
  title: 'Blog',
  openGraph: {
    title: 'Blog',
  },
};
 
// Output:
// <title>Blog</title>
// <meta property="og:title" content="Blog" />
```

In the example above: 在上面的示例中：

- `app/layout.js` 中的 `title`替换为 `app/blog/page.js`中的`title` 。
- `app/layout.js` 中的所有 `openGraph` 字段都将替换为 `app/blog/page.js` ，因为 `app/blog/page.js` 设置了 `openGraph` 元数据。请注意缺少了`openGraph.description` 。

如果您想在段之间共享一些嵌套字段，同时覆盖其他嵌套字段，您可以将它们拉出到一个单独的变量中：

```js
export const openGraphImage = { images: ['http://...'] };
```

```js
import { openGraphImage } from './shared-metadata';
 
export const metadata = {
  openGraph: {
    ...openGraphImage,
    title: 'Home',
  },
};
```

```js
import { openGraphImage } from '../shared-metadata';
 
export const metadata = {
  openGraph: {
    ...openGraphImage,
    title: 'About',
  },
};
```

在上面的示例中，OG 图像在 `app/layout.js` 和 `app/about/page.js` 之间共享，而titles不同。

```js
export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...',
  },
};
```

```js
export const metadata = {
  title: 'About',
};
 
// Output:
// <title>About</title>
// <meta property="og:title" content="Acme" />
// <meta property="og:description" content="Acme is a..." />
```

**Notes**

- `app/layout.js` 中的 `title` 替换为`app/about/page.js` 中的 `title` 。
- `app/layout.js` 中的所有 `openGraph` 字段都在 `app/about/page.js` 中继承，因为 `app/about/page.js` 没有设置 `openGraph` 元数据

## [Dynamic Image Generation 动态图像生成](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#dynamic-image-generation)

`ImageResponse` 构造函数允许您使用 JSX 和 CSS 生成动态图像。这对于创建社交媒体图像（如开放图形图像、Twitter 卡片等）非常有用。

`ImageResponse` 使用 Edge 运行时，Next.js 会自动将正确的header添加到edge的缓存图像，从而帮助提高性能并减少重新计算。

To use it, you can import `ImageResponse` from `next/server`:
要使用它，您可以从 `next/server` 导入 `ImageResponse` ：

```jsx
import { ImageResponse } from 'next/server';
 
export const runtime = 'edge';
 
export async function GET() {
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          textAlign: 'center',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        Hello world!
      </div>
    ),
    {
      width: 1200,
      height: 600,
    },
  );
}
```

`ImageResponse` 与其他 Next.js API 很好地集成，包括路由处理程序和基于文件的元数据。例如，可以在 `opengraph-image.tsx` 文件中使用 `ImageResponse` 在生成时生成 Open Graph 图像，或在请求时动态生成 Open Graph 图像。

`ImageResponse` 支持常见的 CSS 属性，包括弹性框和绝对定位、自定义字体、文本环绕、居中和嵌套图像。请参阅支持的 CSS 属性的完整列表。

> **Good to know：**
>
> - 示例可在Vercel OG Playground中找到。
> - `ImageResponse` 使用 @vercel/og、Satori 和 Resvg 将 HTML 和 CSS 转换为 PNG。
> - 仅支持边缘运行时。默认的 Node.js 运行时将不起作用。
> - 仅支持flex和 CSS 属性的子集。高级布局（例如 `display: grid` ）将不起作用。
> - 最大捆绑包大小为 `500KB` 。捆绑包大小包括您的 JSX、CSS、字体、图像和任何其他资产。如果超出限制，请考虑减小任何资产的大小或在运行时提取。
> - 仅支持 `ttf` 、 `otf` 和 `woff` 种字体格式。为了最大限度地提高字体解析速度， `ttf` 或 `otf` 优先于 `woff` 。

## [JSON-LD](https://nextjs.org/docs/app/building-your-application/optimizing/metadata#json-ld)

JSON-LD 是一种结构化数据格式，搜索引擎可以使用它来理解您的内容。例如，您可以使用它来描述人员、事件、组织、电影、书籍、食谱和许多其他类型的实体。

我们目前对 JSON-LD 的建议是在 `layout.js` 或 `page.js` 组件中将结构化数据渲染为 `<script>` 标记。例如：

```tsx
export default async function Page({ params }) {
  const product = await getProduct(params.id);
 
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'Product',
    name: product.name,
    image: product.image,
    description: product.description,
  };
 
  return (
    <section>
      {/* Add JSON-LD to your page */}
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      {/* ... */}
    </section>
  );
}
```

