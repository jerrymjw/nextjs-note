# redirects

重定向允许您将传入请求路径重定向到其他目标路径。

要使用重定向，您可以在 `next.config.js` 中使用 `redirects` 键

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
    ];
  },
};
```

`redirects` 是一个异步函数，它期望返回一个包含具有 `source` 、 `destination` 和 `permanent` 属性的数组：

- `source` 是传入请求路径模式。
- `destination` 是要路由到达的路径。
- `permanent` `true` 或 `false` - 如果 `true` 将使用指示客户端/搜索引擎永久缓存重定向的 308 状态代码，如果 `false` 将使用临时且未缓存的 307 状态代码。

> **为什么 Next.js 使用 307 和 308**？传统上，302 用于临时重定向，301 用于永久重定向，但许多浏览器将重定向的请求方法更改为 `GET` ，而不管原始方法如何。例如，如果浏览器向 `POST /v1/users` 发出请求，该请求返回位置为 `/v2/users` 的状态代码 `302` ，则后续请求可能是 `GET /v2/users` ，而不是预期的 `POST /v2/users` 。接下来.js使用 307 临时重定向和 308 永久重定向状态代码来显式保留所使用的请求方法。

- `basePath` ： `false` 或 `undefined` - 如果为 false，则匹配时将不包括 `basePath` ，只能用于外部重定向。
- `locale` ： `false` 或 `undefined` - 匹配时是否不应包含区域设置。
- `has` 是具有 `type` 、 `key` 和 `value` 属性的 has 对象的数组。
- `missing` is an array of [missing objects](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#header-cookie-and-query-matching) with the `type`, `key` and `value` properties.
  `missing` 是具有 `type` 、 `key` 和 `value` 属性的 [missing objects](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#header-cookie-and-query-matching) 的数组。

在包含pages和 `/public` 文件的文件系统之前检查重定向。

重定向不会应用于客户端路由 （ `Link` ， `router.push` ），除非中间件存在并且与路径匹配。

应用重定向时，请求中提供的任何查询值都将传递到重定向目标。例如，请参阅以下重定向配置：

```json
{
  source: '/old-blog/:path*',
  destination: '/blog/:path*',
  permanent: false
}
```

当请求 `/old-blog/post-1?hello=world` 时，客户端将被重定向到 `/blog/post-1?hello=world` 。

## [Path Matching 路径匹配](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#path-matching)

Path matches are allowed, for example `/old-blog/:slug` will match `/old-blog/hello-world` (no nested paths):
允许路径匹配，例如 `/old-blog/:slug` 将匹配 `/old-blog/hello-world` （无嵌套路径）：

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/old-blog/:slug',
        destination: '/news/:slug', // Matched parameters can be used in the destination
        permanent: true,
      },
    ];
  },
};
```

### [Wildcard Path Matching 通配符路径匹配](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#wildcard-path-matching)

要匹配通配符路径，您可以在参数后使用 `*` ，例如 `/blog/:slug*` 将匹配 `/blog/a/b/c/d/hello-world` ：

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/blog/:slug*',
        destination: '/news/:slug*', // Matched parameters can be used in the destination
        permanent: true,
      },
    ];
  },
};
```

### [Regex Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#regex-path-matching)

要匹配正则表达式路径，您可以将正则表达式括在参数后面的括号中，例如 `/post/:slug(\\d{1,})` 将匹配 `/post/123` 但不匹配 `/post/abc` ：

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/post/:slug(\\d{1,})',
        destination: '/news/:slug', // Matched parameters can be used in the destination
        permanent: false,
      },
    ];
  },
};
```

以下字符 `(` 、 `)` 、 `{` 、`3、4`、 `*` 、 `+` 、 `?` 用于正则表达式路径匹配，因此当在 `source` 中用作非特殊值时，必须通过在它们前面添加 `\\` 来转义它们：

```js
module.exports = {
  async redirects() {
    return [
      {
        // this will match `/english(default)/something` being requested
        source: '/english\\(default\\)/:slug',
        destination: '/en-us/:slug',
        permanent: false,
      },
    ];
  },
};
```

## [Header, Cookie, and Query Matching](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#header-cookie-and-query-matching)

仅当header、Cookie 或查询值也与 `has` 字段匹配或不匹配时，才匹配重定向，可以使用 `missing` 字段。 `source` 项和所有 `has` 项必须匹配，并且所有 `missing` 项必须不匹配才能应用重定向。

`has` 和 `missing` 项可以具有以下字段：

- `type` ： `String` - 必须是 `header` 、 `cookie` 、 `host` 或 `query` 。
- `key` ： `String` - 要匹配的选定类型中的键。
- `value` ： `String` 或 `undefined` - 要检查的值，如果未定义，则任何值都将匹配。类似正则表达式的字符串可用于捕获值的特定部分，例如，如果值 `first-(?<paramName>.*)` 用于 `first-second` ，则 `second` 将在目标中使用 `:paramName` 。

```js
module.exports = {
  async redirects() {
    return [
      // if the header `x-redirect-me` is present,
      // this redirect will be applied
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'header',
            key: 'x-redirect-me',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // if the header `x-dont-redirect` is present,
      // this redirect will NOT be applied
      {
        source: '/:path((?!another-page$).*)',
        missing: [
          {
            type: 'header',
            key: 'x-do-not-redirect',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // if the source, query, and cookie are matched,
      // this redirect will be applied
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
        permanent: false,
        destination: '/another/:path*',
      },
      // if the header `x-authorized` is present and
      // contains a matching value, this redirect will be applied
      {
        source: '/',
        has: [
          {
            type: 'header',
            key: 'x-authorized',
            value: '(?<authorized>yes|true)',
          },
        ],
        permanent: false,
        destination: '/home?authorized=:authorized',
      },
      // if the host is `example.com`,
      // this redirect will be applied
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'host',
            value: 'example.com',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
    ];
  },
};
```

### [使用基本路径支持的重定向](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#redirects-with-basepath-support)

当利用 `basePath` 支持进行重定向时，每个 `source` 和 `destination` 都会自动以 `basePath` 为前缀，除非您在重定向中添加 `basePath: false` ：

```js
module.exports = {
  basePath: '/docs',
 
  async redirects() {
    return [
      {
        source: '/with-basePath', // automatically becomes /docs/with-basePath
        destination: '/another', // automatically becomes /docs/another
        permanent: false,
      },
      {
        // does not add /docs since basePath: false is set
        source: '/without-basePath',
        destination: 'https://example.com',
        basePath: false,
        permanent: false,
      },
    ];
  },
};
```

### [Redirects with i18n support 使用 i18n 支持的重定向](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#redirects-with-i18n-support)

当利用 `i18n` 支持进行重定向时，每个 `source` 和 `destination` 都会自动添加前缀以处理配置的 `locales` ，除非您将 `locale: false` 添加到重定向。如果使用 `locale: false` ，则必须在 `source` 和 `destination` 前面加上区域设置，以便正确匹配。

```js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },
 
  async redirects() {
    return [
      {
        source: '/with-locale', // automatically handles all locales
        destination: '/another', // automatically passes the locale on
        permanent: false,
      },
      {
        // does not handle locales automatically since locale: false is set
        source: '/nl/with-locale-manual',
        destination: '/nl/another',
        locale: false,
        permanent: false,
      },
      {
        // this matches '/' since `en` is the defaultLocale
        source: '/en',
        destination: '/en/another',
        locale: false,
        permanent: false,
      },
      // it's possible to match all locales even when locale: false is set
      {
        source: '/:locale/page',
        destination: '/en/newpage',
        permanent: false,
        locale: false,
      }
      {
        // this gets converted to /(en|fr|de)/(.*) so will not match the top-level
        // `/` or `/fr` routes like /:path* would
        source: '/(.*)',
        destination: '/another',
        permanent: false,
      },
    ]
  },
}
```

在极少数情况下，您可能需要为较旧的 HTTP 客户端分配自定义状态代码才能正确重定向。在这些情况下，可以使用 `statusCode` 属性而不是 `permanent` 属性，但不能同时使用两者。为了确保 IE11 兼容性，会自动为 308 状态代码添加 `Refresh` header。

## [Other Redirects 其他重定向](https://nextjs.org/docs/app/api-reference/next-config-js/redirects#other-redirects)

- 在 API 路由中，可以使用 `res.redirect()` 。
- Inside [`getStaticProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props) and [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props), you can redirect specific pages at request-time.
  在 [`getStaticProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props) 和 [`getServerSideProps`](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props) 中，您可以在请求时重定向特定页面。

