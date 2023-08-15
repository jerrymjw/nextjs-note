# Static Assets

Next.js可以在根目录中名为 `public` 的文件夹下提供静态文件，如图像。然后，代码可以从 base URL (`/`). 开始引用 `public` 中的文件。

例如，如果在 `public` 中添加 `me.png` ，则以下代码将访问图像：

```tsx
import Image from 'next/image';
 
function Avatar() {
  return <Image src="/me.png" alt="me" width="64" height="64" />;
}
 
export default Avatar;
```

此文件夹对于 `robots.txt` 、 `favicon.ico` 、Google Site Verification和任何其他静态文件（包括 `.html` ）也很有用！

- 确保目录名为 `public` 。该名称无法更改，并且是唯一用于提供静态资产的目录。
- 请确保不要使用与 `pages/` 目录中的文件同名的静态文件，因为这会导致错误。
- 只有构建时位于 `public` 目录中的资产才会由 Next.js 服务。在运行时添加的文件将不可用。我们建议使用 AWS S3 等第三方服务进行持久文件存储。