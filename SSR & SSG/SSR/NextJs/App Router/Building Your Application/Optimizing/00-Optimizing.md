# Optimizations

## [内置组件](https://nextjs.org/docs/app/building-your-application/optimizing#built-in-components)

内置组件消除了实现常见 UI 优化的复杂性。这些组件是：

- **Image**：基于原生 `<img>` 元素构建。图像组件通过**延迟加载(lazy loading)**和根据设备大小自动调整图像大小来优化图像的性能。
- **Link**: 基于原生 `<a>` 标记构建。Link组件在后台预取页面，以实现更快、更流畅的页面过渡。
- **Scripts**: 基于原生 `<script>` 标记构建。脚本组件使您可以控制第三方脚本的加载和执行。

## [Metadata](https://nextjs.org/docs/app/building-your-application/optimizing#metadata)

Metadata可帮助搜索引擎更好地了解您的内容（这可以带来更好的 SEO），并允许您自定义内容在社交媒体上的渲染方式，帮助您在各种平台上创建更具吸引力和一致的用户体验。

Next.js 中的Metadata API 允许你修改页面的 `<head>` 元素。您可以通过两种方式配置Metadata：

- **基于配置的元数据**：导出静态Metadata对象或 `layout.js` 或 `page.js` 文件中的动态generateMetadata函数。
- **基于文件的元数据**：将静态或动态生成的特殊文件添加到路由段。

此外，您可以使用 JSX 和 CSS 以及 [imageResponse](https://nextjs.org/docs/app/api-reference/functions/image-response) 构造函数创建动态 Open Graph Images。

## [Static Assets](https://nextjs.org/docs/app/building-your-application/optimizing#static-assets)

Next.js `/public` 文件夹可用于提供静态资产，如图像、字体和其他文件。 `/public` 中的文件也可以由 CDN 提供程序缓存，以便高效交付。

## [分析和监控](https://nextjs.org/docs/app/building-your-application/optimizing#analytics-and-monitoring)

For large applications, Next.js integrates with popular analytics and monitoring tools to help you understand how your application is performing. Learn more in the , [OpenTelemetry](https://nextjs.org/docs/pages/building-your-application/optimizing/open-telemetry), and [Instrumentation](https://nextjs.org/docs/pages/building-your-application/optimizing/instrumentation) guides.
对于大型应用程序，Next.js 与常用的分析和监视工具集成，以帮助您了解应用程序的性能。有关详细信息，请参阅 [开放遥测](https://nextjs.org/docs/pages/building-your-application/optimizing/open-telemetry) 和 [检测指南](https://nextjs.org/docs/pages/building-your-application/optimizing/instrumentation) 。

## 总结

- 有三个内置组件image， link和scripts
- 元数据分为两种：基于配置的和基于文件的
- `/public`文件夹可以用来提供静态资产，图像文字和其他文件等。这里面的文件也可以由CDN来提供程序缓存，以便高效交付
