# Font Module

This API reference will help you understand how to use [`next/font/google`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#google-fonts) and [`next/font/local`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts). For features and usage, please see the [Optimizing Fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) page.
此 API 参考将帮助您了解如何使用 [`next/font/google`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#google-fonts) 和[`next/font/local`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts)。

### [Font Function Arguments 字体函数参数](https://nextjs.org/docs/app/api-reference/components/font#font-function-arguments)

![image-20230525153818340](/Users/jerry/Library/Application Support/typora-user-images/image-20230525153818340.png)

### [`src`](https://nextjs.org/docs/app/api-reference/components/font#src)

字体文件作为字符串或对象数组（类型为 `Array<{path: string, weight?: string, style?: string}>` ）相对于调用字体加载器函数的目录的路径。

用于 `next/font/local`

- Required

Examples:

- `src:'./fonts/my-font.woff2'` ，其中 `my-font.woff2` 放置在 `app` 目录中名为 `fonts` 的目录中
- `src:[{path: './inter/Inter-Thin.ttf', weight: '100',},{path: './inter/Inter-Regular.ttf',weight: '400',},{path: './inter/Inter-Bold-Italic.ttf', weight: '700',style: 'italic',},]`
- 如果使用 `src:'../styles/fonts/my-font.ttf'` 在 `app/page.tsx` 中调用字体加载器函数，则 `my-font.ttf` 放置在项目根目录的 `styles/fonts` 中

### [`weight`](https://nextjs.org/docs/app/api-reference/components/font#weight)

字体 [`weight`](https://fonts.google.com/knowledge/glossary/weight) 具有以下可能性：

- 一个字符串，其中包含特定字体可用的宽度的可能值，如果是[可变字体variable](https://fonts.google.com/variablefonts) ，则包含值范围
- 如果字体不是[可变谷歌字体 variable google font](https://fonts.google.com/variablefonts). ，则为权重值数组。它仅适用于 `next/font/google` 。

在 `next/font/google` 和 `next/font/local` 中使用

- 如果使用的字体不是可变的，则为必需

Examples:

- `weight: '400'` ：单个宽度值的字符串 - 对于字体 `Inter` ，可能的值为 `'100'` 、 `'200'` 、 `'300'` 、 `'400'` 、 `'500'` 、 `'600'` 、 `'700'` 、 `'800'` 、 `'900'` 或 `'variable'` ，其中 `'variable'` 是默认值）
- `weight: '100 900'` ：变量字体的 `100` 和 `900` 之间的范围的字符串
- `weight: ['100','400','900']` ：非可变字体的 3 个可能值的数组

### [`style`](https://nextjs.org/docs/app/api-reference/components/font#style)

字体 [`style`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style) 具有以下可能性：

- 默认值为 `'normal'` 的字符串值
- An array of style values if the font is not a [variable google font](https://fonts.google.com/variablefonts). It applies to `next/font/google` only.
  样式值数组（如果字体不是可变[谷歌字体variable google font](https://fonts.google.com/variablefonts).）。它仅适用于 `next/font/google` 。

在 `next/font/google` 和 `next/font/local` 中使用

- Optional

Examples:

- `style: 'italic'` ： 字符串 - 它可以是 `normal` 或 `italic` 表示 `next/font/google`
- `style: 'oblique'`: A string - it can take any value for `next/font/local` but is expected to come from [standard font styles](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)
  `style: 'oblique'` ：一个字符串 - 它可以接受 `next/font/local` 的任何值，但应来自 [标准字体样式standard font styles](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)
- `style: ['italic','normal']` ： `next/font/google` 的 2 个值数组 - 这些值来自 `normal` 和 `italic`

### [`subsets`](https://nextjs.org/docs/app/api-reference/components/font#subsets)

字体 [`subsets`](https://fonts.google.com/knowledge/glossary/subsetting) 由字符串值数组定义，其中包含要[预加载preloaded](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#specifying-a-subset)的每个子集的名称。当 {{2}{1}} 选项为 true 时，通过 {} 指定的字体将在头部注入链接预加载标记，这是默认值。

用于 `next/font/google`

- Optional

Examples:

- `subsets: ['latin']` ： 具有子集 `latin` 的数组

您可以在 Google 字体页面上找到您的字体的所有子集的列表。

### [`axes`](https://nextjs.org/docs/app/api-reference/components/font#axes)

某些可变字体具有可以包含的额外 `axes` 。默认情况下，仅包含字体粗细以减小文件大小。 `axes` 的可能值取决于特定字体。

Used in `next/font/google` 用于 `next/font/google`

- Optional

Examples:

- `axes: ['slnt']` ：一个值为 `slnt` 的数组，用于 `Inter` 变量字体，其中 `slnt` 作为附加的 `axes` ，如下所示。您可以使用  [Google variable fonts page](https://fonts.google.com/variablefonts#font-families) 的过滤器并查找 `wght` 以外的轴，找到字体的可能 `axes` 值

### [`display`](https://nextjs.org/docs/app/api-reference/components/font#display)

The font [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display) with possible string [values](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display#values) of `'auto'`, `'block'`, `'swap'`, `'fallback'` or `'optional'` with default value of `'swap'`.
字体 `display` ，可能的字符串值为 `'auto'` 、 `'block'` 、 `'swap'` 、 `'fallback'` 或 `'optional'` ，默认值为 `'swap'` 。

在 `next/font/google` 和 `next/font/local` 中使用

- Optional

Examples:

- `display: 'optional'` ：分配给 `optional` 值的字符串

### [`preload`](https://nextjs.org/docs/app/api-reference/components/font#preload)

一个布尔值，它指定是否应预加载字体。默认值为 `true` 。

在 `next/font/google` 和 `next/font/local` 中使用

- Optional

Examples:

- `preload: false`

### [`fallback`](https://nextjs.org/docs/app/api-reference/components/font#fallback)

无法加载字体时使用的回退字体。没有默认值的回退字体字符串数组。

- Optional

在 `next/font/google` 和 `next/font/local` 中使用

Examples:

- `fallback: ['system-ui', 'arial']` ：将回退字体设置为 `system-ui` 或 `arial` 的数组

### [`adjustFontFallback`](https://nextjs.org/docs/app/api-reference/components/font#adjustfontfallback)

- 对于 `next/font/google` ：一个布尔值，用于设置是否应使用自动回退字体来减少累积布局偏移。默认值为 `true` 。
- `'Times New Roman'` or `false`. The default is `'Arial'`.
  对于 `next/font/local` ：一个字符串或布尔值 `false` 值，用于设置是否应使用自动回退字体来减少累积布局偏移。可能的值为 `'Arial'` 、 `'Times New Roman'` 或 `false` 。默认值为 `'Arial'` 。

在 `next/font/google` 和 `next/font/local` 中使用

- Optional

Examples:

- `adjustFontFallback: false`: for ``next/font/google` 
- `adjustFontFallback: 'Times New Roman'`: for `next/font/local`

### [`variable`](https://nextjs.org/docs/app/api-reference/components/font#variable)

一个字符串值，用于定义在使用 CSS 变量方法应用样式时要使用的  [CSS variable method](https://nextjs.org/docs/app/api-reference/components/font#css-variables)。

在 `next/font/google` 和 `next/font/local` 中使用

- Optional

Examples:

- `variable: '--my-font'` ：声明 CSS 变量 `--my-font`

### [`declarations`](https://nextjs.org/docs/app/api-reference/components/font#declarations)

字体描述符键值对数组，用于进一步定义生成的 `@font-face` 。

用于 `next/font/local`

- Optional

Examples:

- `declarations: [{ prop: 'ascent-override', value: '90%' }]`

## [Applying Styles](https://nextjs.org/docs/app/api-reference/components/font#applying-styles)

您可以通过三种方式应用字体样式：

- [`className`](https://nextjs.org/docs/app/api-reference/components/font#classname)
- [`style`](https://nextjs.org/docs/app/api-reference/components/font#style-1)
- [CSS Variables](https://nextjs.org/docs/app/api-reference/components/font#css-variables)

### [`className`](https://nextjs.org/docs/app/api-reference/components/font#classname)

Returns a read-only CSS `className` for the loaded font to be passed to an HTML element.
返回要传递给 HTML 元素的加载字体的只读 CSS `className` 。

```html
<p className={inter.className}>Hello, Next.js!</p>
```

### [`style`](https://nextjs.org/docs/app/api-reference/components/font#style-1)

返回要传递给 HTML 元素的加载字体的只读 CSS `style` 对象，包括用于访问字体系列名称和回退字体的 `style.fontFamily` 。

```html
<p style={inter.style}>Hello World</p>
```

### [CSS Variables](https://nextjs.org/docs/app/api-reference/components/font#css-variables)

如果要在外部样式表中设置样式并在其中指定其他选项，请使用 CSS 变量方法。

除了导入字体外，还要导入定义 CSS 变量的 CSS 文件，并设置字体加载器对象的变量选项，如下所示：

```tsx
import { Inter } from 'next/font/google';
import styles from '../styles/component.module.css';
 
const inter = Inter({
  variable: '--font-inter',
});
```

若要使用该字体，请将要设置样式的文本的父容器的 `className` 设置为字体加载器的 `variable` 值，并将文本的 `className` 设置为外部 CSS 文件中的 `styles` 属性。

```tsx
<main className={inter.variable}>
  <p className={styles.text}>Hello World</p>
</main>
```

在 `component.module.css` CSS 文件中定义 `text` 选择器类，如下所示:

```css
.text {
  font-family: var(--font-inter);
  font-weight: 200;
  font-style: italic;
}
```

在上面的示例中，文本 `Hello World` 使用 `Inter` 字体设置样式，生成的字体回退使用 `font-weight: 200` 和 `font-style: italic` 设置样式。

## [使用字体定义文件](https://nextjs.org/docs/app/api-reference/components/font#using-a-font-definitions-file)

每次调用 `localFont` 或 Google 字体函数时，该字体都将作为应用程序中的一个实例托管。因此，如果需要在多个位置使用相同的字体，则应将其加载到一个位置，并在需要的位置导入相关的字体对象。这是使用字体定义文件完成的。

例如，在应用目录根目录的 `styles` 文件夹中创建一个 `fonts.ts` 文件。

然后，指定字体定义，如下所示：

```tsx
import { Inter, Lora, Source_Sans_Pro } from 'next/font/google';
import localFont from 'next/font/local';
 
// define your variable fonts
const inter = Inter();
const lora = Lora();
// define 2 weights of a non-variable font
const sourceCodePro400 = Source_Sans_Pro({ weight: '400' });
const sourceCodePro700 = Source_Sans_Pro({ weight: '700' });
// define a custom local font where GreatVibes-Regular.ttf is stored in the styles folder
const greatVibes = localFont({ src: './GreatVibes-Regular.ttf' });
 
export { inter, lora, sourceCodePro400, sourceCodePro700, greatVibes };
```

现在可以在代码中使用这些定义，如下所示：

```tsx
import { inter, lora, sourceCodePro700, greatVibes } from '../styles/fonts';
 
export default function Page() {
  return (
    <div>
      <p className={inter.className}>Hello world using Inter font</p>
      <p style={lora.style}>Hello world using Lora font</p>
      <p className={sourceCodePro700.className}>
        Hello world using Source_Sans_Pro font with weight 700
      </p>
      <p className={greatVibes.className}>My title in Great Vibes font</p>
    </div>
  );
}
```

为了更轻松地访问代码中的字体定义，可以在 `tsconfig.json` 或 `jsconfig.json` 文件中定义路径别名，如下所示：

```json
{
  "compilerOptions": {
    "paths": {
      "@/fonts": ["./styles/fonts"]
    }
  }
}
```

您现在可以导入任何字体定义，如下所示：

```tsx
import { greatVibes, sourceCodePro400 } from '@/fonts';
```

## 总结

- 字体参数分为src, weight, style, subsets, axes, display, preload, fallback, adjustFontFallback, variable, declarations
- 有三种方式可以使用字体样式：
  - 在className中使用
  - 在style中使用
  - 在css variables中使用，要设置样式的文本的父容器的 `className` 设置为字体加载器的 `variable` 值，并将文本的 `className` 设置为外部 CSS 文件中的 `styles` 属性。

- 使用自定义的样式，也可以把样式路径放在tsconfig中