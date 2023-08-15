# Accessibility

Next.js 团队致力于让所有开发人员（及其最终用户）都能访问 Next.js。默认情况下，通过向 Next.js 添加辅助功能，我们的目标是使 Web 对每个人都更具包容性。

## [Route Announcements 路线公告](https://nextjs.org/docs/architecture/accessibility#route-announcements)

当在服务器上渲染的页面之间转换时（例如，使用 `<a href>` 标记），屏幕阅读器和其他辅助技术在页面加载时播报页面标题，以便用户了解页面已更改。

除了传统的页面导航外，Next.js 还支持客户端转换以提高性能（使用 `next/link` ）。为了确保客户端转换也宣布到辅助技术，Next.js 默认包含一个路由播音员。

Next.js 路由播音员首先通过检查 `document.title` 、`<h1>`元素和最后的 URL 路径名来查找要播报的页面名称。为了获得最易于访问的用户体验，请确保应用程序中的每个页面都具有唯一的描述性标题。

## [Linting](https://nextjs.org/docs/architecture/accessibility#linting)

Next.js 提供开箱即用的集成 ESLint 体验，包括 Next.js 的自定义规则。默认情况下，Next.js 包含 `eslint-plugin-jsx-a11y` 以帮助及早发现辅助功能问题，包括警告：

- [aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [aria-proptypes](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-proptypes.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [aria-unsupported-elements](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/aria-unsupported-elements.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [role-has-required-aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/role-has-required-aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)
- [role-supports-aria-props](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/role-supports-aria-props.md?rgh-link-date=2021-06-04T02%3A10%3A36Z)

例如，此插件有助于确保您将替代文本添加到 `img` 标签、使用正确的 `aria-*` 个属性、使用正确的 `role` 属性等。

## [Accessibility Resources 辅助功能资源](https://nextjs.org/docs/architecture/accessibility#accessibility-resources)

- [WebAIM WCAG checklist WebAIM WCAG 清单](https://webaim.org/standards/wcag/checklist)
- [WCAG 2.1 Guidelines WCAG 2.1 指南](https://www.w3.org/TR/WCAG21/)
- [The A11y Project A11y项目](https://www.a11yproject.com/)
- 检查前景和背景元素之间的颜色对比度 [color contrast ratios](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Understanding_WCAG/Perceivable/Color_contrast) 
- Use [`prefers-reduced-motion`](https://web.dev/prefers-reduced-motion/) when working with animations
  处理动画时使用 [`prefers-reduced-motion`](https://web.dev/prefers-reduced-motion/) 