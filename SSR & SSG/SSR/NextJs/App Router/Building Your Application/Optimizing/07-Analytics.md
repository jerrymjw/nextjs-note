# Analytics

Next.jsSpeed Insights（速度洞察）允许您使用不同的指标分析和衡量页面的性能。

您可以在 Vercel 部署[Vercel deployments](https://vercel.com/docs/concepts/speed-insights?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)上以零配置开始收集您的真实体验分数 [Real Experience Score](https://vercel.com/docs/concepts/speed-insights#core-web-vitals-explained?utm_source=next-site&utm_medium=docs&utm_campaign=next-website)。

本文档的其余部分介绍了 Next.js Speed Insights 使用的内置中继器。

## [Web Vitals](https://nextjs.org/docs/app/building-your-application/optimizing/analytics#web-vitals)

[Web Vitals](https://web.dev/vitals/) are a set of useful metrics that aim to capture the user experience of a web page. The following web vitals are all included:
网页指标是一组有用的指标，旨在捕捉网页的用户体验。以下 Web 指标都包括在内：

- [Time to First Byte](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_first_byte) (TTFB)
  到第一个字节的时间 （TTFB）
- [First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint) (FCP) 首次内容绘制 （FCP）
- [Largest Contentful Paint](https://web.dev/lcp/) (LCP) 最大含量涂料 （LCP）
- [First Input Delay](https://web.dev/fid/) (FID) 首次输入延迟 （FID）
- [Cumulative Layout Shift](https://web.dev/cls/) (CLS) 累积布局偏移 （CLS）
- [Interaction to Next Paint](https://web.dev/inp/) (INP) *(experimental)*
  与下一个绘画的交互 （INP） （实验性）