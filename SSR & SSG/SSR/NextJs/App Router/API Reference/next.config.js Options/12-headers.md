# headers

Headers允许您在给定路径上对传入请求的响应上设置自定义 HTTP headers。

要设置自定义 HTTP headers，您可以在 `next.config.js` 中使用 `headers` key

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/about',
        headers: [
          {
            key: 'x-custom-header',
            value: 'my custom header value',
          },
          {
            key: 'x-another-custom-header',
            value: 'my other custom header value',
          },
        ],
      },
    ];
  },
};
```

`headers` 是一个异步函数，它期望返回一个包含具有 `source` 和 `headers` 属性的对象的数组：

- `source` 是传入请求路径模式。
- `headers` 是响应header对象的数组，具有 `key` 和 `value` 属性。
- `basePath` ： `false` 或 `undefined` - 如果为 false，则匹配时将不包含 basePath，只能用于外部重写。
- `locale` ： `false` 或 `undefined` - 匹配时是否不应包含区域设置。
- `has` 是具有 `type` 、 `key` 和 `value` 属性的 has 对象的数组。
- `missing` 是具有 `type` 、 `key` 和 `value` 属性的缺失对象的数组。

在包含页面和 `/public` 文件的文件系统之前检查header。

## [Header Overriding Behavior 标头覆盖行为](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-overriding-behavior)

If two headers match the same path and set the same header key, the last header key will override the first. Using the below headers, the path `/hello` will result in the header `x-hello` being `world` due to the last header value set being `world`.
如果两个header匹配同一路径并设置相同的标头键，则最后一个header key将覆盖第一个header key。使用以下header，路径 `/hello` 将导致header `x-hello` 为 `world` ，因为最后一个header值设置为 `world` 。

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'x-hello',
            value: 'there',
          },
        ],
      },
      {
        source: '/hello',
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
    ];
  },
};
```

## [Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#path-matching)

允许路径匹配，例如 `/blog/:slug` 将匹配 `/blog/hello-world` （无嵌套路径）：

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/blog/:slug',
        headers: [
          {
            key: 'x-slug',
            value: ':slug', // Matched parameters can be used in the value
          },
          {
            key: 'x-slug-:slug', // Matched parameters can be used in the key
            value: 'my other custom header value',
          },
        ],
      },
    ];
  },
};
```

### [Wildcard Path Matching 通配符路径匹配](https://nextjs.org/docs/app/api-reference/next-config-js/headers#wildcard-path-matching)

To match a wildcard path you can use `*` after a parameter, for example `/blog/:slug*` will match `/blog/a/b/c/d/hello-world`:
要匹配通配符路径，您可以在参数后使用 `*` ，例如 `/blog/:slug*` 将匹配 `/blog/a/b/c/d/hello-world` ：

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/blog/:slug*',
        headers: [
          {
            key: 'x-slug',
            value: ':slug*', // Matched parameters can be used in the value
          },
          {
            key: 'x-slug-:slug*', // Matched parameters can be used in the key
            value: 'my other custom header value',
          },
        ],
      },
    ];
  },
};
```

### [Regex Path Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#regex-path-matching)

To match a regex path you can wrap the regex in parenthesis after a parameter, for example `/blog/:slug(\\d{1,})` will match `/blog/123` but not `/blog/abc`:
要匹配正则表达式路径，您可以将正则表达式括在参数后面的括号中，例如 `/blog/:slug(\\d{1,})` 将匹配 `/blog/123` 但不匹配 `/blog/abc` ：

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/blog/:post(\\d{1,})',
        headers: [
          {
            key: 'x-post',
            value: ':post',
          },
        ],
      },
    ];
  },
};
```

The following characters `(`, `)`, `{`, `}`, `:`, `*`, `+`, `?` are used for regex path matching, so when used in the `source` as non-special values they must be escaped by adding `\\` before them:
以下字符 `(` 、 `)` 、 `{` 、`3、4`、 `*` 、 `+` 、 `?` 用于正则表达式路径匹配，因此当在 `source` 中用作非特殊值时，必须通过在它们前面添加 `\\` 来转义它们： ``

```js
module.exports = {
  async headers() {
    return [
      {
        // this will match `/english(default)/something` being requested
        source: '/english\\(default\\)/:slug',
        headers: [
          {
            key: 'x-header',
            value: 'value',
          },
        ],
      },
    ];
  },
};
```

## [Header, Cookie, and Query Matching](https://nextjs.org/docs/app/api-reference/next-config-js/headers#header-cookie-and-query-matching)

仅当header、Cookie 或query值也与 `has` 字段匹配或不匹配时 `missing` 字段时启用。 `source` 和所有 `has` 项必须匹配，并且所有 `missing` 项不得匹配才能应用header。

`has` 和 `missing` 项可以具有以下字段：

- `type` ： `String` - 必须是 `header` 、 `cookie` 、 `host` 或 `query` 。
- `key` ： `String` - 要匹配的选定类型中的键。
- `value` ： `String` 或 `undefined` - 要检查的值，如果未定义，则任何值都将匹配。类似正则表达式的字符串可用于捕获值的特定部分，例如，如果值 `first-(?<paramName>.*)` 用于 `first-second` ，则 `second` 将在目标中使用 `:paramName` 。

```js
module.exports = {
  async headers() {
    return [
      // if the header `x-add-header` is present,
      // the `x-another-header` header will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'header',
            key: 'x-add-header',
          },
        ],
        headers: [
          {
            key: 'x-another-header',
            value: 'hello',
          },
        ],
      },
      // if the header `x-no-header` is not present,
      // the `x-another-header` header will be applied
      {
        source: '/:path*',
        missing: [
          {
            type: 'header',
            key: 'x-no-header',
          },
        ],
        headers: [
          {
            key: 'x-another-header',
            value: 'hello',
          },
        ],
      },
      // if the source, query, and cookie are matched,
      // the `x-authorized` header will be applied
      {
        source: '/specific/:path*',
        has: [
          {
            type: 'query',
            key: 'page',
            // the page value will not be available in the
            // header key/values since value is provided and
            // doesn't use a named capture group e.g. (?<page>home)
            value: 'home',
          },
          {
            type: 'cookie',
            key: 'authorized',
            value: 'true',
          },
        ],
        headers: [
          {
            key: 'x-authorized',
            value: ':authorized',
          },
        ],
      },
      // if the header `x-authorized` is present and
      // contains a matching value, the `x-another-header` will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'header',
            key: 'x-authorized',
            value: '(?<authorized>yes|true)',
          },
        ],
        headers: [
          {
            key: 'x-another-header',
            value: ':authorized',
          },
        ],
      },
      // if the host is `example.com`,
      // this header will be applied
      {
        source: '/:path*',
        has: [
          {
            type: 'host',
            value: 'example.com',
          },
        ],
        headers: [
          {
            key: 'x-another-header',
            value: ':authorized',
          },
        ],
      },
    ];
  },
};
```

## [拥有basePath支持的headers](https://nextjs.org/docs/app/api-reference/next-config-js/headers#headers-with-basepath-support)

当利用拥有`basePath`支持的headers时，每个 `source` 会自动以 `basePath` 为前缀，除非您在header中添加 `basePath: false` ：

```js
module.exports = {
  basePath: '/docs',
 
  async headers() {
    return [
      {
        source: '/with-basePath', // becomes /docs/with-basePath
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
      {
        source: '/without-basePath', // is not modified since basePath: false is set
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
        basePath: false,
      },
    ];
  },
};
```

## [拥有i18n支持的headers](https://nextjs.org/docs/app/api-reference/next-config-js/headers#headers-with-i18n-support)

When leveraging [`i18n` support](https://nextjs.org/docs/pages/building-your-application/routing/internationalization) with headers each `source` is automatically prefixed to handle the configured `locales` unless you add `locale: false` to the header. If `locale: false` is used you must prefix the `source` with a locale for it to be matched correctly.
当利用 拥有`i18n`支持 [`i18n` support](https://nextjs.org/docs/pages/building-your-application/routing/internationalization) 的headers时，每个 `source` 会自动添加前缀以处理配置的 `locales` ，除非您向标头添加 `locale: false` 。如果使用 `locale: false` ，则必须在 `source` 前面加上区域设置，以便正确匹配。

```js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },
 
  async headers() {
    return [
      {
        source: '/with-locale', // automatically handles all locales
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
      {
        // does not handle locales automatically since locale: false is set
        source: '/nl/with-locale-manual',
        locale: false,
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
      {
        // this matches '/' since `en` is the defaultLocale
        source: '/en',
        locale: false,
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
      {
        // this gets converted to /(en|fr|de)/(.*) so will not match the top-level
        // `/` or `/fr` routes like /:path* would
        source: '/(.*)',
        headers: [
          {
            key: 'x-hello',
            value: 'world',
          },
        ],
      },
    ];
  },
};
```

## [Cache-Control](https://nextjs.org/docs/app/api-reference/next-config-js/headers#cache-control)

您可以使用 `res.setHeader` 方法在 Next.js API 路由中设置 `Cache-Control` header：

```js
export default function handler(req, res) {
  res.setHeader('Cache-Control', 's-maxage=86400');
  res.status(200).json({ name: 'John Doe' });
}
```

您不能在 `next.config.js` 个文件中设置 `Cache-Control` 个标头，因为这些标头将在生产中被覆盖，以确保有效缓存 API 路由和静态资产。

如果需要重新验证静态生成的页面的缓存，可以通过在页面的 `getStaticProps` 函数中设置 `revalidate` prop 来实现。

## [Options](https://nextjs.org/docs/app/api-reference/next-config-js/headers#options)

### [X-DNS-Prefetch-Control X-DNS-预取-控制](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-dns-prefetch-control)

此header控制 DNS 预取，允许浏览器主动对外部链接、图像、CSS、JavaScript 等执行域名解析。此预取在后台执行，因此在需要引用的项目时更有可能解析 DNS。这样可以减少用户单击链接时的延迟。

```json
{
  key: 'X-DNS-Prefetch-Control',
  value: 'on'
}
```

### [Strict-Transport-Security 严格运输安全](https://nextjs.org/docs/app/api-reference/next-config-js/headers#strict-transport-security)

This header informs browsers it should only be accessed using HTTPS, instead of using HTTP. Using the configuration below, all present and future subdomains will use HTTPS for a `max-age` of 2 years. This blocks access to pages or subdomains that can only be served over HTTP.
此header通知浏览器只能使用 HTTPS 访问它，而不是使用 HTTP。使用以下配置，所有当前和未来的子域将使用HTTPS`max-age`2年。这会阻止对只能通过 HTTP 提供的页面或子域的访问。

如果要部署到 Vercel，则不需要此header，因为它会自动添加到所有部署中，除非您在 `next.config.js` 中声明 `headers` 。

```json
{
  key: 'Strict-Transport-Security',
  value: 'max-age=63072000; includeSubDomains; preload'
}
```

### [X-XSS-Protection](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-xss-protection)

This header stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although this protection is not necessary when sites implement a strong [`Content-Security-Policy`](https://nextjs.org/docs/app/api-reference/next-config-js/headers#content-security-policy) disabling the use of inline JavaScript (`'unsafe-inline'`), it can still provide protection for older web browsers that don't support CSP.
此header在检测到反射式跨站点脚本cross-site scripting (XSS) 攻击时阻止页面加载。尽管当站点实现禁用内联 JavaScript （ `'unsafe-inline'` ） 的强 [`Content-Security-Policy`](https://nextjs.org/docs/app/api-reference/next-config-js/headers#content-security-policy) 时，此保护不是必需的，但它仍然可以为不支持 CSP 的旧版 Web 浏览器提供保护。

```js
{
  key: 'X-XSS-Protection',
  value: '1; mode=block'
}
```

### [X-Frame-Options](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-frame-options)

此header指示是否应允许网站显示在 `iframe` 内。这可以防止点击劫持攻击。此header已被 CSP 的 `frame-ancestors` 选项取代，该选项在现代浏览器中具有更好的支持。

```json
{
  key: 'X-Frame-Options',
  value: 'SAMEORIGIN'
}
```

### [Permissions-Policy 权限策略](https://nextjs.org/docs/app/api-reference/next-config-js/headers#permissions-policy)

此header允许您控制可以在浏览器中使用哪些features和 APIs。它以前被命名为 `Feature-Policy` 。您可以在此处查看权限选项的完整列表。

```json
{
  key: 'Permissions-Policy',
  value: 'camera=(), microphone=(), geolocation=(), browsing-topics=()'
}
```

### [X-Content-Type-Options X 内容类型选项](https://nextjs.org/docs/app/api-reference/next-config-js/headers#x-content-type-options)

This header prevents the browser from attempting to guess the type of content if the `Content-Type` header is not explicitly set. This can prevent XSS exploits for websites that allow users to upload and share files. For example, a user trying to download an image, but having it treated as a different `Content-Type` like an executable, which could be malicious. This header also applies to downloading browser extensions. The only valid value for this header is `nosniff`.
如果未显式设置 `Content-Type` header，则此header可防止浏览器尝试猜测内容类型。这可以防止允许用户上传和共享文件的网站受到 XSS 攻击。例如，用户尝试下载图像，但将其视为不同的 `Content-Type` ，如可执行文件，这可能是恶意的。此header也适用于下载浏览器扩展程序。此标头的唯一有效值为 `nosniff` 

```json
{
  key: 'X-Content-Type-Options',
  value: 'nosniff'
}
```

### [Referrer-Policy](https://nextjs.org/docs/app/api-reference/next-config-js/headers#referrer-policy)

此header控制浏览器在从当前网站（源）导航到另一个网站时包含的信息量。您可以在此处阅读有关不同选项的信息。

```json
{
  key: 'Referrer-Policy',
  value: 'origin-when-cross-origin'
}
```

### [Content-Security-Policy 内容安全策略](https://nextjs.org/docs/app/api-reference/next-config-js/headers#content-security-policy)

此header有助于防止跨站点脚本 （XSS）、点击劫持和其他代码注入攻击。内容安全策略 （CSP） 可以指定允许的内容来源，包括脚本、样式表、图像、字体、对象、媒体（音频、视频）、iframe 等。

您可以使用模板字符串添加内容安全策略指令(CSP)。

```js
// Before defining your Security Headers
// add Content Security Policy directives using a template string.
 
const ContentSecurityPolicy = `
  default-src 'self';
  script-src 'self';
  child-src example.com;
  style-src 'self' example.com;
  font-src 'self';
`;
```

当指令使用关键字（如 `self` ）时，将其括在单引号 `''` 中。

In the header's value, replace the new line with a space.
在header的值中，将新行替换为空格。

```json
{
  key: 'Content-Security-Policy',
  value: ContentSecurityPolicy.replace(/\s{2,}/g, ' ').trim()
}
```

