# favicon, apple-icon, and icon 网站图标、苹果图标和图标

`favicon` 、 `apple-icon` 或 `icon` 文件约定允许您为应用程序设置图标。

它们可用于添加显示在 Web 浏览器选项卡、电话主屏幕和搜索引擎结果等位置的应用图标。

有两种方法可以设置应用图标：

- [Using image files (.ico, .jpg, .png)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#image-files-ico-jpg-png)
- [Using code to generate an icon (.js, .ts, .tsx)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)

## [mage files (.ico, .jpg, .png)](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#image-files-ico-jpg-png)

使用图像文件通过将 `favicon` 、 `icon` 或 `apple-icon` 图像文件放置在根 `/app` 段来设置应用图标。

Next.js将评估该文件，并自动将相应的标记添加到应用的 `<head>` 元素。

![image-20230527163802044](/Users/jerry/Library/Application Support/typora-user-images/image-20230527163802044.png)

### [`favicon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#favicon)

将 `favicon.ico` 图像文件添加到根 `/app` 路由段。

```
<link rel="icon" href="/favicon.ico" sizes="any" />
```

### [`icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)

将 `icon.(ico|jpg|jpeg|png|svg)` 图像文件添加到根 `/app` 路由段。

```html
<link
  rel="icon"
  href="/icon?<generated>"
  type="image/<generated>"
  sizes="<generated>"
/>
```

### [`apple-icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#apple-icon)

将 `apple-icon.(jpg|jpeg|png)` 图像文件添加到根 `/app` 路由段。

```html
<link
  rel="apple-touch-icon"
  href="/apple-icon?<generated>"
  type="image/<generated>"
  sizes="<generated>"
/>
```

> **Good to know 很高兴知道**
>
> - 您可以通过在文件名中添加数字后缀来设置多个图标。例如， `icon1.png` 、 `icon2.png` 等。编号文件将按词法排序。
> - 只能在根 `/app` 段中设置应用图标。
> - 相应的 `<link>` 标记和属性（如 `rel` 、 `href` 、 `type` 和 `sizes` ）由评估文件的图标类型和元数据确定。
>   - 例如，一个 32 x 32px `.png` 的文件将具有 `type="image/png"` 和 `sizes="32x32"` 属性。
> - `sizes="any"` is added to `favicon.ico` output to [avoid a browser bug](https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs) where an `.ico` icon is favored over `.svg`.
>   ``sizes="any"` 被添加到 `favicon.ico` 输出以避免浏览器错误，其中 `.ico` 图标优先于 `.svg` 。

## [使用代码生成图标（.js、.ts、.tsx）](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)

除了使用文本图像文件之外，还可以使用代码以编程方式生成图标。

通过创建默认导出函数的 `icon` 或 `apple-icon` 路由来生成应用程序图标。

![image-20230527164316155](/Users/jerry/Library/Application Support/typora-user-images/image-20230527164316155.png)

生成图标的最简单方法是使用 `next/server` 中的 `ImageResponse` API。

```tsx
import { ImageResponse } from 'next/server';
 
// Route segment config
export const runtime = 'edge';
 
// Image metadata
export const size = {
  width: 32,
  height: 32,
};
export const contentType = 'image/png';
 
// Image generation
export default function Icon() {
  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 24,
          background: 'black',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          color: 'white',
        }}
      >
        A
      </div>
    ),
    // ImageResponse options
    {
      // For convenience, we can re-use the exported icons size metadata
      // config to also set the ImageResponse's width and height.
      ...size,
    },
  );
}
```

```html
<link rel="icon" href="/icon?<generated>" type="image/png" sizes="32x32" />
```

> **Good to know 很高兴知道**
>
> - 默认情况下，生成的图标是静态优化的（在构建时生成并缓存），除非它们使用动态函数或动态数据提取。
> - 您可以使用 `generateImageMetadata` 在同一文件中生成多个图标。
> - 不能生成 `favicon` 图标。请改用 `1 或 favicon`.ico 文件。

### [Props](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#props)

默认导出函数接收以下属性：

#### [`params` (optional) `params` （可选）](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#params-optional)

包含从根段向下到 `icon` 或 `apple-icon` 段的动态路由参数对象的对象被共置。

```tsx
export default function Icon({ params }: { params: { slug: string } }) {
  // ...
}
```

![image-20230527164836756](/Users/jerry/Library/Application Support/typora-user-images/image-20230527164836756.png)

### [Returns](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#returns)

默认导出函数应返回 `Blob` | `ArrayBuffer` | `TypedArray` | `DataView` | `ReadableStream` | `Response` .

> **Note:** `ImageResponse` 满足此返回类型。

### [Config exports](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#config-exports)

您可以选择通过从 `icon` 或 `apple-icon` 路由导出 `size` 和 `contentType` 变量来配置图标的元数据。

![image-20230527165653840](/Users/jerry/Library/Application Support/typora-user-images/image-20230527165653840.png)

#### [`size`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#size)

```js
export const size = { width: 32, height: 32 };
 
export default function Icon() {}
```

```html
<link rel="icon" sizes="32x32" />
```

#### [`contentType`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#contenttype)

```js
export const contentType = 'image/png';
 
export default function Icon() {}
```

```html
<link rel="icon" type="image/png" />
```

#### [Route Segment Config 路由段配置](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#route-segment-config)

`icon` and `apple-icon` are specialized [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/router-handlers) that can use the same [route segment configuration](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config) options as Pages and Layouts.
`icon` 和 `apple-icon` 是专用路由处理程序，可以使用与页面和布局相同的路由段配置选项。

![image-20230527170040821](/Users/jerry/Library/Application Support/typora-user-images/image-20230527170040821.png)

```js
export const runtime = 'edge';
 
export default function Icon() {}
```

