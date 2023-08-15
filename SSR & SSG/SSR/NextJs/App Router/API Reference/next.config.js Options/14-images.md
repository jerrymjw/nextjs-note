# images

如果要使用云服务来优化图像，而不是使用 Next.js 内置映像优化 API，则可以使用以下各项配置 `next.config.js` ：

```js
module.exports = {
  images: {
    loader: 'custom',
    loaderFile: './my/image/loader.js',
  },
};
```

此 `loaderFile` 必须指向相对于 Next.js 应用程序的根目录的文件。该文件必须导出返回字符串的默认函数，例如：

```js
export default function myImageLoader({ src, width, quality }) {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`;
}
```

或者，您可以使用 [`loader` prop](https://nextjs.org/docs/pages/api-reference/components/image#loader) 将函数传递给 `next/image` 的每个实例。

## [Example Loader Configuration](https://nextjs.org/docs/app/api-reference/next-config-js/images#example-loader-configuration)

### [Akamai](https://nextjs.org/docs/app/api-reference/next-config-js/images#akamai)

```js
// Docs: https://techdocs.akamai.com/ivm/reference/test-images-on-demand
export default function akamaiLoader({ src, width, quality }) {
  return `https://example.com/${src}?imwidth=${width}`;
}
```

### [Cloudinary](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudinary)

```js
// Demo: https://res.cloudinary.com/demo/image/upload/w_300,c_limit,q_auto/turtles.jpg
export default function cloudinaryLoader({ src, width, quality }) {
  const params = ['f_auto', 'c_limit', `w_${width}`, `q_${quality || 'auto'}`];
  return `https://example.com/${params.join(',')}${src}`;
}
```

### [Cloudflare](https://nextjs.org/docs/app/api-reference/next-config-js/images#cloudflare)

```js
// Docs: https://developers.cloudflare.com/images/url-format
export default function cloudflareLoader({ src, width, quality }) {
  const params = [`width=${width}`, `quality=${quality || 75}`, 'format=auto'];
  return `https://example.com/cdn-cgi/image/${params.join(',')}/${src}`;
}
```

### [Contentful](https://nextjs.org/docs/app/api-reference/next-config-js/images#contentful)

```js
// Docs: https://www.contentful.com/developers/docs/references/images-api/
export default function contentfulLoader({ src, quality, width }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set('fm', 'webp');
  url.searchParams.set('w', width.toString());
  url.searchParams.set('q', quality.toString() || '75');
  return url.href;
}
```

## [Fastly](https://nextjs.org/docs/app/api-reference/next-config-js/images#fastly)

```js
// Docs: https://developer.fastly.com/reference/io/
export default function fastlyLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set('auto', 'webp');
  url.searchParams.set('width', width.toString());
  url.searchParams.set('quality', quality.toString() || '75');
  return url.href;
}
```

## [Gumlet](https://nextjs.org/docs/app/api-reference/next-config-js/images#gumlet)

```js
// Docs: https://docs.gumlet.com/reference/image-transform-size
export default function gumletLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  url.searchParams.set('format', 'auto');
  url.searchParams.set('w', width.toString());
  url.searchParams.set('q', quality.toString() || '75');
  return url.href;
}
```

### [ImageEngine](https://nextjs.org/docs/app/api-reference/next-config-js/images#imageengine)

```js
// Docs: https://support.imageengine.io/hc/en-us/articles/360058880672-Directives
export default function imageengineLoader({ src, width, quality }) {
  const compression = 100 - (quality || 50)
  const params = [`w_${width}`, `cmpr_${compression}`)]
  return `https://example.com${src}?imgeng=/${params.join('/')`
}
```

### [Imgix](https://nextjs.org/docs/app/api-reference/next-config-js/images#imgix)

```js
// Demo: https://static.imgix.net/daisy.png?format=auto&fit=max&w=300
export default function imgixLoader({ src, width, quality }) {
  const url = new URL(`https://example.com${src}`);
  const params = url.searchParams;
  params.set('auto', params.getAll('auto').join(',') || 'format');
  params.set('fit', params.get('fit') || 'max');
  params.set('w', params.get('w') || width.toString());
  params.set('q', quality.toString() || '50');
  return url.href;
}
```

### [Thumbor](https://nextjs.org/docs/app/api-reference/next-config-js/images#thumbor)

```js
// Docs: https://thumbor.readthedocs.io/en/latest/
export default function thumborLoader({ src, width, quality }) {
  const params = [`${width}x0`, `filters:quality(${quality || 75})`];
  return `https://example.com${params.join('/')}${src}`;
}
```

