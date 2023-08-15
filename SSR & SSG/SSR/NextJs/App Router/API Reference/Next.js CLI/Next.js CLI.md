# Next.js CLI

Next.js CLI 允许您start, build, and export应用程序。

要获取可用 CLI 命令的列表，请在项目目录中运行以下命令：

```terminal
npx next -h
```

输出应如下所示：

```terminal
Usage
  $ next <command>
 
Available commands
  build, start, export, dev, lint, telemetry, info
 
Options
  --version, -v   Version number
  --help, -h      Displays this message
 
For more information run a command with the --help flag
  $ next build --help
```

您可以将任何节点参数传递给 `next` 命令：

```terminal
NODE_OPTIONS='--throw-deprecation' next
NODE_OPTIONS='-r esm' next
NODE_OPTIONS='--inspect' next
```

> 注意：在没有命令的情况下运行 `next` 与运行 `next dev` 相同

## [Build](https://nextjs.org/docs/app/api-reference/next-cli#build)

`next build` 创建应用程序的优化过的production。输出显示有关每个路由的信息。

- **Size** – 导航到页面客户端时下载的资产数。每个路由的大小仅包括其依赖项。
- **First Load JS** – 从服务器访问页面时下载的资产数量。所有人共享的 JS 数量显示为单独的指标。

这两个值**都使用 gzip 压缩**。第一个荷载用绿色、黄色或红色表示。以绿色为目标，实现高性能应用程序。

您可以使用 `next build` 中的 `--profile` 标志为 React 启用生产分析。这需要 Next.js 9.5：

```terminal
next build --profile
```

之后，您可以像在开发中一样使用探查器。

可以在 `next build` 中使用 `--debug` 标志启用更详细的生成输出。这需要 Next.js 9.5.3：

```terminal
next build --debug
```

启用此标志后，将显示其他生成输出，例如rewrites, redirects, and headers 。

## [Development 开发](https://nextjs.org/docs/app/api-reference/next-cli#development)

`next dev` 在开发模式下启动应用程序，具有热代码重新加载(hot-code reloading)、错误报告(error reporting)等功能：

默认情况下，应用程序将从 `http://localhost:3000` 启动。默认端口可以用 `-p` 更改，如下所示：

```terminal
npx next dev -p 4000
```

或者使用 `PORT` 环境变量：

```terminal
PORT=4000 npx next dev
```

> 注意：不能在 `.env` 中设置 `PORT` ，因为启动 HTTP 服务器发生在初始化任何其他代码之前。

您还可以将主机名设置为不同于默认值 `0.0.0.0` ，这对于使应用程序可用于网络上的其他设备非常有用。默认主机名可以用 `-H` 更改，如下所示：

```terminal
npx next dev -H 192.168.1.2
```

## [Production 生产](https://nextjs.org/docs/app/api-reference/next-cli#production)

`next start` 在生产模式下启动应用程序。应用程序应首先使用 `next build` 进行编译。

默认情况下，应用程序将从 `http://localhost:3000` 启动。默认端口可以用 `-p` 更改，如下所示：

```terminal
npx next start -p 4000
```

或者使用 `PORT` 环境变量：

```terminal
PORT=4000 npx next start
```

> 注意：不能在 `.env` 中设置 `PORT` ，因为启动 HTTP 服务器发生在初始化任何其他代码之前。

> 注意： `next start` 不能与 `output: 'standalone'` 或 `output: 'export'` 一起使用。

### [Keep Alive Timeout 保持活动超时](https://nextjs.org/docs/app/api-reference/next-cli#keep-alive-timeout)

When deploying Next.js behind a downstream proxy (e.g. a load-balancer like AWS ELB/ALB) it's important to configure Next's underlying HTTP server with [keep-alive timeouts](https://nodejs.org/api/http.html#http_server_keepalivetimeout) that are *larger* than the downstream proxy's timeouts. Otherwise, once a keep-alive timeout is reached for a given TCP connection, Node.js will immediately terminate that connection without notifying the downstream proxy. This results in a proxy error whenever it attempts to reuse a connection that Node.js has already terminated.
在下游代理（例如 AWS ELB/ALB）后面部署 Next 时.js请务必使用 Keep-alive Timeout大于下游代理的Timeout来配置 Next 的底层 HTTP 服务器。否则，一旦给定 TCP 连接达到Keep Alive Timeout，Node.js 将立即终止该连接，而不通知下游代理。每当它尝试重用 Node.js 已终止的连接时，这都会导致代理错误。

To configure the timeout values for the production Next.js server, pass `--keepAliveTimeout` (in milliseconds) to `next start`, like so:
配置生产 Next.js 服务器的Timeout值，将 `--keepAliveTimeout` （以毫秒为单位）传递给 `next start` ，如下所示：

```terminal
npx next start --keepAliveTimeout 70000
```

## [Lint](https://nextjs.org/docs/app/api-reference/next-cli#lint)

`next lint` 对 `pages/` 、 `app` （仅当启用了实验性 `appDir` 功能）、 `components/` 、 `lib/` 和 `src/` 目录中的所有文件运行 ESLint。它还提供了一个引导式设置，用于安装任何所需的依赖项（如果应用程序中尚未配置 ESLint）。

如果您有其他要 lint 的目录，可以使用 `--dir` 标志指定它们：

```terminal
next lint --dir utils
```

## [Telemetry 遥测](https://nextjs.org/docs/app/api-reference/next-cli#telemetry)

接下来.js收集有关常规使用情况的**完全匿名**遥测数据。参与此匿名计划是可选的，如果您不想分享任何信息，您可以选择退出。

## [Next Info](https://nextjs.org/docs/app/api-reference/next-cli#next-info)

`next info` 打印有关当前系统的相关详细信息，可用于报告 Next.js 错误。这些信息包括操作系统平台/架构/版本、二进制文件（Node.js、npm、Yarn、pnpm）和 npm 包版本（ `next` 、 `react` 、 `react-dom` ）。

在项目的根目录中运行以下命令：

```terminal
next info
```

将为您提供类似以下示例的信息：

```terminal
 
    Operating System:
      Platform: linux
      Arch: x64
      Version: #22-Ubuntu SMP Fri Nov 5 13:21:36 UTC 2021
    Binaries:
      Node: 16.13.0
      npm: 8.1.0
      Yarn: 1.22.17
      pnpm: 6.24.2
    Relevant packages:
      next: 12.0.8
      react: 17.0.2
      react-dom: 17.0.2

```

然后，此信息应粘贴到 GitHub 问题中。