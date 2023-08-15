# sitemap.xml

在 `app` 目录的根目录中添加或生成与sitemap XML 格式匹配的 `sitemap.xml` 文件，以帮助爬虫工具更有效地抓取您的网站。

## [Static `sitemap.xml`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#static-sitemapxml)

```xml
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
</urlset>
```

## [生成站点地图](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#generate-a-sitemap)

添加返回 `Sitemap` 的 `sitemap.js` 或 `sitemap.ts` 文件。

```ts
import { MetadataRoute } from 'next';
 
export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://acme.com',
      lastModified: new Date(),
    },
    {
      url: 'https://acme.com/about',
      lastModified: new Date(),
    },
    {
      url: 'https://acme.com/blog',
      lastModified: new Date(),
    },
  ];
}
```

Output:

```xml
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
</urlset>
```

### [站点地图返回类型](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-return-type)

```tsx
type Sitemap = Array<{
  url: string;
  lastModified?: string | Date;
}>;
```

> **Good to know**
>
> - 将来，我们将支持多个站点地图和站点地图索引。