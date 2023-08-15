# robots.txt

在 `app` 目录的根目录中添加或生成一个与漫游器排除标准匹配的 `robots.txt` 文件，以告诉爬虫工具他们可以在您的网站上访问哪些网址。

## [Static `robots.txt`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#static-robotstxt)

```txt
User-Agent: *
Allow: /
Disallow: /private/

Sitemap: https://acme.com/sitemap.xml
```

## [Generate a Robots file 生成机器人文件](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#generate-a-robots-file)

添加返回 `Robots` 对象的 `robots.js` 或 `robots.ts` 文件。

```tsx
import { MetadataRoute } from 'next';
 
export default function robots(): MetadataRoute.Robots {
  return {
    rules: {
      userAgent: '*',
      allow: '/',
      disallow: '/private/',
    },
    sitemap: 'https://acme.com/sitemap.xml',
  };
}
```

Output:

```txt
User-Agent: *
Allow: /
Disallow: /private/

Sitemap: https://acme.com/sitemap.xml
```

### [Robots object](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#robots-object)

```tsx
type Robots = {
  rules:
    | {
        userAgent?: string | string[];
        allow?: string | string[];
        disallow?: string | string[];
        crawlDelay?: number;
      }
    | Array<{
        userAgent: string | string[];
        allow?: string | string[];
        disallow?: string | string[];
        crawlDelay?: number;
      }>;
  sitemap?: string | string[];
  host?: string;
};
```

