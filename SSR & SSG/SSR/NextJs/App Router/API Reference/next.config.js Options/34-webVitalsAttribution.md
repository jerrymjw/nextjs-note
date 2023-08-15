# webVitalsAttribution

在调试与 Web 指标相关的问题时，如果我们能够查明问题的根源，通常会很有帮助。例如，在累积布局偏移 （CLS） 的情况下，我们可能想知道在发生单个最大布局偏移时移动的第一个元素。或者，对于最大内容绘制 （LCP），我们可能希望标识与页面的 LCP 对应的元素。如果 LCP 元素是图像，了解图像资源的 URL 可以帮助我们找到需要优化的资产。

通过确定对 Web Vitals 分数（又名Attribution）的最大贡献者，我们可以获得更深入的信息，例如  [PerformanceEventTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceEventTiming), [PerformanceNavigationTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming) 和 [PerformanceResourceTiming](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming).的条目。

默认情况下，Attribution在 Next.js 中处于禁用状态但可以通过在 `next.config.js` 中指定以下内容来按指标启用。