## [Server Components](https://nextjs.org/docs/getting-started/react-essentials#server-components)

### 为什么选择服务器组件？

服务器组件使开发人员能够更好地利用服务器基础结构。使用服务器组件，初始页面加载速度更快，仅当客户端交互性通过客户端组件在应用程序中使用时，才会添加其他 JavaScript。使用 Next.js 加载路由时，初始 HTML 将在服务器上呈现。然后，此 HTML 在浏览器中逐步增强，允许客户端通过异步加载 Next.js 和 React 客户端运行时来接管应用程序并添加交互性。为了更轻松地过渡到服务器组件，默认情况下，App Router 中的所有组件都是服务器组件，包括特殊文件和共置组件。这使您可以自动采用它们，而无需额外的工作，并实现开箱即用的出色性能。您还可以选择使用 ['use client' ](https://nextjs.org/docs/getting-started/react-essentials#using-the-use-client-directive)指令加入客户端组件。

## [Client Components](https://nextjs.org/docs/getting-started/react-essentials#client-components)

### [`"use client"` 指令](https://github.com/reactjs/rfcs/pull/227)

`"use client"` 可以使用client端操作

```tsx
'use client';
 
import { useState } from 'react';
 
export default function Counter() {
  const [count, setCount] = useState(0);
 
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

![Use Client Directive and Network Boundary](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fuse-client-directive.png&w=3840&q=75)

`"use client"` 位于仅服务器代码和客户端代码之间。它位于文件顶部的导入上方。在文件中定义 `"use client"` 后，导入到其中的所有其他模块（包括子组件也就是**subtree**）都被视为客户端捆绑包的一部分。

注：可以在整个文件中使用`"use client"` 也可以只在某个**function**里使用，这样的话就仅仅是在**function**里使用了客户端。

**Good to know: 很高兴知道：**

- 服务器组件模块（“**use server**”）图中的组件保证仅在服务器上渲染。
- 客户端组件模块（“**use client**”）图中的组件主要在客户端上呈现，但使用 Next.js，它们也可以在服务器上预呈现并在客户端上冻结(**hydrate,又称为水合**)。
- 在进行任何导入之前，必须在**文件顶部**定义 `"use client"` 指令。
- `"use client"` 不需要在每个文件中定义。客户端模块边界只需在“入口点”（也就是**top level**）定义一次，即可将导入其中的所有模块视为客户端组件。

## [何时使用服务器和客户端组件？](https://nextjs.org/docs/getting-started/react-essentials#when-to-use-server-and-client-components)

为了简化服务器组件和客户端组件之间的决策，我们建议使用服务器组件（ `App Router directory` 中的默认值），直到您有客户端组件的用例。

下表总结了服务器和客户端组件的不同用例：

![image-20230518215538003](/Users/jerry/Library/Application Support/typora-user-images/image-20230518215538003.png)

## [Patterns](https://nextjs.org/docs/getting-started/react-essentials#patterns)

### [Moving Client Components to the Leaves 将客户端组件移动到叶子](https://nextjs.org/docs/getting-started/react-essentials#moving-client-components-to-the-leaves)

为了提高应用程序的性能，我们建议尽可能将客户端组件移动到组件树的叶子位置。例如，您可能有一个包含静态元素（例如**Logo**、**Link**等）的布局和一个使用状态的交互式搜索栏。

**不要使整个布局成为客户端组件**，而是将**交互式逻辑**移动到**客户端组件**（例如 `<SearchBar />` ） 并将**布局**保留为**服务器组件**。这意味着您不必将布局的所有组件 Javascript 发送到客户端。

```ts
// SearchBar is a Client Component
import SearchBar from './searchbar';
// Logo is a Server Component
import Logo from './logo';
 
// Layout is a Server Component by default
export default function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <>
      <nav>
        <Logo />
        <SearchBar />
      </nav>
      <main>{children}</main>
    </>
  );
}
```

### [组成客户端和服务器组件](https://nextjs.org/docs/getting-started/react-essentials#composing-client-and-server-components)

服务端和客户端组件可以组合在同一组件树中。

在幕后，React 处理渲染的方式如下：

- 在**服务端**上，React 在将结果发送到客户端之前渲染所有服务器组件。
  - 
    这包括嵌套在客户端组件中的服务端组件。
  - 
    将跳过在此阶段遇到的客户端组件。
- 在**客户端**上，React 在服务端组件的渲染结果中渲染客户端组件和插槽，合并在服务器和客户端上完成的工作。
  - 如果任何服务器组件嵌套在客户端组件中，则其呈现的内容将正确放置在客户端组件中。

> **温馨提示**：在 Next.js 中，在初始页面加载期间，上述步骤中的服务器组件和客户端组件的呈现结果都将作为 HTML 在服务器上预呈现，以生成更快的初始页面加载。

**注：这个意思就是优先渲染服务端，如果是客户端中有服务端组件部分那么优先渲染服务端组件，而客户端组件在这时先被跳过**

#### [在客户端组件中嵌套服务器组件](https://nextjs.org/docs/getting-started/react-essentials#nesting-server-components-inside-client-components)

鉴于上面概述的渲染流程，将服务器组件导入客户端组件存在限制，因为此方法需要额外的服务器往返（因为客户端中遇到服务端组件要渲染而客户端组件则先被跳过）。

##### [ 不支持的模式](https://nextjs.org/docs/getting-started/react-essentials#unsupported-pattern)

不支持以下模式。不能将服务器组件导入到客户端组件中：

```ts
'use client';
 
// This pattern will **not** work!
// You cannot import a Server Component into a Client Component.
import ExampleServerComponent from './example-server-component';
 
export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode;
}) {
  const [count, setCount] = useState(0);
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
      <ExampleServerComponent />
    </>
  );
}
```

##### [推荐模式：将服务器组件作为 prop 传递给客户端组件](https://nextjs.org/docs/getting-started/react-essentials#recommended-pattern-passing-server-components-to-client-components-as-props)

相反，在设计客户端组件时，您可以使用 React props 来标记服务器组件的“**洞**”。

服务器组件将在服务器上渲染，当客户端组件在客户端上渲染时，“**洞**”将被服务器组件的渲染结果填充。
一种常见的模式是使用 React `children` 道具来创建“**洞**”。我们可以重构 `<ExampleClientComponent>` 以接受通用 `children` prop，并将 `<ExampleClientComponent>` 的导入和显式嵌套移动到父组件。

```tsx
'use client';
 
import { useState } from 'react';
 
export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode;
}) {
  const [count, setCount] = useState(0);
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
      {children}
    </>
  );
}
```

现在， `<ExampleClientComponent>` 不知道 `children` 是什么。事实上，从它的角度来看，它甚至不知道 `children` 最终将由服务器组件的结果填充。

`ExampleClientComponent` 的唯一责任是决定最终应该放置任何 `children` 。

在父服务器组件中，可以导入 `<ExampleClientComponent>` 和 `<ExampleServerComponent>` ，并将 `<ExampleServerComponent>` 作为 `<ExampleClientComponent>` 的子项传递：

```tsx
// This pattern works:
// You can pass a Server Component as a child or prop of a
// Client Component.
import ExampleClientComponent from './example-client-component';
import ExampleServerComponent from './example-server-component';
 
// Pages in Next.js are Server Components by default
export default function Page() {
  return (
    <ExampleClientComponent>
      <ExampleServerComponent />
    </ExampleClientComponent>
  );
}
```

使用此方法， `<ExampleClientComponent>` 和 `<ExampleServerComponent>` 的呈现是分离的，并且可以独立渲染 - 与服务器组件对齐，服务器组件在客户端组件之前在服务器上渲染。

> **Good to know 很高兴知道**
>
> - 此模式已应用于具有 `children` 属性的布局和页面中，因此您不必创建额外的包装器组件。
> - 将 React 组件 （JSX） 传递给其他组件并不是一个新概念，并且一直是 React 组合模型的一部分。
> - 这种组合策略适用于服务器和客户端组件，因为接收 prop 的组件不知道 prop 是什么。它只负责它被传递的东西应该放在哪里。
>   - 这允许在客户端上呈现客户端组件之前，在服务器上独立呈现传递的 prop，在本例中，在服务器上渲染。
>   - “提升内容”策略的相同策略已用于避免父组件中的状态更改重新渲染导入的嵌套子组件。
> - 您不要被 `children` prop限制，你可以使用任何prop来传递 JSX。

### [将 props 从服务器传递到客户端组件（序列化）](https://nextjs.org/docs/getting-started/react-essentials#passing-props-from-server-to-client-components-serialization)

从服务器传递到客户端组件的 props 需要可序列化。这意味着函数、日期等值不能直接传递给客户端组件。

> **Where is the Network Boundary? 网络边界在哪里？**
> 在App Router中，网络边界位于服务器组件和客户端组件之间。这与 `getStaticProps` / `getServerSideProps` 和页面组件之间的边界的页面不同。在服务器组件中获取的数据不需要序列化，因为它不会跨越网络边界，除非将其传递给客户端组件。

### [将仅服务器代码排除在客户端组件之外（Poisoning）](https://nextjs.org/docs/getting-started/react-essentials#keeping-server-only-code-out-of-client-components-poisoning)

由于 JavaScript 模块可以在服务器和客户端组件之间共享，因此只打算在服务器上运行的代码可能会潜入客户端。

例如，采用以下数据获取函数：

```tsx
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  });
 
  return res.json();
}
```

乍一看， `getData` 似乎在服务器和客户端上都有效。但是由于环境变量 `API_KEY` 没有前缀 `NEXT_PUBLIC` ，它是一个只能在服务器上访问的私有变量。接下来.js在客户端代码中将私有环境变量替换为空字符串，以防止安全信息泄露。

As a result, even though `getData()` can be imported and executed on the client, it won't work as expected. And while making the variable public would make the function work on the client, it would leak sensitive information.
因此，即使可以在客户端上导入和执行 `getData()` ，它也不会按预期工作。虽然将变量设为公共将使函数在客户端上工作，但它会泄漏敏感信息。

So, this function was written with the intention that it would only ever be executed on the server.
因此，编写此函数的目的是使其仅在服务器上执行。

#### [The "server only" package “仅服务器”软件包](https://nextjs.org/docs/getting-started/react-essentials#the-server-only-package)

为了防止这种意外的客户端使用服务器代码，我们可以使用 `server-only` 包给其他开发人员一个build-time error，如果他们不小心将这些模块之一导入客户端组件。

要使用 `server-only` ，请先安装软件包：

```terminal
npm install server-only
```

然后将包导入到包含server-only代码的任何模块中：

```tsx
import 'server-only';
 
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  });
 
  return res.json();
}
```

现在，导入 `getData()` 的任何客户端组件都将收到生成时错误，说明此模块只能在服务器上使用。

相应的包 `client-only` 可用于标记包含仅客户端代码的模块，例如，访问 `window` 对象的代码。

### [Data Fetching](https://nextjs.org/docs/getting-started/react-essentials#data-fetching)

尽管可以在客户端组件中提取数据，但我们建议在**服务器组件中提取数据**，除非您有**特定原因**(如**需要交互**)在客户端上提取数据。将数据提取移动到服务器可以带来更好的性能和用户体验。

[Learn more about data fetching](https://nextjs.org/docs/app/building-your-application/data-fetching).
了解有关数据获取的更多信息。

### [Third-party packages 第三方软件包](https://nextjs.org/docs/getting-started/react-essentials#third-party-packages)

由于服务器组件是新的，生态系统中的第三方软件包刚刚开始将 `"use client"` 指令添加到使用仅限客户端的功能（如 `useState` 、 `useEffect` 和 `createContext` ）的组件中。

目前，使用 客户端专用功能的 `npm` 包中的许多组件还没有该指令。这些第三方组件**将在您自己的客户端组件中按预期工作**，因为它们具有 `"use client"` 指令，但它们**不会在服务器组件中工作**。

例如，假设您安装了具有 `<AcmeCarousel />` 组件的假设 `acme-carousel` 包。此组件使用 `useState` ，但它还没有 `"use client"` 指令。

If you use `<AcmeCarousel />` within a Client Component, it will work as expected:
如果在客户端组件中使用 `<AcmeCarousel />` ，它将按预期工作：

```tsx
'use client';
 
import { useState } from 'react';
import { AcmeCarousel } from 'acme-carousel';
 
export default function Gallery() {
  let [isOpen, setIsOpen] = useState(false);
 
  return (
    <div>
      <button onClick={() => setIsOpen(true)}>View pictures</button>
 
      {/* 🟢 Works, since AcmeCarousel is used within a Client Component */}
      {isOpen && <AcmeCarousel />}
    </div>
  );
}
```

但是，如果您尝试直接在服务器组件中使用它，您将看到一个错误：

```tsx
import { AcmeCarousel } from 'acme-carousel';
 
export default function Page() {
  return (
    <div>
      <p>View pictures</p>
 
      {/* 🔴 Error: `useState` can not be used within Server Components */}
      <AcmeCarousel />
    </div>
  );
}
```

这是因为 Next.js 不知道 `<AcmeCarousel />` 正在使用仅限客户端(**client-only**)的功能。

要解决此问题，您可以将依赖于客户端专用(**client-only**)功能的第三方组件包装在您自己的客户端组件中：

```tsx
'use client';
 
import { AcmeCarousel } from 'acme-carousel';
 
export default AcmeCarousel;
```

现在，可以直接在服务器组件中使用 `<Carousel />` ：

```tsx
import Carousel from './carousel';
 
export default function Page() {
  return (
    <div>
      <p>View pictures</p>
 
      {/*  Works, since Carousel is a Client Component */}
      <Carousel />
    </div>
  );
}
```

我们**不希望您需要包装大多数第三方组件**，因为您可能会在客户端组件中使用它们。但是，一个例外是提供程序组件，因为它们依赖于 React 状态和上下文，并且通常在应用程序的根目录中需要。 在下面了解有关第三方上下文提供程序的更多信息。

#### [Library Authors](https://nextjs.org/docs/getting-started/react-essentials#library-authors)

- 以类似的方式，创建供其他开发人员使用的包的库作者可以使用 `"use client"` 指令来标记其包的客户端入口点。这允许包的用户将包组件直接导入到其服务器组件中，而无需创建包装边界。
- 您可以通过在树中更深入地使用“use client”来优化软件包，允许导入的模块成为服务器组件模块图的一部分。
- 值得注意的是，一些捆绑器可能会删除 `"use client"` 指令。您可以找到有关如何配置 esbuild 以在 React Wrap Balancer 和 Vercel Analytics 存储库中包含 `"use client"` 指令的示例。

## [Context](https://nextjs.org/docs/getting-started/react-essentials#context)

大多数 React 应用程序依靠上下文在组件之间共享数据，无论是直接通过 `createContext` 还是间接通过从第三方库导入的提供程序组件。

在 Next.js 13 中，客户端组件完全支持上下文，但不能直接在服务器组件中创建或使用上下文。这是因为**服务器组件没有 React 状态**（因为它们**不是交互式的**），并且上下文主要用于在某些 React 状态更新后重新渲染树中的交互式组件（**因为需要交互，所以需要使用use client**）。

我们将讨论在**服务器组件**之间**共享数据**的**替代方法**，但首先，让我们看一下如何在客户端组件中使用上下文。

### [在客户端组件中使用上下文](https://nextjs.org/docs/getting-started/react-essentials#using-context-in-client-components)

客户端组件完全支持所有上下文 API：

```tsx
'use client';
 
import { createContext, useContext, useState } from 'react';
 
const SidebarContext = createContext();
 
export function Sidebar() {
  const [isOpen, setIsOpen] = useState();
 
  return (
    <SidebarContext.Provider value={{ isOpen }}>
      <SidebarNav />
    </SidebarContext.Provider>
  );
}
 
function SidebarNav() {
  let { isOpen } = useContext(SidebarContext);
 
  return (
    <div>
      <p>Home</p>
 
      {isOpen && <Subnav />}
    </div>
  );
}
```

但是，上下文**provider**程序通常渲染在App的根目录附近，以共享全局关注点，例如当前主题。由于服务器组件不支持上下文，因此尝试在App的根目录下创建上下文将导致error：

```tsx
import { createContext } from 'react';
 
//  createContext is not supported in Server Components
export const ThemeContext = createContext({});
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
      </body>
    </html>
  );
}
```

若要解决此问题，请创建上下文并在客户端组件中**渲染provider**：

```tsx
'use client';
 
import { createContext } from 'react';
 
export const ThemeContext = createContext({});
 
export default function ThemeProvider({ children }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>;
}
```

服务器组件现在能够直接**渲染provider**，因为它已被标记为客户端组件：

```tsx
import ThemeProvider from './theme-provider';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

在根目录**渲染provider**后，整个应用中的所有其他客户端组件将能够使用此上下文。

> 注意：您应该在树中尽可能深入地呈现提供程序 - 请注意 `ThemeProvider` 如何仅包装 `{children}` 而不是整个 `<html>` 文档。这使 Next.js 更容易优化服务器组件的静态部分。

### [在服务器组件中呈现第三方上下文提供程序](https://nextjs.org/docs/getting-started/react-essentials#rendering-third-party-context-providers-in-server-components)

第三方 npm 包通常包含需要在应用程序根目录附近**渲染provider**。如果这些提供程序包含 `"use client"` 指令，则可以直接在服务器组件中渲染它们。但是，由于服务器组件是如此之新，因此许多第三方提供商**尚未添加该指令**。

如果尝试呈现没有 `"use client"` 的第三方提供程序，则会导致error：

```tsx
import { ThemeProvider } from 'acme-theme';
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {/*  Error: `createContext` can't be used in Server Components */}
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

要解决此问题，请将第三方提供程序包装在您自己的客户端组件中：

```tsx
'use client';
 
import { ThemeProvider } from 'acme-theme';
import { AuthProvider } from 'acme-auth';
 
export function Providers({ children }) {
  return (
    <ThemeProvider>
      <AuthProvider>{children}</AuthProvider>
    </ThemeProvider>
  );
}
```

现在，可以直接在**根布局**中**导入**和**渲染** `<Providers />` 。

```tsx
import { Providers } from './providers';
 
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

在根目录中**渲染provider**后，这些库中的所有组件和hook都将在您自己的客户端组件中按预期工作。

第三方库将 `"use client"` 添加到其客户端代码后，你将能够删除包装器客户端组件。

### [在服务器组件之间共享数据](https://nextjs.org/docs/getting-started/react-essentials#sharing-data-between-server-components)

由于**服务器组件不是交互式**的，因此**不会从 React 状态读取**，因此您**不需要上下文的全部功能来共享数据**。如果您有多个服务器组件需要访问的通用数据，则可以在模块范围内使用本机 JavaScript 模式（如全局单例）。

例如，模块可用于在多个组件之间共享数据库连接：

```tsx
export const db = new DatabaseConnection();
```

```tsx
import { db } from '@utils/database';
 
export async function UsersLayout() {
  let users = await db.query();
  // ...
}
```

在上面的示例中，布局和页面都需要进行数据库查询。其中每个组件都通过导入 `@utils/database` 模块来共享对数据库的访问权限。(**不通过react context而是直接进入database查看**)

### [在服务器组件之间共享获取请求](https://nextjs.org/docs/getting-started/react-essentials#sharing-fetch-requests-between-server-components)

获取数据时，您可能希望在 `page` 或 `layout` 及其某些子组件之间共享 `fetch` 的结果。这是组件之间**不必要的耦合**，可能导致在组件之间来回传递 `props` 。

相反，我们建议将**数据提取（Data Fetching）与使用数据的组件放在一起**。 `fetch` 请求在服务器组件中自动删除重复数据(**当有相同的数据被再次fetch时，不会fetch而是从缓存中提取，因为是自动缓存的**)，因此每个路由段（Route Segment）都可以**准确请求所需的数据**，而无**需担心重复请求**。Next.js将从 `fetch` **缓存中**读取**相同的值**。
