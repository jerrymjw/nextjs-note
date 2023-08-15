# Runtime Config

> **Note**: 此功能被视为旧功能，不适用于 [Automatic Static Optimization](https://nextjs.org/docs/pages/building-your-application/rendering/automatic-static-optimization), [Output File Tracing](https://nextjs.org/docs/pages/api-reference/next-config-js/output#automatically-copying-traced-files), or [React Server Components](https://nextjs.org/docs/getting-started/react-essentials#server-components)。请改用环境变量[environment variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables) 以避免初始化开销。

若要将runtime配置添加到应用，请打开 `next.config.js` 并添加 `publicRuntimeConfig` 和 `serverRuntimeConfig` 配置：

```js
module.exports = {
  serverRuntimeConfig: {
    // Will only be available on the server side
    mySecret: 'secret',
    secondSecret: process.env.SECOND_SECRET, // Pass through env variables
  },
  publicRuntimeConfig: {
    // Will be available on both server and client
    staticFolder: '/static',
  },
};
```

Place any server-only runtime config under `serverRuntimeConfig`.
将任何server-only runtime配置置于 `serverRuntimeConfig` 。

Anything accessible to both client and server-side code should be under `publicRuntimeConfig`.
任何可访问的客户端和服务器端代码都应置于 `publicRuntimeConfig` 。

> 依赖于 `publicRuntimeConfig` 的页面**必须**使用 `getInitialProps` 或 `getServerSideProps` ，或者您的应用程序必须具有 `getInitialProps` 的自定义 App 才能选择退出自动静态优化。运行时配置如果不渲染服务器端，则对任何页面（或页面中的组件）都不可用。

要访问应用中的运行时配置，请使用 `next/config` ，如下所示：

```js
import getConfig from 'next/config';
import Image from 'next/image';
 
// Only holds serverRuntimeConfig and publicRuntimeConfig
const { serverRuntimeConfig, publicRuntimeConfig } = getConfig();
// Will only be available on the server-side
console.log(serverRuntimeConfig.mySecret);
// Will be available on both server-side and client-side
console.log(publicRuntimeConfig.staticFolder);
 
function MyImage() {
  return (
    <div>
      <Image
        src={`${publicRuntimeConfig.staticFolder}/logo.png`}
        alt="logo"
        layout="fill"
      />
    </div>
  );
}
 
export default MyImage;
```

