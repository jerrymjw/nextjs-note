# Turbopack

Turbopack（测试版）是一个针对JavaScript和TypeScript优化的增量捆绑器，用Rust编写，并内置于Next.js中。

## [Usage 用法](https://nextjs.org/docs/architecture/turbopack#usage)

Turbopack 可以在 Next.js 中的 `pages` 和 `app` 目录中使用，以加快本地开发速度。要启用 Turbopack，请在运行 Next.js 开发服务器时使用 `--turbo` 标志。

```json
{
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

## [Supported Features 支持的功能](https://nextjs.org/docs/architecture/turbopack#supported-features)

要了解有关 Turbopack 当前支持的功能的更多信息，请查看文档。

## [Unsupported Features 不支持的功能](https://nextjs.org/docs/architecture/turbopack#unsupported-features)

Turbopack 目前仅支持 `next dev` ，不支持 `next build` 。我们目前正在努力支持builds，因为我们越来越接近稳定性。