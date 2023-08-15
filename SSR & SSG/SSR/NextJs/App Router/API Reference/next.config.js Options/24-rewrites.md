# rewrites

重写允许您将传入请求路径映射到其他目标路径。

**重写**充当 URL 代理并屏蔽目标路径，使用户看起来没有更改其在网站上的位置。相反，**重定向**将重新路由到新页面并显示 URL 更改。

To use rewrites you can use the `rewrites` key in `next.config.js`:
要使用重写，您可以在 `next.config.js` 中使用 `rewrites` 键：

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/about',
        destination: '/',
      },
    ];
  },
};
```

重写应用于**客户端路由**， `<Link href="/about">` 将应用上述示例中的重写。

`rewrites` 是一个异步函数，它期望返回一个数组或数组的对象（见下文），其中包含具有 `source` 和 `destination` 属性的对象：

- `source` ： `String` - 是传入请求路径模式。
- `destination` ： `String` 是要路由到的路径。
- `basePath` ： `false` 或 `undefined` - 如果为 false，则匹配时将不包含 basePath，只能用于外部重写。
- `locale` ： `false` 或 `undefined` - 匹配时是否不应包含区域设置。
- `has` 是具有 `type` 、 `key` 和 `value` 属性的 has 对象的数组。
- `missing` 是具有 `type` 、 `key` 和 `value` 属性的missing对象的数组。

当 `rewrites` 函数返回数组时，在检查文件系统（pages和 `/public` 文件）之后和动态路由之前应用重写。当 `rewrites` 函数返回具有特定形状的数组对象时，可以更改此行为并进行更精细的控制，从 Next.js 的 `v10.1` 开始：

```js
module.exports = {
  async rewrites() {
    return {
      beforeFiles: [
        // These rewrites are checked after headers/redirects
        // and before all files including _next/public files which
        // allows overriding page files
        {
          source: '/some-page',
          destination: '/somewhere-else',
          has: [{ type: 'query', key: 'overrideMe' }],
        },
      ],
      afterFiles: [
        // These rewrites are checked after pages/public files
        // are checked but before dynamic routes
        {
          source: '/non-existent',
          destination: '/somewhere-else',
        },
      ],
      fallback: [
        // These rewrites are checked after both pages/public files
        // and dynamic routes are checked
        {
          source: '/:path*',
          destination: `https://my-old-site.com/:path*`,
        },
      ],
    };
  },
};
```

> **Note**: `beforeFiles` 中的重写不会在匹配源后立即检查文件系统/动态路由，它们会继续，直到检查完所有 `beforeFiles` 。

Next.js检查路由的顺序是：

1. 检查/应用标头
2. 检查/应用重定向
3. 检查/应用 `beforeFiles` 重写
4. 检查/服务公共目录中的静态文件、 `_next/static` 文件和非动态页面
5. 检查/应用 `afterFiles` 重写，如果匹配其中一个重写，我们会在每次匹配后检查动态路由/静态文件
6. `fallback` rewrites are checked/applied, these are applied before rendering the 404 page and after dynamic routes/all static assets have been checked. If you use [fallback: true/'blocking'](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths#fallback-true) in `getStaticPaths`, the fallback `rewrites` defined in your `next.config.js` will *not* be run.
   检查/应用`fallback`重写，这些重写在渲染404页面之前和动态路由/所有静态资产检查之后应用。如果在 `getStaticPaths` 中使用 [fallback: true/'blocking'](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths#fallback-true)，则不会运行 `next.config.js` 中定义的回退 `rewrites` 。

## [Rewrite parameters](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#rewrite-parameters)

在重写中使用参数时，当 `destination` 中未使用任何参数时，默认情况下将在query中传递参数。

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/old-about/:path*',
        destination: '/about', // The :path parameter isn't used here so will be automatically passed in the query
      },
    ];
  },
};
```

如果在目标中使用参数，则不会在查询中自动传递任何参数。

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/docs/:path*',
        destination: '/:path*', // The :path parameter is used here so will not be automatically passed in the query
      },
    ];
  },
};
```

如果目标中已使用参数，则仍可以在查询中手动传递参数，方法是在 `destination` 中指定查询

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/:first/:second',
        destination: '/:first?second=:second',
        // Since the :first parameter is used in the destination the :second parameter
        // will not automatically be added in the query although we can manually add it
        // as shown above
      },
    ];
  },
};
```

> 注意：来自自动静态优化的静态优化或来自重写的预渲染参数将在冻结后在客户端上解析，并在查询中提供。

## [Path Matching 路径匹配](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#path-matching)

允许路径匹配，例如 `/blog/:slug` 将匹配 `/blog/hello-world` （无嵌套路径）

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/blog/:slug',
        destination: '/news/:slug', // Matched parameters can be used in the destination
      },
    ];
  },
};
```

### [Wildcard Path Matching 通配符路径匹配](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#wildcard-path-matching)

要匹配通配符路径，您可以在参数后使用 `*` ，例如 `/blog/:slug*` 将匹配 `/blog/a/b/c/d/hello-world` ：

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/blog/:slug*',
        destination: '/news/:slug*', // Matched parameters can be used in the destination
      },
    ];
  },
};
```

### [Regex Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#regex-path-matching)

要匹配正则表达式路径，您可以将正则表达式括在参数后面的括号中，例如 `/blog/:slug(\\d{1,})` 将匹配 `/blog/123` 但不匹配 `/blog/abc` ：

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/old-blog/:post(\\d{1,})',
        destination: '/blog/:post', // Matched parameters can be used in the destination
      },
    ];
  },
};
```

以下字符 `(` 、 `)` 、 `{` 、`3、4`、 `*` 、 `+` 、 `?` 用于正则表达式路径匹配，因此当在 `source` 中用作非特殊值时，必须通过在它们前面添加 `\\` 来转义它们：

```js
module.exports = {
  async rewrites() {
    return [
      {
        // this will match `/english(default)/something` being requested
        source: '/english\\(default\\)/:slug',
        destination: '/en-us/:slug',
      },
    ];
  },
};
```

## [Header, Cookie, and Query Matching](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#header-cookie-and-query-matching)

仅当header、Cookie 或查询值也与 `has` 字段匹配或不匹配 `missing` 字段时才匹配重写。 `source` 和所有 `has` 项必须匹配，并且所有 `missing` 项必须不匹配才能应用重写。

`has` 和 `missing` 项可以具有以下字段：

- `type` ： `String` - 必须是 `header` 、 `cookie` 、 `host` 或 `query` 。
- `key` ： `String` - 要匹配的选定类型中的键。
- `value` ： `String` 或 `undefined` - 要检查的值，如果未定义，则任何值都将匹配。类似正则表达式的字符串可用于捕获值的特定部分，例如，如果值 `first-(?<paramName>.*)` 用于 `first-second` ，则 `second` 将在目标中使用 `:paramName` 。

```js
module.exports = {
  async rewrites() {
    return [
      // if the header `x-rewrite-me` is present,
      // this rewrite will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'header',
            key: 'x-rewrite-me',
          },
        ],
        destination: '/another-page',
      },
      // if the header `x-rewrite-me` is not present,
      // this rewrite will be applied
      {
        source: '/:path*',
        missing: [
          {
            type: 'header',
            key: 'x-rewrite-me',
          },
        ],
        destination: '/another-page',
      },
      // if the source, query, and cookie are matched,
      // this rewrite will be applied
      {
        source: '/specific/:path*',
        has: [
          {
            type: 'query',
            key: 'page',
            // the page value will not be available in the
            // destination since value is provided and doesn't
            // use a named capture group e.g. (?<page>home)
            value: 'home',
          },
          {
            type: 'cookie',
            key: 'authorized',
            value: 'true',
          },
        ],
        destination: '/:path*/home',
      },
      // if the header `x-authorized` is present and
      // contains a matching value, this rewrite will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'header',
            key: 'x-authorized',
            value: '(?<authorized>yes|true)',
          },
        ],
        destination: '/home?authorized=:authorized',
      },
      // if the host is `example.com`,
      // this rewrite will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'host',
            value: 'example.com',
          },
        ],
        destination: '/another-page',
      },
    ];
  },
};
```

## [重写为外部 URL](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#rewriting-to-an-external-url)

重写允许您重写到外部 URL。这对于逐步采用 Next.js 特别有用。下面是将主应用的 `/blog` 路由重定向到外部站点的示例重写

```js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/blog',
        destination: 'https://example.com/blog',
      },
      {
        source: '/blog/:slug',
        destination: 'https://example.com/blog/:slug', // Matched parameters can be used in the destination
      },
    ];
  },
};
```

如果使用 `trailingSlash: true` ，则还需要在 `source` 参数中插入尾部斜杠。如果目标服务器也希望尾部斜杠，则它也应包含在 `destination` 参数中。

```js
module.exports = {
  trailingSlash: true,
  async rewrites() {
    return [
      {
        source: '/blog/',
        destination: 'https://example.com/blog/',
      },
      {
        source: '/blog/:path*/',
        destination: 'https://example.com/blog/:path*/',
      },
    ];
  },
};
```

### [增量采用 Next.js](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#incremental-adoption-of-nextjs)

您还可以让 Next.js 在检查所有 Next.js 路由后回退到代理到现有网站。

这样，在将更多页面迁移到Next.js时，您就不必更改重写配置

```js
module.exports = {
  async rewrites() {
    return {
      fallback: [
        {
          source: '/:path*',
          destination: `https://custom-routes-proxying-endpoint.vercel.app/:path*`,
        },
      ],
    };
  },
};
```

### [拥有 basePath 支持的重写](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#rewrites-with-basepath-support)

当利用 `basePath` 支持进行重写时，每个 `source` 和 `destination` 都自动以 `basePath` 为前缀，除非您在重写中添加 `basePath: false` ：

```js
module.exports = {
  basePath: '/docs',
 
  async rewrites() {
    return [
      {
        source: '/with-basePath', // automatically becomes /docs/with-basePath
        destination: '/another', // automatically becomes /docs/another
      },
      {
        // does not add /docs to /without-basePath since basePath: false is set
        // Note: this can not be used for internal rewrites e.g. `destination: '/another'`
        source: '/without-basePath',
        destination: 'https://example.com',
        basePath: false,
      },
    ];
  },
};
```

### [拥有i18n 支持的重写](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites#rewrites-with-i18n-support)

当利用 `i18n` 支持进行重写时，每个 `source` 和 `destination` 都会自动添加前缀以处理配置的 `locales` ，除非您将 `locale: false` 添加到重写中。如果使用 `locale: false` ，则必须在 `source` 和 `destination` 前面加上区域设置，以便正确匹配。

```js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },
 
  async rewrites() {
    return [
      {
        source: '/with-locale', // automatically handles all locales
        destination: '/another', // automatically passes the locale on
      },
      {
        // does not handle locales automatically since locale: false is set
        source: '/nl/with-locale-manual',
        destination: '/nl/another',
        locale: false,
      },
      {
        // this matches '/' since `en` is the defaultLocale
        source: '/en',
        destination: '/en/another',
        locale: false,
      },
      {
        // it's possible to match all locales even when locale: false is set
        source: '/:locale/api-alias/:path*',
        destination: '/api/:path*',
        locale: false,
      },
      {
        // this gets converted to /(en|fr|de)/(.*) so will not match the top-level
        // `/` or `/fr` routes like /:path* would
        source: '/(.*)',
        destination: '/another',
      },
    ];
  },
};
```

