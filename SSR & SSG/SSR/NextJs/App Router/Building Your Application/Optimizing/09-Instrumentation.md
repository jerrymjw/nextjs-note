# Instrumentation

> **Note**: 此功能是实验性的。要使用它，您必须通过在 `next.config.js` 中定义 `experimental.instrumentationHook = true;` 来明确选择加入。

如果从此文件中导出名为 `register` 的函数，则每当引导新的 Next.js 服务器实例时，我们都会调用该函数。部署 `register` 函数时，将在每次冷启动时调用它（但在每个环境中恰好调用一次）。

有时，在代码中导入文件可能很有用，因为它会导致副作用。例如，可以导入定义一组全局变量的文件，但从不在代码中显式使用导入的文件。您仍然可以访问包声明的全局变量。

您可以导入在 `instrumentation.ts` 中具有副作用的文件，您可能希望在 `register` 函数中使用这些文件，如以下示例所示：

```tsx
import { init } from 'package-init';
 
export function register() {
  init();
}
```

但是，我们建议从 `register` 函数中使用 `import` 导入有副作用的文件。以下示例演示了 `register` 函数中 `import` 的基本用法：

```tsx
export async function register() {
  await import('package-with-side-effect');
}
```

通过执行此操作，可以将所有副作用并置到代码中的一个位置，并避免导入文件的任何意外后果。

我们在所有环境中都调用 `register` ，因此有必要有条件地导入任何不支持 `edge` 和 `nodejs` 的代码。您可以使用环境变量 `NEXT_RUNTIME` 获取当前环境。导入特定于环境的代码如下所示：

```ts
export async function register() {
  if (process.env.NEXT_RUNTIME === 'nodejs') {
    await import('./instrumentation-node');
  }
 
  if (process.env.NEXT_RUNTIME === 'edge') {
    await import('./instrumentation-edge');
  }
}
```

