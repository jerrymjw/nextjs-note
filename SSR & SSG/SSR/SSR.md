单页应用程序 (SPA) 在 Web 开发中变得非常普遍，因为它们允许在同一网站内导航而无需刷新页面，从而极大地改善了用户体验。这要归功于 React 和 Vue 等前端库，它们允许通过操作 DOM 直接在浏览器上呈现页面（客户端呈现）。

但是 SPA 是有代价的：网页现在在等待 JavaScript 呈现时初始加载速度变慢，并且搜索引擎爬虫发现它们的可能性降低，这导致 SEO（搜索引擎优化）性能不佳。

这就是为什么服务器端渲染 (SSR) 通过在服务器上完成大部分渲染并将生成的静态 HTML 提供给浏览器来拯救 SPA 框架的原因。这些服务器端生成的页面也可以预呈现，以便可以从 CDN 提供它们或使用替代缓存解决方案来最小化页面下载时间，这通常称为静态站点生成 (SSG)。

但是使用客户端渲染的所有好处呢？SSR 框架通过将服务器发送的标记转换为行为类似于单页应用程序的动态 DOM 来回答这个问题。这个过程通常被称为`hydration`。

### Svelte

[SvelteKit](https://kit.svelte.dev/)是一个[开源框架，用于构建基于](https://github.com/sveltejs/kit)[Svelte 的](https://svelte.dev/)高性能 Web 应用程序，由于集成了[Vite](https://vitejs.dev/)工具，它提供了一种快速的开发体验，可以立即反映更改，而不会丢失应用程序状态。[它是Sapper](https://sapper.svelte.dev/)的继任者。



![SvelteKit](https://static.simply-how.com/server-side-rendering-web-frameworks/svelte/SvelteKit.PNG)



[您可以使用Adapters](https://kit.svelte.dev/docs/adapters)构建针对多个平台的 SvelteKit 网络应用程序，例如：

- Node.js（适配器节点）。常用于服务端渲染
- 静态站点生成（适配器静态）
- Cloudflare Workers（适配器-cloudflare-worker）
- Vercel（适配器-vercel）
- 您还可以[编写自己的适配器](https://kit.svelte.dev/docs/writing-adapters)或使用[社区提供的适配器](https://sveltesociety.dev/components#adapters)

它还带有基于每个应用程序或每个页面的可配置服务器端呈现。你可以：

- 为不需要 JavaScript 的页面禁用水合作用
- 禁用某些页面的客户端路由
- 预呈现静态页面以在构建时生成它们
- 不需要时禁用 SSR

### Next.js

[Next.js](https://nextjs.org/)是一个非常流行的基于 React 的[开源](https://github.com/vercel/next.js)框架，专为生产使用而设计，考虑到开发人员的经验。它还采用约定优于配置的概念。



![Next.js - 功能](https://static.simply-how.com/server-side-rendering-web-frameworks/react/Next.js+-+features.PNG)



它带有对 TypeScript 的开箱即用支持、无组件状态丢失的热重新加载、优化的捆绑、自动路由预取和混合渲染：

- [静态生成](https://nextjs.org/docs/basic-features/pages#static-generation)：页面在构建时呈现，可以在运行时使用[增量静态再生单独更新](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration)
- [服务器端呈现](https://nextjs.org/docs/basic-features/pages#server-side-rendering)：页面在每个请求上动态呈现。这可以在每页的基础上配置。

### Gatsby

[Gatsby](https://www.gatsbyjs.com/)是一个[基于](https://github.com/gatsbyjs/gatsby)React 的开源框架，用于使用静态优先方法创建现代应用程序和网站。[它可以使用插件](https://www.gatsbyjs.com/plugins)轻松集成到外部数据源（如 Wordpress 和 Drupal）。它还提供[主题](https://www.gatsbyjs.com/docs/themes/)作为站点功能、数据源和设计的抽象。

它在每页的基础上带有许多渲染选项：

- 静态站点生成 (SSG)：整个站点在构建时预呈现。这是默认模式。为了优化构建时间，可以启用[增量构建，通过仅重建更改的页面来加快后续构建](https://www.gatsbyjs.com/blog/2020-04-22-announcing-incremental-builds/)
- [延迟静态生成 (DSG)](https://www.gatsbyjs.com/docs/how-to/rendering-options/using-deferred-static-generation/)：在首次请求时将某些页面的呈现延迟到运行时
- [服务器端渲染（SSR）](https://www.gatsbyjs.com/docs/how-to/rendering-options/using-server-side-rendering/)：页面是动态渲染的



![盖茨比云 - SSG vs DSG vs SSR](https://static.simply-how.com/server-side-rendering-web-frameworks/react/Gatsby+Cloud+-+SSG+vs+DSG+vs+SSR.png)

### Nuxt

[Nuxt](https://nuxtjs.org/)是一种流行的[开源](https://github.com/nuxt/nuxt.js)Vue 框架，它遵循约定优于配置范式，允许开发人员非常快速地开始编码，从而提供增强的开发人员体验。



![Nuxt - 直观的开发者体验](https://static.simply-how.com/server-side-rendering-web-frameworks/vue.js/Nuxt+-+Intuitive+Developer+Experience.PNG)



它具有每个页面的自动路由和代码拆分功能，并提供 2 种渲染模式：

- [静态站点生成](https://nuxtjs.org/docs/concepts/static-site-generation/)(SSG)：应用程序在构建阶段呈现。更具体地说，所有`.vue`页面都被转换为 HTML 和 JavaScript 文件。API 调用也缓存在包含生成内容的文件夹中，以避免某些客户端 API 调用。
- [服务器端渲染](https://nuxtjs.org/docs/concepts/server-side-rendering/)：SSR 基于 Node.js。当浏览器从服务器接收到呈现的 HTML 时，显示内容并执行水合过程。完成此过程后，页面将变为交互式。



![Nuxt——服务端渲染步骤](https://static.simply-how.com/server-side-rendering-web-frameworks/vue.js/Nuxt+-+Server-side+rendering+steps.PNG)

### Quasar

[Quasar](https://quasar.dev/)是一个基于 Vue.js 的[开源](https://github.com/quasarframework/quasar)框架，在设计时考虑了性能和响应能力，并具有缓存破坏和摇树优化等功能。它还附带了一套完整的 Vue 组件。



![类星体 - 功能](https://static.simply-how.com/server-side-rendering-web-frameworks/vue.js/Quasar+-+features.PNG)



它允许您使用不同的构建模式构建针对多个平台的应用程序：

- 单页 (SPA)
- 服务器端渲染 (SSR)
- 渐进式网络应用程序 (PWA)
- 浏览器扩展
- 通过 Cordova 或 Capacitor 的移动应用程序（Android、iOS 等）
- 多平台桌面应用程序（使用 Electron）

可以使用 PWA 或 SPA 客户端接管配置服务器端[渲染](https://quasar.dev/quasar-cli-webpack/developing-ssr/configuring-ssr/)。

### Conclusion

大多数 SSR 框架都考虑到了性能和开发人员体验。

这些框架中的大多数都为静态站点生成提供了预渲染选项，Next.js 提供增量静态重新生成，而 Gatsby 允许延迟静态生成。

大多数人在水合作用后将过渡到 SPA，而 Quasar Framework 添加了 PWA 接管的选项。

服务器端渲染可以在每个页面的基础上进行配置，SvelteKit 特别提供了在特定页面上禁用水合作用和客户端路由的可能性。
