

# opengraph-image and twitter-image

`opengraph-image` 和 `twitter-image` 文件约定允许您为路由段设置Open Graph和 Twitter images。

它们可用于设置当用户共享指向您网站的链接时显示在social networks 和messaging apps上的图像。

有两种方法可以设置Open Graph和Twitter图像：

- [Using image files (.jpg, .png, .gif)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif)
- [Using code to generate images (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx)

## [Image files (.jpg, .png, .gif)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif)

使用图像文件通过在路由中放置 `opengraph-image` 或 `twitter-image` 图像文件来设置路由段的共享图像。

Next.js将评估文件并自动将相应的标记添加到应用的 `<head>` 元素中。

![image-20230527173045905](/Users/jerry/Library/Application Support/typora-user-images/image-20230527173045905.png)

### [`opengraph-image`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-image)

将 `opengraph-image.(jpg|jpeg|png|gif)` 图像文件添加到任何路由段。

```html
<meta property="og:image" content="<generated>" />
<meta property="og:image:type" content="<generated>" />
<meta property="og:image:width" content="<generated>" />
<meta property="og:image:height" content="<generated>" />
```

### [`twitter-image`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-image)

将 `twitter-image.(jpg|jpeg|png|gif)` 图像文件添加到任何路由段

```html
<meta name="twitter:image" content="<generated>" />
<meta name="twitter:image:type" content="<generated>" />
<meta name="twitter:image:width" content="<generated>" />
<meta name="twitter:image:height" content="<generated>" />
```

### [`opengraph-image.alt.txt`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-imagealttxt)

在与 `opengraph-image.(jpg|jpeg|png|gif)` 图像相同的路由段中添加随同的 `opengraph-image.alt.txt` 文件，该文件是替代文本。

```txt
About Acme
```

```html
<meta property="og:image:alt" content="About Acme" />
```

### [`twitter-image.alt.txt`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-imagealttxt)

在与 `twitter-image.(jpg|jpeg|png|gif)` 图像相同的路由段中添加随同的 `twitter-image.alt.txt` 文件，该文件是替代文本。

```txt
About Acme
```

```html
<meta property="og:image:alt" content="About Acme" />
```

## [Generate images using code (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx)

除了使用文本图像文件之外，还可以使用代码以编程方式生成图像。

通过创建默认导出函数的 `opengraph-image` 或 `twitter-image` 路由来生成路由段的共享映像。

![image-20230527174033269](/Users/jerry/Library/Application Support/typora-user-images/image-20230527174033269.png)

> **Good to know**
>
> - 默认情况下，生成的图像是**静态优化**的（在构建时生成并缓存），除非它们使用**动态函数**或**动态数据获取**。
> - 您可以使用[`generateImageMetadata`](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata)在同一文件中生成多个图像。

生成图像的最简单方法是使用 `next/server` 里的 [ImageResponse](https://nextjs.org/docs/app/api-reference/functions/image-response)。

```tsx
import { ImageResponse } from 'next/server';
 
// Route segment config
export const runtime = 'edge';
 
// Image metadata
export const alt = 'About Acme';
export const size = {
  width: 1200,
  height: 630,
};
 
export const contentType = 'image/png';
 
// Font
const interSemiBold = fetch(
  new URL('./Inter-SemiBold.ttf', import.meta.url),
).then((res) => res.arrayBuffer());
 
// Image generation
export default function Image() {
  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        About Acme
      </div>
    ),
    // ImageResponse options
    {
      // For convenience, we can re-use the exported opengraph-image
      // size config to also set the ImageResponse's width and height.
      ...size,
      fonts: [
        {
          name: 'Inter',
          data: await interSemiBold,
          style: 'normal',
          weight: 400,
        },
      ],
    },
  );
}
```

```html
<meta property="og:image" content="<generated>" />
<meta property="og:image:alt" content="About Acme" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

### [Props](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#props)

默认导出函数接收以下属性：

#### [`params` (optional)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#params-optional)

包含从根段向下到 `opengraph-image` 或 `twitter-image` 段的动态路由参数对象的对象被共置。

```tsx
export default function Image({ params }: { params: { slug: string } }) {
  // ...
}
```

### [Returns](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#returns)

默认导出函数应返回 `Blob` | `ArrayBuffer` | `TypedArray` | `DataView` | `ReadableStream` | `Response` .

> **Note**： `ImageResponse` 满足此返回类型。

### [Config exports](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#config-exports)

您可以选择通过从 `opengraph-image` 或 `twitter-image` 路由导出 `alt` 、 `size` 和 `contentType` 变量来配置图像的metadata。

![image-20230527174814544](/Users/jerry/Library/Application Support/typora-user-images/image-20230527174814544.png)

#### [`alt`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#alt)

```tsx
export const alt = 'My images alt text';
 
export default function Image() {}
```

```html
<meta property="og:image:alt" content="My images alt text" />
```

#### [`size`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#size)

```tsx
export const size = { width: 1200, height: 630 };
 
export default function Image() {}
```

```html
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

#### [`contentType`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#contenttype)

```tsx
export const contentType = 'image/png';
 
export default function Image() {}
```

```html
<meta property="og:image:type" content="image/png" />
```

#### [Route Segment Config 路由段配置](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#route-segment-config)

`opengraph-image` and `twitter-image` are specialized [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/router-handlers) that can use the same [route segment configuration](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config) options as Pages and Layouts.
`opengraph-image` 和 `twitter-image` 是专用路由处理程序，可以使用与页面和布局相同的路由段配置选项。

![image-20230527175247050](/Users/jerry/Library/Application Support/typora-user-images/image-20230527175247050.png)

```tsx
export const runtime = 'edge';
 
export default function Image() {}
```

### [Examples](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#examples)

#### [Using external data 使用外部数据](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#using-external-data)

此示例使用 `params` 对象和外部数据生成图像。

> **Good to know:** 默认情况下，此生成的图像将进行**静态优化**。您可以配置单个 `fetch` `options` 或路由段options以更改此行为。

```tsx
import { ImageResponse } from 'next/server';
 
export const runtime = 'edge';
 
export const alt = 'About Acme';
export const size = {
  width: 1200,
  height: 630,
};
export const contentType = 'image/png';
 
export default async function Image({ params }: { params: { slug: string } }) {
  const post = await fetch(`https://.../posts/${params.slug}`).then((res) =>
    res.json(),
  );
 
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 48,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    ),
    {
      ...size,
    },
  );
}
```

