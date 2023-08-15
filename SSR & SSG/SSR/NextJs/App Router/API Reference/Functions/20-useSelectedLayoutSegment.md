# useSelectedLayoutSegment

`useSelectedLayoutSegment` 是一个客户端组件挂钩，允许您读取活动路由段，该路由段位于调用它的布局的**下一级**。

它对于导航 UI 非常有用，例如父布局中的选项卡，这些选项卡根据活动的子段更改样式。

```tsx
'use client';
 
import { useSelectedLayoutSegment } from 'next/navigation';
 
export default function ExampleClientComponent() {
  const segment = useSelectedLayoutSegment();
 
  return <>Active segment: {segment}</>;
}
```

> **Good to know: 很高兴知道：**
>
> - 由于 `useSelectedLayoutSegment` 是客户端组件挂钩，而布局默认是服务器组件，因此通常通过导入到布局中的客户端组件调用 `useSelectedLayoutSegment` 。
> - `useSelectedLayoutSegment` 仅返回下一级的段。要返回所有活动段，请参阅 `useSelectedLayoutSegments`

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment#parameters)

```js
const segment = useSelectedLayoutSegment();
```

`useSelectedLayoutSegment` 不采用任何参数。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment#returns)

`useSelectedLayoutSegment` 返回活动段的字符串，如果不存在，则返回 `null` 。

For example, given the Layouts and URLs below, the returned segment would be:
例如，给定下面的布局和 URL，返回的段将是：

| Layout                    | Visited URL                    | Returned Segment |
| ------------------------- | ------------------------------ | ---------------- |
| `app/layout.js`           | `/`                            | `null`           |
| `app/layout.js`           | `/dashboard`                   | `'dashboard'`    |
| `app/dashboard/layout.js` | `/dashboard`                   | `null`           |
| `app/dashboard/layout.js` | `/dashboard/settings`          | `'settings'`     |
| `app/dashboard/layout.js` | `/dashboard/analytics`         | `'analytics'`    |
| `app/dashboard/layout.js` | `/dashboard/analytics/monthly` | `'analytics'`    |

## [Examples](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment#examples)

### [Creating an active link component](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment#creating-an-active-link-component)

您可以使用 `useSelectedLayoutSegment` 创建活动链接组件，该组件根据活动段更改样式。例如，博客侧边栏中的精选帖子列表：

```tsx
'use client';
 
import Link from 'next/link';
import { useSelectedLayoutSegment } from 'next/navigation';
 
// This *client* component will be imported into a blog layout
export default function BlogNavLink({
  slug,
  children,
}: {
  slug: string;
  children: React.ReactNode;
}) {
  // Navigating to `/blog/hello-world` will return 'hello-world'
  // for the selected layout segment
  const segment = useSelectedLayoutSegment();
  const isActive = slug === segment;
 
  return (
    <Link
      href={`/blog/${slug}`}
      // Change style depending on whether the link is active
      style={{ fontWeight: isActive ? 'bold' : 'normal' }}
    >
      {children}
    </Link>
  );
}
```

```tsx
// Import the Client Component into a parent Layout (Server Component)
import { BlogNavLink } from './blog-nav-link';
import getFeaturedPosts from './get-featured-posts';
 
export default async function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  const featuredPosts = await getFeaturedPosts();
  return (
    <div>
      {featuredPosts.map((post) => (
        <div key={post.id}>
          <BlogNavLink slug={post.slug}>{post.title}</BlogNavLink>
        </div>
      ))}
      <div>{children}</div>
    </div>
  );
}
```

