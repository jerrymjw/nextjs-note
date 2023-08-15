## 两种形式的预渲染

Next.js有两种形式的预渲染：**Static Generation**(静态生成)和服务器端渲染(**Server-side Rendering**)。区别在于它**何时为页面生成 HTML**。

- **静态生成**是在生成时生成 HTML 的预渲染方法。然后，预渲染的 HTML 将在每个请求中重复使用。
- **服务器端渲染（SSR）**是在每个请求上生成 HTML 的预渲染方法。

![img](https://nextjs.org/static/images/learn/data-fetching/static-generation.png)

![img](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering.png)

> 在开发模式下（运行 `npm run dev` 或 `yarn dev` 时），页面在每个请求上预渲染。这也适用于静态生成，使其更易于开发。进入生产环境时，静态生成将在build时发生一次，而不是在每个请求上发生。

### Per-page Basis

重要的是，Next.js 允许您选择要用于每个页面的预渲染form。可以通过对大多数页面使用静态生成，对其他页面使用服务器端渲染来创建“**hybrid**”Next.js 应用。

![img](https://nextjs.org/static/images/learn/data-fetching/per-page-basis.png)

### 何时使用 [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) v.s. [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering) 

我们建议**尽可能使用静态生成**（带数据和不带数据），因为页面可以build一次并由 CDN 提供服务，这比让服务器在每个请求上渲染页面要快得多。

You can use [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) for many types of pages, including:
您可以将静态生成用于多种类型的页面，包括：

- Marketing pages
- Blog posts
- E-commerce product listings 电子商务产品列表
- Help and documentation 帮助和文档

您应该问自己：“我可以在用户请求之前预呈现此页面吗？如果答案是肯定的，那么您应该选择 [静态生成](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) .

另一方面，如果无法在用户请求之前预呈现页面，则[静态生成](https://nextjs.org/docs/basic-features/pages#static-generation-recommended)不是一个好主意。也许您的页面显示经常更新的数据(比如股票)，并且页面内容会根据每个请求而更改。

在这种情况下，您可以使用 [服务器端渲染](https://nextjs.org/docs/basic-features/pages#server-side-rendering) 。它会变慢，但预渲染的页面将始终是最新的。或者，您可以跳过预渲染并使用客户端 JavaScript 来填充经常更新的数据。

 ## 总结

- 当我们需要在与用户交互之前（JS注入之前）就需要展示页面那么就使用预渲染。当JS注入（hydrate）后，使其成为一个可以进行动态交互的状态
- 更新完数据后我们可以再进行SSR
- Hybrid其实很常见，就是先进行预渲染然后JS注入，交互后，hydrate然后进行SSR

