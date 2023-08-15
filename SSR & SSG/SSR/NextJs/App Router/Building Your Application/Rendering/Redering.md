# Rendering

Rendering将您编写的代码转换为用户界面。React 18 和 Next.js 13 引入了渲染应用程序的新方法。

## [渲染环境](https://nextjs.org/docs/app/building-your-application/rendering#rendering-environments)

可以在两种环境中呈现应用程序代码：客户端和服务器。

![Client and Server Environemnts](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fclient-and-server-environments.png&w=3840&q=75)

- **客户端**是指用户设备上向服务器发送应用程序代码请求的浏览器。然后，它将来自服务器的响应转换为用户可以与之交互的界面。
- **服务器**是指数据中心中的计算机，它存储应用程序代码，接收来自客户端的请求，执行一些计算，并发回适当的响应。

> 注意：服务器可以指应用程序部署到的区域、分发应用程序代码的 [边缘网络(Edge Network)](https://vercel.com/docs/concepts/edge-network/overview) 或可以缓存渲染工作结果的内容分发网络 （CDN）[内容分发网络(Content Delivery Networks) (CDNs)](https://developer.mozilla.org/en-US/docs/Glossary/CDN)  中的计算机。

## [Component-level 客户端和服务器渲染](https://nextjs.org/docs/app/building-your-application/rendering#component-level-client-and-server-rendering)

在 React 18 之前，使用 React 渲染应用程序的主要方式是完全在客户端上。Next.js提供了一种更简单的方法，通过生成 HTML 并将其发送到客户端以由 React [冻结](https://react.dev/reference/react-dom/hydrate#hydrating-server-rendered-html)，将应用程序分解为**页面**并在服务器上预渲染。但是，这导致客户端上需要额外的JavaScript才能使初始HTML具有交互性。

现在, [使用服务器和客户端组件 ](https://nextjs.org/docs/getting-started/react-essentials)，React 可以在**客户端和服务器**上渲染，这意味着您可以在**组件级别选择**渲染环境。默认情况下， [`app` 路由器](https://nextjs.org/docs/app/building-your-application/routing#the-app-router)使用**服务器组件**，允许您轻松地在服务器上呈现组件并减少发送到客户端的 JavaScript 量。

## [服务器上的静态和动态渲染](https://nextjs.org/docs/app/building-your-application/rendering#static-and-dynamic-rendering-on-the-server)

除了使用 React 组件进行客户端和服务器端渲染之外，Next.js 还为您提供了使用静态和动态渲染优化服务器上渲染的选项。

### [静态渲染](https://nextjs.org/docs/app/building-your-application/rendering#static-rendering)

使用**静态渲染**，*服务器*和*客户端*组件都可以在生成时在服务器上**预渲染**。工作结果将被[缓存](https://nextjs.org/docs/app/building-your-application/data-fetching/caching)并在后续请求中**重用**。缓存的结果也可以[重新验证](https://nextjs.org/docs/app/building-your-application/data-fetching#revalidating-data)。

> 注意：这相当于 [页面路由器(Pages Router)](https://nextjs.org/docs/pages/building-your-application/rendering).中的[静态站点生成(Static Site Generation) (SSG)](https://nextjs.org/docs/pages/building-your-application/rendering/static-site-generation) 和[增量静态重新生成(Incremental Static Regeneration) (ISR)](https://nextjs.org/docs/pages/building-your-application/rendering/incremental-static-regeneration) 。

服务器和客户端组件在静态渲染期间的渲染方式不同：

- 客户端组件在服务器上预渲染和缓存其 HTML 和 JSON。然后将缓存的结果发送到客户端进行冻结（hydration）。
- 服务器组件由 React 渲染在服务器上，它们的有效负载用于生成 HTML。相同的渲染有效负载也用于冻结客户端上的组件，从而在客户端上不需要 JavaScript。

### [动态渲染](https://nextjs.org/docs/app/building-your-application/rendering#dynamic-rendering)

With Dynamic Rendering, both Server *and* Client Components are rendered on the server at **request time**. The result of the work is not cached.
使用动态渲染时，服务器和客户端组件都在**request time**渲染在服务器上。不缓存工作结果。

> 注意：这相当于页面路由器中的 [服务器端渲染](https://nextjs.org/docs/pages/building-your-application/rendering/server-side-rendering)中的(`getServerSideProps()`)。

## [edge和node.js运行时间](https://nextjs.org/docs/app/building-your-application/rendering#edge-and-nodejs-runtimes)

在服务器上，有两个运行时间可以渲染页面：

- Node.js 运行时间（默认）可以访问生态系统中的所有 Node.js API 和兼容包。
- The **Edge Runtime** is based on [Web APIs](https://nextjs.org/docs/app/api-reference/edge).
  边缘运行时间基于 [Web APIs](https://nextjs.org/docs/app/api-reference/edge)。

## 总结

- 渲染环境分为客户端（传统）和服务端渲染
- 客户端渲染就是通过生成 HTML 并将其发送到客户端以由 React [冻结](https://react.dev/reference/react-dom/hydrate#hydrating-server-rendered-html)，将应用程序分解为**页面**并在服务器上预渲染。但是，这导致客户端上需要额外的JavaScript才能使初始HTML具有交互性。
- react18以后可以直接在服务端渲染，这分为静态渲染和动态渲染
- 静态渲染主要是预渲染，客户端组件在服务器上预渲染和缓存其 HTML 和 JSON。然后将缓存的结果发送到客户端进行冻结（hydration），服务器组件由 React 渲染在服务器上，它们的有效负载用于生成 HTML。相同的渲染有效负载也用于冻结客户端上的组件，从而在客户端上不需要 JavaScript。
- 动态渲染要根据其运行时间来决定渲染的时机，比如state改变，那么这就是一个渲染的时机