# Font Optimization

[**`next/font`**](https://nextjs.org/docs/app/api-reference/components/font) 将自动优化字体（包括自定义字体）并删除外部网络请求，以提高隐私和性能。

`next/font` 包括任何字体文件的内置自动自托管(**built-in automatic self-hosting** )。这意味着您可以以最佳方式加载布局偏移为零的 Web 字体，这要归功于所使用的底层 CSS `size-adjust` 属性。

这个新的字体系统还允许您方便地使用所有 Google 字体，同时考虑到性能和隐私。CSS 和字体文件在构建时下载，并与其余静态资产一起自托管。**浏览器不会向谷歌发送任何请求**。

## [Google Fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#google-fonts)

自动自托管任何谷歌字体。字体包含在部署中，并从与部署相同的域提供。**浏览器不会向谷歌发送任何请求**。

首先将要使用的字体从 `next/font/google` 导入为函数。我们建议使用[可变字体 variable fonts](https://fonts.google.com/variablefonts) 以获得最佳性能和灵活性。

```tsx
import { Inter } from 'next/font/google';
 
// If loading a variable font, you don't need to specify the font weight
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
});
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```

如果不能使用可变字体，则需要**指定粗细**：

```tsx
import { Roboto } from 'next/font/google';
 
const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
  display: 'swap',
});
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  );
}
```

您可以使用数组指定多个权重和/或样式:

```tsx
const roboto = Roboto({
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  subsets: ['latin'],
  display: 'swap',
});
```

> 温馨提示：对包含多个单词的字体名称使用下划线 （_）。例如 `Roboto Mono` 应导入为 `Roboto_Mono` 。

### [指定子集](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#specifying-a-subset)

谷歌字体会自动[子集 subset](https://fonts.google.com/knowledge/glossary/subsetting)。这将减小字体文件的大小并提高性能。您需要定义要预加载的子集中的哪些。在 `preload` 为 `true` 时未能指定任何子集将导致警告。

这可以通过将它添加到函数调用来完成：

```tsx
const inter = Inter({ subsets: ['latin'] });
```

### [Using Multiple Fonts 使用多种字体](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#using-multiple-fonts)

您可以在应用程序中导入和使用多种字体。您可以采取两种方法。

第一种方法是创建一个实用工具函数，该函数导出字体，导入字体，并在需要时应用其 `className` 。这可确保仅在呈现字体时预加载字体：

```tsx
import { Inter, Roboto_Mono } from 'next/font/google';
 
export const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
});
 
export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
});
```

```tsx
import { inter } from './fonts';
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={inter.className}>
      <body>
        <div>{children}</div>
      </body>
    </html>
  );
}
```

```tsx
import { roboto_mono } from './fonts';
 
export default function Page() {
  return (
    <>
      <h1 className={roboto_mono.className}>My page</h1>
    </>
  );
}
```

在上面的示例中， `Inter` 将全局应用，并且可以根据需要导入和应用 `Roboto Mono` 。

或者，您可以创建一个 CSS 变量并将其与您首选的 CSS 解决方案一起使用：

```tsx
import { Inter, Roboto_Mono } from 'next/font/google';
import styles from './global.css';
 
const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
  display: 'swap',
});
 
const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
});
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>
        <h1>My App</h1>
        <div>{children}</div>
      </body>
    </html>
  );
}
```

```css
html {
  font-family: var(--font-inter);
}
 
h1 {
  font-family: var(--font-roboto-mono);
}
```

在上面的示例中， `Inter` 将全局应用，任何 `<h1>` 标记的样式都将使用 `Roboto Mono` 。

> 建议：保守地使用多种字体，因为每种新字体都是客户端必须下载的额外资源。

## [Local Fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts)

Import `next/font/local` and specify the `src` of your local font file. We recommend using [variable fonts](https://fonts.google.com/variablefonts) for the best performance and flexibility.
导入 `next/font/local` 并指定本地字体文件的 `src` 。我们建议使用[可变字体 variable fonts](https://fonts.google.com/variablefonts) 以获得最佳性能和灵活性。

```tsx
import localFont from 'next/font/local';
 
// Font files can be colocated inside of `app`
const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap',
});
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  );
}
```

如果要对单个字体系列使用多个文件， `src` 可以是数组：

```tsx
const roboto = localFont({
  src: [
    {
      path: './Roboto-Regular.woff2',
      weight: '400',
      style: 'normal',
    },
    {
      path: './Roboto-Italic.woff2',
      weight: '400',
      style: 'italic',
    },
    {
      path: './Roboto-Bold.woff2',
      weight: '700',
      style: 'normal',
    },
    {
      path: './Roboto-BoldItalic.woff2',
      weight: '700',
      style: 'italic',
    },
  ],
});
```

## [With Tailwind CSS](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#with-tailwind-css)

`next/font` 可以通过  [CSS variable](https://nextjs.org/docs/app/api-reference/components/font#css-variables)与 [Tailwind CSS](https://tailwindcss.com/)  一起使用。

在下面的示例中，我们使用 `next/font/google` 中的字体 `Inter` （您可以使用 Google 或本地字体中的任何字体）。使用 `variable` 选项加载字体以定义 CSS 变量名称并将其分配给 `inter` 。然后，使用 `inter.variable` 将 CSS 变量添加到 HTML 文档中。

```tsx
import { Inter, Roboto_Mono } from 'next/font/google';
 
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
});
 
const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-roboto-mono',
});
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

最后，将 CSS 变量添加到您的 Tailwind CSS 配置中：

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-inter)'],
        mono: ['var(--font-roboto-mono)'],
      },
    },
  },
  plugins: [],
};
```

现在可以使用 `font-sans` 和 `font-mono` 实用程序类将字体应用于元素。

## [Preloading](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#preloading)

在网站上的页面上调用字体函数时，该函数并非全局可用，并且不会在所有路由上预加载。相反，字体仅根据使用它的文件类型预加载到相关路由上：

- 如果它是一个唯一的页面，它被预加载到该页面的唯一路由上。
- 如果它是一个布局，它被预加载到布局包装的所有路由上。
- 如果是根布局，则会在所有路由上预加载。

## [Reusing fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#reusing-fonts)

每次调用 `localFont` 或 Google 字体函数时，该字体都会作为应用程序中的一个实例托管。因此，如果在多个文件中加载相同的字体函数，则会托管同一字体的多个实例。在这种情况下，建议执行以下操作：

- 在一个共享文件中调用字体加载器函数
- 将其导出为常量
- 在要使用此字体的每个文件中导入常量

## 总结

- 字体分为谷歌字体和本地字体
- [**`next/font`**](https://nextjs.org/docs/app/api-reference/components/font) 自带谷歌字体，默认使用可变字体
- 最好能用可变字体，如果不能就需要指定粗细
- 数组指定多个权重和/或样式
- 谷歌字体会自动[子集 subset](https://fonts.google.com/knowledge/glossary/subsetting)
- 可以导入和使用多种字体。有两种方法：1.创建一个实用工具函数，该函数导出字体，导入字体，并在需要时应用其 `className` 。2.创建一个 CSS 变量并将其与您首选的 CSS 解决方案一起使用
- 导入 `next/font/local` 并指定本地字体文件的 `src`
- 如果使用tailwind，可以借用css variable的特性来控制字体
- 在网站上的页面上调用字体函数时，该函数并非全局可用，并且不会在所有路由上预加载。相反，字体仅根据使用它的文件类型预加载到相关路由上：

  - 如果它是一个唯一的页面，它被预加载到该页面的唯一路由上。
  - 如果它是一个布局，它被预加载到布局包装的所有路由上。
  - 如果是根布局，则会在所有路由上预加载。

- 如果在多个文件中加载相同的字体函数，则会托管同一字体的多个实例。在这种情况下，建议执行以下操作：

  - 在一个共享文件中调用字体加载器函数
  - 将其导出为常量
  - 在要使用此字体的每个文件中导入常量