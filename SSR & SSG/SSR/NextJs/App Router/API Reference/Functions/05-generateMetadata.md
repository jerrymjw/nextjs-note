# Metadata Object and generateMetadata Options

此页面涵盖所有基于 Config(**Config-based Metadata**) 的Metadata选项，包括 `generateMetadata` 和静态Metadata对象。

```tsx
import { Metadata } from 'next';
 
// either Static metadata
export const metadata: Metadata = {
  title: '...',
};
 
// or Dynamic metadata
export async function generateMetadata({ params }) {
  return {
    title: '...',
  };
}
```

> **Good to know:**
>
> - `metadata` 对象和 `generateMetadata` 函数导出仅在**服务器组件中受支持**。
> - 不能从相同路由段同时导出 `metadata` 对象和 `generateMetadata` 函数。

## [The `metadata` object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#the-metadata-object)

要定义静态metadata，请从 `layout.js` 或 `page.js` 文件中导出 `Metadata` 对象。

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: '...',
  description: '...',
};
 
export default function Page() {}
```

## [`generateMetadata` function](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#generatemetadata-function)

动态metadata依赖于动态信息，例如当前路由参数、外部数据或父段中的 `metadata` ，可以通过导出返回 `Metadata` 对象的 `generateMetadata` 函数来设置。

```tsx
import { Metadata, ResolvingMetadata } from 'next';
 
type Props = {
  params: { id: string };
  searchParams: { [key: string]: string | string[] | undefined };
};
 
export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata,
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

### [Parameters](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#parameters)

`generateMetadata` function accepts the following parameters:
`generateMetadata` 函数接受以下参数：

- `props` - 包含当前路由参数的对象：

  - `params` - 一个包含从根段向下到`generateMetadata` 段的动态路由参数对象的对象。例子：
    
    | Route                           | URL         | `params`                  |
    | ------------------------------- | ----------- | ------------------------- |
    | `app/shop/[slug]/page.js`       | `/shop/1`   | `{ slug: '1' }`           |
    | `app/shop/[tag]/[item]/page.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |
    | `app/shop/[...slug]/page.js`    | `/shop/1/2` | `{ slug: ['1', '2'] }`    |
    
  - `searchParams` - 包含当前 URL 的搜索参数的对象。例子：
    
    | URL             | `searchParams`       |
    | --------------- | -------------------- |
    | `/shop?a=1`     | `{ a: '1' }`         |
    | `/shop?a=1&b=2` | `{ a: '1', b: '2' }` |
    | `/shop?a=1&a=2` | `{ a: ['1', '2'] }`  |
  
- `parent` - 来自父路由段的已解析metadata的promise。

### [Returns](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#returns)

`generateMetadata` 应返回包含一个或Metadata field的 `Metadata` 对象。

> **Good to know: 很高兴知道：**
>
> - 如果元数据不依赖于runtime信息，则应使用静态 `metadata` 对象而不是 `generateMetadata` 来定义Metadata。
> - 渲染路由时，Next.js 将自动对 `generateMetadata` 、 `generateStaticParams` 、布局、页面和服务器组件中的相同数据的 `fetch` 请求进行重复数据删除。如果 `fetch` 不可用，可以使用 React `cache` 。
> - `searchParams` 仅在 `page.js` 段中可用。
> - The [`redirect()`](https://nextjs.org/docs/app/api-reference/functions/redirect) and [`notFound()`](https://nextjs.org/docs/app/api-reference/functions/not-found) Next.js methods can also be used inside `generateMetadata`.
>   Next.js中的 [`redirect()`](https://nextjs.org/docs/app/api-reference/functions/redirect) 和 [`notFound()`](https://nextjs.org/docs/app/api-reference/functions/not-found) 方法也可以在 `generateMetadata` 中使用。

## [Metadata Fields](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadata-fields)

### [`title`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#title)

The `title` attribute is used to set the title of the document. It can be defined as a simple [string](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#string) or an optional [template object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#template-object).
`title` 属性用于设置文档的标题。它可以定义为简单 [string](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#string) 或可选的 [template object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#template-object)。

#### [String](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#string)

```js
export const metadata = {
  title: 'Next.js',
};
```

```html
<title>Next.js</title>
```

#### [Template object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#template-object)

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '...',
    default: '...',
    absolute: '...',
  },
};
```

##### [Default](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#default)

`title.default` 可用于为未定义 `title` 的子路由段提供**回退标题**。

```tsx
export const metadata = {
  title: {
    default: 'Acme',
  },
};
```

```tsx
export const metadata = {};
 
// Output: <title>Acme</title>
```

##### [Template](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#template)

`title.template` 可用于向子路由段中定义的 `titles` 添加前缀或后缀。

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
  },
};
```

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'About',
};
 
// Output: <title>About | Acme</title>
```

> **Good to know: 很高兴知道：**
>
> - `title.template` 适用于**子**路由段，而**不是**定义子路由段的分段。这意味着：
>   - `title.default` is required when you add a `title.template`.
>     添加 `title.template` 时 `title.default` 是必须有的。
>   - 在  `layout.js` 中定义的 `title.template` 将不适用于在同一路由段的 `page.js` 中定义的 `title` 。
>   - 在 `page.js` 中定义的 `title.template` 不起作用，因为页面始终是终止区段（它没有任何子路由区段）。
> - 如果路由未定义 `title` 或 `title.default` ，则 `title.template` **无效**。

##### [Absolute](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#absolute)

`title.absolute` 可用于提供**忽略**父区段中 `title.template` 集的标题。

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
  },
};
```

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    absolute: 'About',
  },
};
 
// Output: <title>About</title>
```

> **Good to know: **
>
> - `layout.js`
>   - `title` （字符串）和 `title.default` 定义子段的默认标题（不定义自己的 `title` ）。如果存在，它将从最近的父段增加 `title.template` 。
>   - `title.absolute` 定义子段的默认标题。它会忽略父段中的 `title.template` 。
>   - `title.template` 为子区段定义新的标题模板。
> - `page.js`
>   - 如果页面未定义自己的标题，则将使用最接近父级解析的标题。
>   - `title` （字符串）定义路由标题。如果存在，它将从最近的父段增加 `title.template` 。
>   - `title.absolute` 定义路由标题。它会忽略父段中的 `title.template` 。
>   - `title.template` 在 `page.js` 中不起作用，因为页面始终是路由的终止段。

### [`description`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#description)

```js
export const metadata = {
  description: 'The React Framework for the Web',
};
```

```html
<meta name="description" content="The React Framework for the Web" />
```

### [Basic Fields](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#basic-fields)

```js
export const metadata = {
  generator: 'Next.js',
  applicationName: 'Next.js',
  referrer: 'origin-when-cross-origin',
  keywords: ['Next.js', 'React', 'JavaScript'],
  authors: [{ name: 'Seb' }, { name: 'Josh', url: 'https://nextjs.org' }],
  colorScheme: 'dark',
  creator: 'Jiachi Liu',
  publisher: 'Sebastian Markbåge',
  formatDetection: {
    email: false,
    address: false,
    telephone: false,
  },
};
```

```html
<meta name="application-name" content="Next.js" />
<meta name="author" content="Seb" />
<link rel="author" href="https://nextjs.org" />
<meta name="author" content="Josh" />
<meta name="generator" content="Next.js" />
<meta name="keywords" content="Next.js,React,JavaScript" />
<meta name="referrer" content="origin-when-cross-origin" />
<meta name="color-scheme" content="dark" />
<meta name="creator" content="Jiachi Liu" />
<meta name="publisher" content="Sebastian Markbåge" />
<meta name="format-detection" content="telephone=no, address=no, email=no" />
```

### [`metadataBase`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadatabase)

`metadataBase` 是一个方便的选项，用于为需要完全限定 URL 的 `metadata` 字段设置基本 URL 前缀。

- `metadataBase` 允许在**当前路由段及以下**位置定义的基于 URL 的 `metadata` 字段使用**相对路径**，而不是其他必需的绝对 URL。
- 字段的相对路径将由 `metadataBase` 组成，以形成完全限定的 URL。
- 如果未配置， `metadataBase` 则会被默认值[default value](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#default-value)**自动填充**。

```js
export const metadata = {
  metadataBase: new URL('https://acme.com'),
  alternates: {
    canonical: '/',
    languages: {
      'en-US': '/en-US',
      'de-DE': '/de-DE',
    },
  },
  openGraph: {
    images: '/og-image.png',
  },
};
```

```html
<link rel="canonical" href="https://acme.com" />
<link rel="alternate" hreflang="en-US" href="https://nextjs.org/en-US" />
<link rel="alternate" hreflang="de-DE" href="https://nextjs.org/de-DE" />
<meta property="og:image" content="https://acme.com/og-image.png" />
```

> **Good to know: 很高兴知道：**
>
> - `metadataBase` 通常在根 `app/layout.js` 中设置，以应用于所有路由中基于 URL 的 `metadata` 字段。
> - 所有需要绝对 URL 的基于 URL 的 `metadata` 字段都可以配置为 `metadataBase` 选项。
> - `metadataBase` 可以包含subdomain（例如 `https://app.acme.com` ）或base路径（例如 `https://acme.com/start/from/here` ）
> - 如果 `metadata` 字段提供绝对 URL，则将忽略 `metadataBase` 。
> - 在基于 URL 的 `metadata` 字段中使用相对路径而不配置 `metadataBase` 将导致build error。
> - Next.js将 `metadataBase` （例如 `https://acme.com/` ）和相对字段（例如 `/path` ）之间的重复斜杠规范化为单个斜杠（例如 `https://acme.com/path` ）

#### [Default value](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#default-value)

如果未配置，则 `metadataBase` 具有默认值

- 检测到 `VERCEL_URL` 时： `https://${process.env.VERCEL_URL}` ，否则回退到 `http://localhost:${process.env.PORT || 3000}` 。
- 覆盖默认值时，我们建议使用环境变量来计算 URL。这允许为 local development, staging, and production environment配置 URL。

#### [URL Composition](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#url-composition)

URL 组合优先于开发人员意图，而不是默认目录遍历语义。

- Trailing slashes between `metadataBase` and `metadata` fields are normalized.
  `metadataBase` 到 `metadata` 字段之间的尾随斜杠将规范化。
- An "absolute" path in a `metadata` field (that typically would replace the whole URL path) is treated as a "relative" path (starting from the end of `metadataBase`).
  `metadata` 字段中的“绝对”路径（通常会替换整个 URL 路径）被视为“相对”路径（从 `metadataBase` 的末尾开始）。

For example, given the following `metadataBase`:
例如，给定以下 `metadataBase` ：

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  metadataBase: new URL('https://acme.com/api'),
};
```

Any `metadata` fields that inherit the above `metadataBase` and set their own value will be resolved as follows:
任何继承上述 `metadataBase` 并设置自己的值的 `metadata` 字段将按如下方式解析：

| `metadata` field                     | Resolved URL                         |
| ------------------------------------ | ------------------------------------ |
| `/`                                  | `https://acme.com/api`               |
| `./`                                 | `https://acme.com/api`               |
| `payments`                           | `https://acme.com/api/payments`      |
| `/payments`                          | `https://acme.com/api/payments`      |
| `./payments`                         | `https://acme.com/api/payments`      |
| `../payments`                        | `https://acme.com/payments`          |
| `https://beta.acme.com/api/payments` | `https://beta.acme.com/api/payments` |

### [`openGraph`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#opengraph)

```js
export const metadata = {
  openGraph: {
    title: 'Next.js',
    description: 'The React Framework for the Web',
    url: 'https://nextjs.org',
    siteName: 'Next.js',
    images: [
      {
        url: 'https://nextjs.org/og.png',
        width: 800,
        height: 600,
      },
      {
        url: 'https://nextjs.org/og-alt.png',
        width: 1800,
        height: 1600,
        alt: 'My custom alt',
      },
    ],
    locale: 'en_US',
    type: 'website',
  },
};
```

```html
<meta property="og:title" content="Next.js" />
<meta property="og:description" content="The React Framework for the Web" />
<meta property="og:url" content="https://nextjs.org/" />
<meta property="og:site_name" content="Next.js" />
<meta property="og:locale" content="en_US" />
<meta property="og:image:url" content="https://nextjs.org/og.png" />
<meta property="og:image:width" content="800" />
<meta property="og:image:height" content="600" />
<meta property="og:image:url" content="https://nextjs.org/og-alt.png" />
<meta property="og:image:width" content="1800" />
<meta property="og:image:height" content="1600" />
<meta property="og:image:alt" content="My custom alt" />
<meta property="og:type" content="website" />
```

```js
export const metadata = {
  openGraph: {
    title: 'Next.js',
    description: 'The React Framework for the Web',
    type: 'article',
    publishedTime: '2023-01-01T00:00:00.000Z',
    authors: ['Seb', 'Josh'],
  },
};
```

```html
<meta property="og:title" content="Next.js" />
<meta property="og:description" content="The React Framework for the Web" />
<meta property="og:type" content="article" />
<meta property="article:published_time" content="2023-01-01T00:00:00.000Z" />
<meta property="article:author" content="Seb" />
<meta property="article:author" content="Josh" />
```

> **Good to know: 很高兴知道：**
>
> - It may be more convenient to use the [file-based Metadata API](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#image-files-jpg-png-gif) for Open Graph images. Rather than having to sync the config export with actual files, the file-based API will automatically generate the correct metadata for you.
>   对开放图形图像使用基于文件的元数据 API 可能更方便。无需将配置导出与实际文件同步，基于文件的 API 将自动生成正确的元数据。

### [`robots`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#robots)

```js
export const metadata = {
  robots: {
    index: false,
    follow: true,
    nocache: true,
    googleBot: {
      index: true,
      follow: false,
      noimageindex: true,
      'max-video-preview': -1,
      'max-image-preview': 'large',
      'max-snippet': -1,
    },
  },
};
```

```html
<meta name="robots" content="noindex, follow, nocache" />
<meta
  name="googlebot"
  content="index, nofollow, noimageindex, max-video-preview:-1, max-image-preview:large, max-snippet:-1"
/>
```

### [`icons`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#icons)

注意：我们建议尽可能对图标使用基于文件的元数据 API。无需将配置导出与实际文件同步，基于文件的 API 将自动生成正确的元数据。

```js
export const metadata = {
  icons: {
    icon: '/icon.png',
    shortcut: '/shortcut-icon.png',
    apple: '/apple-icon.png',
    other: {
      rel: 'apple-touch-icon-precomposed',
      url: '/apple-touch-icon-precomposed.png',
    },
  },
};
```

```html
<link rel="shortcut icon" href="/shortcut-icon.png" />
<link rel="icon" href="/icon.png" />
<link rel="apple-touch-icon" href="/apple-icon.png" />
<link
  rel="apple-touch-icon-precomposed"
  href="/apple-touch-icon-precomposed.png"
/>
```

```js
export const metadata = {
  icons: {
    icon: [{ url: '/icon.png' }, new URL('/icon.png', 'https://example.com')],
    shortcut: ['/shortcut-icon.png'],
    apple: [
      { url: '/apple-icon.png' },
      { url: '/apple-icon-x3.png', sizes: '180x180', type: 'image/png' },
    ],
    other: [
      {
        rel: 'apple-touch-icon-precomposed',
        url: '/apple-touch-icon-precomposed.png',
      },
    ],
  },
};
```

```html
<link rel="shortcut icon" href="/shortcut-icon.png" />
<link rel="icon" href="/icon.png" />
<link rel="apple-touch-icon" href="/apple-icon.png" />
<link
  rel="apple-touch-icon-precomposed"
  href="/apple-touch-icon-precomposed.png"
/>
<link rel="icon" href="https://example.com/icon.png" />
<link
  rel="apple-touch-icon"
  href="/apple-icon-x3.png"
  sizes="180x180"
  type="image/png"
/>
```

> 注意： `msapplication-*` 元标记在Microsoft Edge的Chromium版本中不再受支持，因此不再需要。

### [`themeColor`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#themecolor)

**Simple theme color 简单的主题颜色**

```js
export const metadata = {
  themeColor: 'black',
};
```

```html
<meta name="theme-color" content="black" />
```

**With media attribute** 

```js
export const metadata = {
  themeColor: [
    { media: '(prefers-color-scheme: light)', color: 'cyan' },
    { media: '(prefers-color-scheme: dark)', color: 'black' },
  ],
};
```

```html
<meta name="theme-color" media="(prefers-color-scheme: light)" content="cyan" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="black" />
```

### [`manifest`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#manifest)

A web application manifest, as defined in the [Web Application Manifest specification](https://developer.mozilla.org/en-US/docs/Web/Manifest).
Web 应用程序清单，如 Web 应用程序清单规范中所定义。

```js
export const metadata = {
  manifest: 'https://nextjs.org/manifest.json',
};
```

```html
<link rel="manifest" href="https://nextjs.org/manifest.json" />
```

### [`twitter`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#twitter)

Learn more about the [Twitter Card markup reference](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/markup).
详细了解 Twitter 卡片标记参考。

```js
export const metadata = {
  twitter: {
    card: 'summary_large_image',
    title: 'Next.js',
    description: 'The React Framework for the Web',
    siteId: '1467726470533754880',
    creator: '@nextjs',
    creatorId: '1467726470533754880',
    images: ['https://nextjs.org/og.png'],
  },
};
```

```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site:id" content="1467726470533754880" />
<meta name="twitter:creator" content="@nextjs" />
<meta name="twitter:creator:id" content="1467726470533754880" />
<meta name="twitter:title" content="Next.js" />
<meta name="twitter:description" content="The React Framework for the Web" />
<meta name="twitter:image" content="https://nextjs.org/og.png" />
```

```js
export const metadata = {
  twitter: {
    card: 'app',
    title: 'Next.js',
    description: 'The React Framework for the Web',
    siteId: '1467726470533754880',
    creator: '@nextjs',
    creatorId: '1467726470533754880',
    images: {
      url: 'https://nextjs.org/og.png',
      alt: 'Next.js Logo',
    },
    app: {
      name: 'twitter_app',
      id: {
        iphone: 'twitter_app://iphone',
        ipad: 'twitter_app://ipad',
        googleplay: 'twitter_app://googleplay',
      },
      url: {
        iphone: 'https://iphone_url',
        ipad: 'https://ipad_url',
      },
    },
  },
};
```

```html
<meta name="twitter:site:id" content="1467726470533754880" />
<meta name="twitter:creator" content="@nextjs" />
<meta name="twitter:creator:id" content="1467726470533754880" />
<meta name="twitter:title" content="Next.js" />
<meta name="twitter:description" content="The React Framework for the Web" />
<meta name="twitter:card" content="app" />
<meta name="twitter:image" content="https://nextjs.org/og.png" />
<meta name="twitter:image:alt" content="Next.js Logo" />
<meta name="twitter:app:name:iphone" content="twitter_app" />
<meta name="twitter:app:id:iphone" content="twitter_app://iphone" />
<meta name="twitter:app:id:ipad" content="twitter_app://ipad" />
<meta name="twitter:app:id:googleplay" content="twitter_app://googleplay" />
<meta name="twitter:app:url:iphone" content="https://iphone_url" />
<meta name="twitter:app:url:ipad" content="https://ipad_url" />
<meta name="twitter:app:name:ipad" content="twitter_app" />
<meta name="twitter:app:name:googleplay" content="twitter_app" />
```

### [`viewport`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#viewport)

> **Note**: The `viewport` meta tag is automatically set with the following default values. Usually, manual configuration is unnecessary as the default is sufficient. However, the information is provided for completeness.
> 注意： `viewport` 元标记会自动设置为以下默认值。通常，手动配置是不必要的，因为默认值就足够了。但是，提供该信息是为了完整起见。

```js
export const metadata = {
  viewport: {
    width: 'device-width',
    initialScale: 1,
    maximumScale: 1,
  },
};
```

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1, maximum-scale=1"
/>
```

### [`verification`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#verification)

```js
export const metadata = {
  verification: {
    google: 'google',
    yandex: 'yandex',
    yahoo: 'yahoo',
    other: {
      me: ['my-email', 'my-link'],
    },
  },
};
```

```html
<meta name="google-site-verification" content="google" />
<meta name="y_key" content="yahoo" />
<meta name="yandex-verification" content="yandex" />
<meta name="me" content="my-email" />
<meta name="me" content="my-link" />
```

### [`appleWebApp`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#applewebapp)

```js
export const metadata = {
  itunes: {
    appId: 'myAppStoreID',
    appArgument: 'myAppArgument',
  },
  appleWebApp: {
    title: 'Apple Web App',
    statusBarStyle: 'black-translucent',
    startupImage: [
      '/assets/startup/apple-touch-startup-image-768x1004.png',
      {
        url: '/assets/startup/apple-touch-startup-image-1536x2008.png',
        media: '(device-width: 768px) and (device-height: 1024px)',
      },
    ],
  },
};
```

```html
<meta
  name="apple-itunes-app"
  content="app-id=myAppStoreID, app-argument=myAppArgument"
/>
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-title" content="Apple Web App" />
<link
  href="/assets/startup/apple-touch-startup-image-768x1004.png"
  rel="apple-touch-startup-image"
/>
<link
  href="/assets/startup/apple-touch-startup-image-1536x2008.png"
  media="(device-width: 768px) and (device-height: 1024px)"
  rel="apple-touch-startup-image"
/>
<meta
  name="apple-mobile-web-app-status-bar-style"
  content="black-translucent"
/>
```

### [`alternates`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#alternates)

```js
export const metadata = {
  alternates: {
    canonical: 'https://nextjs.org',
    languages: {
      'en-US': 'https://nextjs.org/en-US',
      'de-DE': 'https://nextjs.org/de-DE',
    },
    media: {
      'only screen and (max-width: 600px)': 'https://nextjs.org/mobile',
    },
    types: {
      'application/rss+xml': 'https://nextjs.org/rss',
    },
  },
};
```

```html
<link rel="canonical" href="https://nextjs.org" />
<link rel="alternate" hreflang="en-US" href="https://nextjs.org/en-US" />
<link rel="alternate" hreflang="de-DE" href="https://nextjs.org/de-DE" />
<link
  rel="alternate"
  media="only screen and (max-width: 600px)"
  href="https://nextjs.org/mobile"
/>
<link
  rel="alternate"
  type="application/rss+xml"
  href="https://nextjs.org/rss"
/>
```

### [`appLinks`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#applinks)

```js
export const metadata = {
  appLinks: {
    ios: {
      url: 'https://nextjs.org/ios',
      app_store_id: 'app_store_id',
    },
    android: {
      package: 'com.example.android/package',
      app_name: 'app_name_android',
    },
    web: {
      url: 'https://nextjs.org/web',
      should_fallback: true,
    },
  },
};
```

```html
<meta property="al:ios:url" content="https://nextjs.org/ios" />
<meta property="al:ios:app_store_id" content="app_store_id" />
<meta property="al:android:package" content="com.example.android/package" />
<meta property="al:android:app_name" content="app_name_android" />
<meta property="al:web:url" content="https://nextjs.org/web" />
<meta property="al:web:should_fallback" content="true" />
```

### [`archives`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#archives)

Describes a collection of records, documents, or other materials of historical interest ([source](https://www.w3.org/TR/2011/WD-html5-20110113/links.html#rel-archives)).
描述记录、文档或其他具有历史意义的材料的集合（来源）。

```js
export const metadata = {
  archives: ['https://nextjs.org/13'],
};
```

```html
<link rel="archives" href="https://nextjs.org/13" />
```

### [`assets`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#assets)

```js
export const metadata = {
  assets: ['https://nextjs.org/assets'],
};
```

```html
<link rel="assets" href="https://nextjs.org/assets" />
```

### [`bookmarks`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#bookmarks)

```js
export const metadata = {
  bookmarks: ['https://nextjs.org/13'],
};
```

```html
<link rel="bookmarks" href="https://nextjs.org/13" />
```

### [`category`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#category)

```js
export const metadata = {
  category: 'technology',
};
```

```html
<meta name="category" content="technology" />
```

### [`other`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#other)

应使用内置支持涵盖所有元数据选项。但是，可能有特定于您网站的自定义元数据标记，或者刚刚发布的全新元数据标记。您可以使用 `other` 选项渲染任何自定义元数据标记。

```js
export const metadata = {
  other: {
    custom: 'meta',
  },
};
```

```html
<meta name="custom" content="meta" />
```

## [不支持的Metadata](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#unsupported-metadata)

The following metadata types do not currently have built-in support. However, they can still be rendered in the layout or page itself.
以下元数据类型当前没有内置支持。但是，它们仍然可以在布局或页面本身中渲染。

| Metadata                      | Recommendation                                               |
| ----------------------------- | ------------------------------------------------------------ |
| `<meta http-equiv="...">`     | Use appropriate HTTP Headers via [`redirect()`](https://nextjs.org/docs/app/api-reference/functions/redirect), [Middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware#nextresponse), [Security Headers](https://nextjs.org/docs/app/api-reference/next-config-js/headers) 通过 `redirect()` 、中间件、安全标头使用适当的 HTTP 标头 |
| `<base>`                      | 在布局或页面本身中呈现代码。                                 |
| `<noscript>`                  | 在布局或页面本身中呈现代码。                                 |
| `<style>`                     | 在 Next.js 中了解有关样式设置的详细信息。                    |
| `<script>`                    | 详细了解如何使用脚本。                                       |
| `<link rel="stylesheet" />`   | `import` stylesheets直接在布局或页面本身。                   |
| `<link rel="preload />`       | Use [ReactDOM preload method](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-relpreload) |
| `<link rel="preconnect" />`   | Use [ReactDOM preconnect method](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-relpreconnect) |
| `<link rel="dns-prefetch" />` | Use [ReactDOM prefetchDNS method](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-reldns-prefetch) |

### [Resource hints](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#resource-hints)

`<link>` 元素具有多个 `rel` 关键字，可用于向浏览器提示可能需要外部资源。浏览器使用此信息根据关键字应用预加载优化。

虽然元数据 API 不直接支持这些提示，但可以使用新的 `ReactDOM` 方法将它们安全地插入到文档的 `<head>` 中。

```tsx
'use client';
 
import ReactDOM from 'react-dom';
 
export function PreloadResources() {
  ReactDOM.preload('...', { as: '...' });
  ReactDOM.preconnect('...', { crossOrigin: '...' });
  ReactDOM.prefetchDNS('...');
 
  return null;
}
```

##### [`<link rel="preload">`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-relpreload)

在页面渲染（浏览器）生命周期的早期开始加载资源。

```tsx
ReactDOM.preload(href: string, options: { as: string })
```

```html
<link rel="preload" href="..." as="..." />
```

##### [`<link rel="preconnect">`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-relpreconnect)

抢先启动与源的连接。

```js
ReactDOM.preconnect(href: string, options?: { crossOrigin?: string })
```

```html
<link rel="preconnect" href="..." crossorigin />
```

##### [`<link rel="dns-prefetch">`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#link-reldns-prefetch)

尝试在请求资源之前解析域名。

```js
ReactDOM.prefetchDNS(href: string)
```

```html
<link rel="dns-prefetch" href="..." />
```

> **Good to know: **
>
> - 这些方法目前仅在客户端组件中受支持。
>   - 注意：客户端组件在初始页面加载时仍处于服务器端渲染状态。
> - Next.js 内置功能如`next/font` 、 `next/image` 和 `next/script` 等会自动处理相关的资源提示。
> - React 18.3 尚未包含 `ReactDOM.preload` 、 `ReactDOM.preconnect` 和 `ReactDOM.preconnectDNS` 的类型定义。可以使用 `// @ts-ignore` 作为临时解决方案以避免类型错误。

## [Types](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#types)

可以使用 `Metadata` 类型向元数据添加类型安全性。如果在 IDE 中使用内置的 TypeScript 插件 [built-in TypeScript plugin](https://nextjs.org/docs/app/building-your-application/configuring/typescript) ，则无需手动添加类型，但如果需要，仍可以显式添加类型。

### [`metadata` object](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadata-object)

```tsx
import type { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Next.js',
};
```

### [`generateMetadata` function](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#generatemetadata-function-1)

#### [Regular function](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#regular-function)

```tsx
import type { Metadata } from 'next';
 
export function generateMetadata(): Metadata {
  return {
    title: 'Next.js',
  };
}
```

#### [Async function](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#async-function)

```tsx
import type { Metadata } from 'next';
 
export async function generateMetadata(): Promise<Metadata> {
  return {
    title: 'Next.js',
  };
}
```

#### [With segment props](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#with-segment-props)

```tsx
import type { Metadata } from 'next';
 
type Props = {
  params: { id: string };
  searchParams: { [key: string]: string | string[] | undefined };
};
 
export function generateMetadata({ params, searchParams }: Props): Metadata {
  return {
    title: 'Next.js',
  };
}
 
export default function Page({ params, searchParams }: Props) {}
```

#### [With parent metadata](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#with-parent-metadata)

```tsx
import type { Metadata, ResolvingMetadata } from 'next';
 
export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata,
): Promise<Metadata> {
  return {
    title: 'Next.js',
  };
}
```

#### [JavaScript Projects](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#javascript-projects)

对于 JavaScript 项目，可以使用 JSDoc 添加类型安全性。

```tsx
/** @type {import("next").Metadata} */
export const metadata = {
  title: 'Next.js',
};
```

