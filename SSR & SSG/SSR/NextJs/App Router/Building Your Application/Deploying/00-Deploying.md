# Deploying

## [Next.js Build API](https://nextjs.org/docs/app/building-your-application/deploying#nextjs-build-api)

`next build` 会生成用于生产的应用程序的优化版本。此标准输出包括：

- 使用 `getStaticProps` 或自动静态优化 [Automatic Static Optimization](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization)的页面的 HTML 文件
- 全局样式或单独范围样式的 CSS 文件
- 用于从 Next.js 服务器预渲染动态内容的 JavaScript
- 通过 React 在客户端进行交互的 JavaScript

此输出在 `.next` 文件夹中生成：

- `.next/static/chunks/pages` – 此文件夹中的每个 JavaScript 文件都与具有相同名称的路由相关。例如， `.next/static/chunks/pages/about.js` 是在应用程序中查看 `/about` 路由时加载的 JavaScript 文件
- `.next/static/media` – 从 `next/image` 静态导入的图像被哈希并复制到此处
- `.next/static/css` – 应用程序中所有页面的全局 CSS 文件
- `.next/server/pages` – 从服务器预渲染的 HTML 和 JavaScript 入口点。`.nft.json` 文件是在启用输出文件跟踪[Output File Tracing](https://nextjs.org/docs/pages/api-reference/next-config-js/output) 时创建的，其中包含依赖于给定页面的所有文件路径。
- `.next/server/chunks` – 在整个应用程序中的多个位置使用的共享 JavaScript 块
- `.next/cache` – 来自 Next.js 服务器的生成缓存和缓存图像、响应和页面的输出。使用缓存有助于缩短构建时间并提高加载图像的性能

`.next` 中的所有 JavaScript 代码都已**编译**，浏览器捆绑包已**缩小**，以帮助实现最佳性能并支持所有现代浏览器 [all modern browsers](https://nextjs.org/docs/architecture/supported-browsers)。

## [Managed Next.js with Vercel](https://nextjs.org/docs/app/building-your-application/deploying#managed-nextjs-with-vercel)

Vercel 是无需配置即可部署 Next.js 应用程序的最快方法。

部署到 Vercel 时，平台会自动检测 Next.js，运行 `next build` ，并为您优化构建输出，包括：

- 跨部署保留缓存资产（如果未更改）
- [Immutable deployments](https://vercel.com/features/previews?utm_source=next-site&utm_medium=docs&utm_campaign=next-website) with a unique URL for every commit
  不可变部署[Immutable deployments](https://vercel.com/features/previews?utm_source=next-site&utm_medium=docs&utm_campaign=next-website) ，每次提交都有一个唯一的 URL
- 如果可能，页面会自动静态优化
- 资产（JavaScript，CSS，图像，字体）被压缩并从全球边缘网络提供
- API 路由会自动优化为可以无限扩展的独立无服务器函数
- 中间件自动优化为边缘功能，具有零冷启动和即时启动

此外，Vercel还提供以下功能：

- 通过 Next.js 速度洞察自动监控性能
- 自动 HTTPS 和 SSL 证书
- 自动CI / CD（通过GitHub，GitLab，Bitbucket等）
- 支持环境变量 [Environment Variables](https://vercel.com/docs/environment-variables?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)
- Support for自定义域 [Custom Domains](https://vercel.com/docs/custom-domains?utm_source=next-site&utm_medium=docs&utm_campaign=next-website) 
- Support for [Image Optimization](https://nextjs.org/docs/pages/building-your-application/optimizing/images) with `next/image`
  用 `next/image`支持 [Image Optimization](https://nextjs.org/docs/pages/building-your-application/optimizing/images) 
- Instant global deployments via `git push`
  通过 `git push` 即时全局部署

## [Self-Hosting](https://nextjs.org/docs/app/building-your-application/deploying#self-hosting)

You can self-host Next.js with support for all features using Node.js or Docker. You can also do a Static HTML Export, which [has some limitations](https://nextjs.org/docs/app/building-your-application/deploying/static-exports).
您可以自托管 Next.js并支持使用 Node.js 或 Docker 的所有功能。您还可以执行静态 HTML 导出，这有一些限制。

### [Node.js Server](https://nextjs.org/docs/app/building-your-application/deploying#nodejs-server)

Next.js can be deployed to any hosting provider that supports Node.js. For example, [AWS EC2](https://aws.amazon.com/ec2/) or a [DigitalOcean Droplet](https://www.digitalocean.com/products/droplets/).
接下来.js可以部署到任何支持 Node.js 的托管提供商。例如，AWS EC2 或 DigitalOcean Droplet。

First, ensure your `package.json` has the `"build"` and `"start"` scripts:
首先，确保 `package.json` 具有 `"build"` 和 `"start"` 脚本：

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

然后，运行 `npm run build` 以生成应用程序。最后，运行 `npm run start` 以启动 Node.js 服务器。此服务器支持 Next.js 的所有功能。

> 如果使用 [`next/image`](https://nextjs.org/docs/pages/building-your-application/optimizing/images)，请考虑通过在项目目录中运行 `npm install sharp` 来添加 `sharp` 以提高生产环境中的性能图像优化 [Image Optimization](https://nextjs.org/docs/pages/building-your-application/optimizing/images)。在 Linux 平台上， `sharp` 可能需要额外的配置 [additional configuration](https://sharp.pixelplumbing.com/install#linux-memory-allocator) 以防止过多的内存使用。

### [Docker Image](https://nextjs.org/docs/app/building-your-application/deploying#docker-image)

Next.js can be deployed to any hosting provider that supports [Docker](https://www.docker.com/) containers. You can use this approach when deploying to container orchestrators such as [Kubernetes](https://kubernetes.io/) or [HashiCorp Nomad](https://www.nomadproject.io/), or when running inside a single node in any cloud provider.
接下来.js可以部署到支持 [Docker](https://www.docker.com/) 容器的任何托管提供商。在部署到容器业务流程协调程序（如 [Kubernetes](https://kubernetes.io/) 或 [HashiCorp Nomad](https://www.nomadproject.io/)）时，或者在任何云提供商的单个节点内运行时，可以使用此方法。

1. [Install Docker](https://docs.docker.com/get-docker/) on your machine
2. Clone the [with-docker](https://github.com/vercel/next.js/tree/canary/examples/with-docker) example
3. Build your container: `docker build -t nextjs-docker .`
4. Run your container: `docker run -p 3000:3000 nextjs-docker`

如果您需要在多个环境中使用不同的环境变量，请查看我们的 with-docker-multi-env 示例。

### [Static HTML Export 静态 HTML 导出](https://nextjs.org/docs/app/building-your-application/deploying#static-html-export)

If you’d like to do a static HTML export of your Next.js app, follow the directions on our [Static HTML Export documentation](https://nextjs.org/docs/app/building-your-application/deploying/static-exports).
如果要对 Next.js 应用执行静态 HTML 导出，请按照静态 HTML 导出文档 [Static HTML Export documentation](https://nextjs.org/docs/app/building-your-application/deploying/static-exports)中的说明进行操作。

## [Other Services](https://nextjs.org/docs/app/building-your-application/deploying#other-services)

以下服务支持Next.js `v12+`。下面，你将找到将 Next.js 部署到每个服务的示例或指南。

### [Managed Server](https://nextjs.org/docs/app/building-your-application/deploying#managed-server)

- [AWS Copilot](https://aws.github.io/copilot-cli/)
- [Digital Ocean App Platform 数字海洋应用平台](https://docs.digitalocean.com/tutorials/app-nextjs-deploy/)
- [Google Cloud Run 谷歌云运行](https://github.com/vercel/next.js/tree/canary/examples/with-docker)
- [Heroku](https://elements.heroku.com/buildpacks/mars/heroku-nextjs)
- [Railway](https://docs.railway.app/getting-started)
- [Render](https://render.com/docs/deploy-nextjs-app)

> **Note**: 还有一些托管平台允许您使用 Dockerfile，如上例所示。

### [Static Only](https://nextjs.org/docs/app/building-your-application/deploying#static-only)

以下服务仅支持使用 `output: 'export'` 部署 Next.js。

- [GitHub Pages](https://github.com/vercel/next.js/tree/canary/examples/github-pages)

您还可以手动将输出从 [`output: 'export'`](https://nextjs.org/docs/app/building-your-application/deploying/static-exports) 部署到任何静态托管提供程序，通常通过 CI/CD 管道，如 GitHub Actions、Jenkins、AWS CodeBuild、Circle CI、Azure Pipelines 等。

### [Serverless](https://nextjs.org/docs/app/building-your-application/deploying#serverless)

- [AWS Amplify](https://aws.amazon.com/blogs/mobile/amplify-next-js-13/)
- [Azure Static Web Apps Azure 静态 Web Apps](https://learn.microsoft.com/en-us/azure/static-web-apps/nextjs)
- [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-nextjs-site/)
- [Firebase](https://firebase.google.com/docs/hosting/nextjs)
- [Netlify](https://docs.netlify.com/integrations/frameworks/next-js)
- [Terraform](https://github.com/milliHQ/terraform-aws-next-js)
- [SST](https://docs.sst.dev/start/nextjs)

> **Note**: 并非所有无服务器提供程序都从 `next start` 开始实现 [Next.js Build API](https://nextjs.org/docs/app/building-your-application/deploying#nextjs-build-api) 。请与提供商联系，了解支持哪些功能。

## [Automatic Updates](https://nextjs.org/docs/app/building-your-application/deploying#automatic-updates)

部署 Next.js 应用程序时，您希望查看最新版本，而无需重新加载。

Next.js将在路由时在后台自动加载最新版本的应用程序。对于客户端导航， `next/link` 将暂时充当正常的 `<a>` 标记。

**Note**: 如果新页面（具有旧版本）已被 `next/link` 预取，则 Next.js 将使用旧版本。导航到尚未预取（且未在 CDN 级别缓存）的页面将加载最新版本。

## [Manual Graceful shutdowns 手动平稳关闭](https://nextjs.org/docs/app/building-your-application/deploying#manual-graceful-shutdowns)

Sometimes you might want to run some cleanup code on process signals like `SIGTERM` or `SIGINT`.
有时，您可能希望对进程信号（如  `SIGTERM` 或 `SIGINT` ）运行一些清理代码。

您可以通过将 env 变量 `NEXT_MANUAL_SIG_HANDLE` 设置为 `true` 然后在 `_document.js` 文件中为该信号注册一个处理程序来做到这一点。请注意，您需要直接在系统 env 变量中注册 env 变量，而不是在 `.env` 文件中注册。

```json
{
  "scripts": {
    "dev": "NEXT_MANUAL_SIG_HANDLE=true next dev",
    "build": "next build",
    "start": "NEXT_MANUAL_SIG_HANDLE=true next start"
  }
}
```

```js
```

