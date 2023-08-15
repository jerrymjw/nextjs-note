# draftMode

`draftMode` 函数允许您检测服务器组件内的草稿模式。

```js
import { draftMode } from 'next/headers';
 
export default function Page() {
  const { isEnabled } = draftMode();
  return (
    <main>
      <h1>My Blog Post</h1>
      <p>Draft Mode is currently {isEnabled ? 'Enabled' : 'Disabled'}</p>
    </main>
  );
}
```

