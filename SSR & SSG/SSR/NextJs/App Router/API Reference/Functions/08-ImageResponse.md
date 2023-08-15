# ImageResponse

`ImageResponse` 构造函数允许您使用 JSX 和 CSS 生成动态图像。这对于生成社交媒体图像（例如开放图形图像，Twitter卡片等）非常有用。

以下选项可用于 `ImageResponse` ：

```tsx
import { ImageResponse } from 'next/server'
 
new ImageResponse(
  element: ReactElement,
  options: {
    width?: number = 1200
    height?: number = 630
    emoji?: 'twemoji' | 'blobmoji' | 'noto' | 'openmoji' = 'twemoji',
    fonts?: {
      name: string,
      data: ArrayBuffer,
      weight: number,
      style: 'normal' | 'italic'
    }[]
    debug?: boolean = false
 
    // Options that will be passed to the HTTP response
    status?: number = 200
    statusText?: string
    headers?: Record<string, string>
  },
)
```

