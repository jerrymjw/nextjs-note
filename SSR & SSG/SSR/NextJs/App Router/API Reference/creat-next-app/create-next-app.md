# create-next-app

开始使用 Next.js 的最简单方法是使用 `create-next-app` 。此 CLI 工具使您能够快速开始构建新的 Next.js 应用程序，并为您设置好一切。可以使用默认的 Next.js 模板或使用官方 Next.js 示例之一创建新应用。若要开始，请使用以下命令：

### [Interactive 交互式](https://nextjs.org/docs/app/api-reference/create-next-app#interactive)

您可以通过运行以下命令以交互方式创建新项目：

```terminal
npx create-next-app@latest
 
yarn create next-app
 
pnpm create next-app
```

然后，系统将询问您以下提示：

```terminal
What is your project named?  my-app
Would you like to add TypeScript with this project?  Y/N
Would you like to use ESLint with this project?  Y/N
Would you like to use Tailwind CSS with this project? Y/N
Would you like to use the `src/ directory` with this project? Y/N
What import alias would you like configured? `@/*`
```

回答提示后，将根据您的答案使用正确的配置创建一个新项目。

### [Non-interactive 非交互式](https://nextjs.org/docs/app/api-reference/create-next-app#non-interactive)

还可以传递命令行参数以非交互方式设置新项目。

此外，您可以通过以 `--no-` 为前缀（例如 `--no-eslint` ）来否定默认选项。

请参阅 `create-next-app --help` ：

```terminal
Usage: create-next-app <project-directory> [options]
 
Options:
  -V, --version                        output the version number
  --ts, --typescript
 
    Initialize as a TypeScript project. (default)
 
  --js, --javascript
 
    Initialize as a JavaScript project.
 
  --tailwind
 
    Initialize with Tailwind CSS config. (default)
 
  --eslint
 
    Initialize with ESLint config.
 
  --src-dir
 
    Initialize inside a `src/` directory.
 
  --import-alias <alias-to-configure>
 
    Specify import alias to use (default "@/*").
 
  --use-npm
 
    Explicitly tell the CLI to bootstrap the app using npm
 
  --use-pnpm
 
    Explicitly tell the CLI to bootstrap the app using pnpm
 
  -e, --example [name]|[github-url]
 
    An example to bootstrap the app with. You can use an example name
    from the official Next.js repo or a GitHub URL. The URL can use
    any branch and/or subdirectory
 
  --example-path <path-to-example>
 
    In a rare case, your GitHub URL might contain a branch name with
    a slash (e.g. bug/fix-1) and the path to the example (e.g. foo/bar).
    In this case, you must specify the path to the example separately:
    --example-path foo/bar
 
  --reset-preferences
 
    Explicitly tell the CLI to reset any stored preferences
 
  -h, --help                           output usage information
```

### [Why use Create Next App?”？](https://nextjs.org/docs/app/api-reference/create-next-app#why-use-create-next-app)

`create-next-app` 允许您在几秒钟内创建新的 Next.js 应用。它由Next.js的创建者正式维护，包括许多好处：

- **Interactive Experience**交互式体验：运行 `npx create-next-app@latest` （不带参数）将启动交互式体验，指导你完成项目设置。
- **Zero Dependencies**零依赖：初始化项目只需一秒钟。“创建Next App”的依赖项为零。
- **Offline Support**脱机支持：创建Next App用将自动检测您是否处于脱机状态，并使用本地包缓存启动项目。
- **Support for Examples**支持示例：创建Next App可以使用 Next.js 示例集合中的示例（例如 `npx create-next-app --example api-routes` ）引导应用程序。
- **Tested**已测试：该软件包是 Next.js monorepo 的一部分，并使用与 Next.js 本身相同的集成测试套件进行测试，确保它在每个版本中都按预期工作。