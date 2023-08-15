# incrementalCacheHandlerPath

In Next.js, 默认缓存处理程序使用文件系统缓存。这不需要配置，但是，您可以使用 `next.config.js` 中的 `incrementalCacheHandlerPath` 字段自定义缓存处理程序。

```js
module.exports = {
  incrementalCacheHandlerPath: './cache-handler.js',
};
```

下面是自定义缓存处理程序的示例:

```js
const cache = new Map();
 
module.exports = class CacheHandler {
  constructor(options) {
    this.options = options;
    this.cache = {};
  }
 
  async get(key) {
    return cache.get(key);
  }
 
  async set(key, data) {
    cache.set(key, {
      value: data,
      lastModified: Date.now(),
    });
  }
};
```

## [API Reference](https://nextjs.org/docs/app/api-reference/next-config-js/incrementalCacheHandlerPath#api-reference)

cache handler可以实现以下方法： `get` 、 `set` 和 `revalidateTag` 。

### [`get()`](https://nextjs.org/docs/app/api-reference/next-config-js/incrementalCacheHandlerPath#get)

| Parameter | Type     | Description                               |
| --------- | -------- | ----------------------------------------- |
| `key`     | `string` | The key to the cached value. 缓存值的键。 |

Returns the cached value or `null` if not found.
返回缓存的值，如果未找到，则返回 `null` 。

### [`set()`](https://nextjs.org/docs/app/api-reference/next-config-js/incrementalCacheHandlerPath#set)

| Parameter | Type                         | Description                                           |
| --------- | ---------------------------- | ----------------------------------------------------- |
| `key`     | `string`                     | The key to store the data under. 用于存储数据的密钥。 |
| `data`    | Data or `null` 数据或 `null` | The data to be cached. 要缓存的数据。                 |

Returns `Promise<void>`. 返回 `Promise<void>` 。

### [`revalidateTag()`](https://nextjs.org/docs/app/api-reference/next-config-js/incrementalCacheHandlerPath#revalidatetag)

| Parameter | Type     | Description                                         |
| --------- | -------- | --------------------------------------------------- |
| `tag`     | `string` | The cache tag to revalidate. 要重新验证的缓存标记。 |

Returns `Promise<void>`. Learn more about [revalidating data](https://nextjs.org/docs/app/building-your-application/data-fetching/revalidating) or the [`revalidateTag()`](https://nextjs.org/docs/app/api-reference/functions/revalidateTag) function.
返回 `Promise<void>` 。详细了解如何重新验证数据或 `revalidateTag()` 函数。