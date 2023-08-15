# Fast Refresh

快速刷新是 Next.js 的一项功能，它为您提供有关对 React 组件所做的编辑的即时反馈。默认情况下，在 **9.4 或更高版本上**的所有 Next.js 应用程序中启用快速刷新。启用“Next.js快速刷新”后，大多数编辑应在一秒钟内可见，而**不会丢失组件状态**。

## [How It Works 工作原理](https://nextjs.org/docs/architecture/fast-refresh#how-it-works)

- 如果您编辑的文件**仅导出 React 组件**，则快速刷新将仅更新该文件的代码，并重新渲染您的组件。您可以编辑该文件中的任何内容，包括样式、渲染逻辑、事件处理程序或效果。
- 如果您编辑的文件导出不是 React 组件，快速刷新将重新运行该文件以及导入该文件的其他文件。因此，如果 `Button.js` 和 `Modal.js` 都导入 `theme.js` ，则编辑 `theme.js` 将更新两个组件。
- 最后，如果您**编辑由 React 树外部的文件导入的文件**，快速刷新将**回退到执行完全重新加载**。你可能有一个文件，它渲染了一个 React 组件，但也导出了一个由**非 React 组件**导入的值。例如，也许您的组件也导出了一个常量，而非 React 实用程序文件会导入它。在这种情况下，请考虑将常量迁移到单独的文件，并将其导入到这两个文件中。这将重新启用快速刷新以工作。其他情况通常可以用类似的方式解决。

## [Error Resilience 容错能力](https://nextjs.org/docs/architecture/fast-refresh#error-resilience)

### [Syntax Errors 语法错误](https://nextjs.org/docs/architecture/fast-refresh#syntax-errors)

如果在开发过程中出现语法错误，可以修复它并再次保存文件。该错误将自动消失，因此您无需重新加载应用程序。**您不会丢失组件状态**。

### [Runtime Errors 运行时错误](https://nextjs.org/docs/architecture/fast-refresh#runtime-errors)

如果您犯了一个错误，导致组件内部出现运行时错误，您将看到上下文覆盖。修复错误将自动关闭叠加层，而无需重新加载应用程序。

如果在渲染期间未发生错误，则将保留组件状态。如果在渲染过程中确实发生了错误，React 将使用更新的代码重新挂载您的应用程序。

如果应用中有错误边界 [error boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)（对于生产中的正常故障，这是一个好主意），则它们将在渲染错误后的下一次编辑时重试渲染。这意味着具有错误边界可以防止你始终重置为根应用状态。但是，请记住，错误边界不应过于精细。它们被 React 在生产中使用，并且应该始终有意识地设计。

## [Limitations 局限性](https://nextjs.org/docs/architecture/fast-refresh#limitations)

快速刷新会尝试在您正在编辑的组件中保留本地 React 状态，但前提是这样做是安全的。以下是每次编辑文件时可能会看到本地状态被重置的几个原因：

- 不会保留class components的Local state（只有函数组件和 Hook 保留状态）。
- 您正在编辑的文件除了 React 组件之外，可能还有其他导出。
- 有时，文件会导出调用高阶组件（如 `HOC(WrappedComponent)` ）的结果。如果返回的组件是一个class，则其state将被重置。
- 匿名箭头函数（Anonymous arrow functions）（如`export default () => <div />;` ）会导致快速刷新不保留本地组件状态。对于大型代码库，您可以使用我们的  [`name-default-component` codemod](https://nextjs.org/docs/pages/building-your-application/upgrading/codemods#name-default-component).

随着越来越多的代码库转移到function components 和 Hook，您可以期望在更多情况下保留状态。

## [Tips](https://nextjs.org/docs/architecture/fast-refresh#tips)

- 默认情况下，快速刷新在function components （和Hooks）中保留 React 本地状态。
- 有时，您可能希望*强制*重置状态，并重新挂载组件。例如，如果您要调整仅在挂载时发生的动画，这会很方便。为此，您可以在正在编辑的文件中的任何位置添加 `// @refresh reset` 。此指令是文件的本地指令，并指示快速刷新在每次编辑时重新装入该文件中定义的组件。
- 您可以将 `console.log` 或 `debugger;` 放入您在开发期间编辑的组件中。

## [Fast Refresh and Hooks](https://nextjs.org/docs/architecture/fast-refresh#fast-refresh-and-hooks)

When possible, Fast Refresh attempts to preserve the state of your component between edits. In particular, `useState` and `useRef` preserve their previous values as long as you don't change their arguments or the order of the Hook calls.
如果可能，“快速刷新”会尝试在编辑之间保留组件的状态。特别是，只要不更改它们的参数（arguments）或 Hook 调用的顺序，`useState` 和 `useRef` 就会保留其以前的值。

具有依赖项（如 `useEffect` 、 `useMemo` 和 `useCallback` ）的挂钩将始终在快速刷新期间更新。在快速刷新发生时，将忽略它们的依赖项列表。

例如，当您将 `useMemo(() => x * 2, [x])` 编辑为 `useMemo(() => x * 10, [x])` 时，即使 `x` （依赖项）未更改，它也会重新运行。如果 React 没有这样做，你的编辑就不会反映在屏幕上！

有时，这可能会导致意想不到的结果。例如，即使具有空依赖项数组的 `useEffect` 在快速刷新期间**仍**会重新运行一次。

但是，即使没有快速刷新，编写可复原偶尔重新运行 `useEffect` 的代码也是一种很好的做法。这将使您以后更容易引入新的依赖项，并且它由 React 严格模式强制执行，我们强烈建议启用该模式。