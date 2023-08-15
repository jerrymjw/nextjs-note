# generateImageMetadata

您可以使用 `generateImageMetadata` 生成一个图像的不同版本，也可以为一个路由段返回多个图像。当您想要避免硬编码（hard-coding）Metadata值（例如图标）时，这很有用。

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata#parameters)

`generateMetadata` 函数接受以下参数：

#### [`params` (optional) `params` （可选）](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata#params-optional)

一个包含从根段向下到 `generateImageMetadata` 段调用的[dynamic route parameters](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)对象的对象

```tsx
export function generateImageMetadata({
  params,
}: {
  params: { slug: string };
}) {
  // ...
}
```

![image-20230530145343016](/Users/jerry/Library/Application Support/typora-user-images/image-20230530145343016.png)

## [Returns](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata#returns)

`generateImageMetadata` 函数应返回包含图像Metadata（如 `alt` 和 `size` ）的对象的 `array` 。此外，每个项目**必须**包含一个 `id` 值，将传递给 props 的图像生成函数。

![image-20230530154722639](/Users/jerry/Library/Application Support/typora-user-images/image-20230530154722639.png)

```tsx
import { ImageResponse } from 'next/server';
 
export function generateImageMetadata() {
  return [
    {
      contentType: 'image/png',
      size: { width: 48, height: 48 },
      id: 'small',
    },
    {
      contentType: 'image/png',
      size: { width: 72, height: 72 },
      id: 'medium',
    },
  ];
}
 
export default function Icon({ id }: { id: string }) {
  return new ImageResponse(
    (
      <div
        style={{
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          fontSize: 88,
          background: '#000',
          color: '#fafafa',
        }}
      >
        Icon {id}
      </div>
    ),
  );
}
```

### [Examples](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata#examples)

#### [Using external data](https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata#using-external-data)

This example uses the `params` object and external data to generate multiple [Open Graph images](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image) for a route segment.
此示例使用 `params` 对象和外部数据为路径段生成多个[Open Graph images](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image)图像。

