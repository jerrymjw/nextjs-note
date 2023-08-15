# Edge Runtime

Next.js Edge Runtime基于标准 Web API，它支持以下 API：

## [Network APIs 网络接口](https://nextjs.org/docs/app/api-reference/edge#network-apis)

| API                                                          | Description 描述                                  |
| ------------------------------------------------------------ | ------------------------------------------------- |
| [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) | Fetches a resource 提取资源                       |
| [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) | Represents an HTTP request 表示 HTTP 请求         |
| [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) | Represents an HTTP response 表示 HTTP 响应        |
| [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers) | Represents HTTP headers 表示 HTTP 标头            |
| [`FetchEvent`](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent) | Represents a fetch event 表示提取事件             |
| [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) | Adds an event listener 添加事件侦听器             |
| [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData) | Represents form data 表示表单数据                 |
| [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File) | Represents a file 表示一个文件                    |
| [`Blob`](https://developer.mozilla.org/en-US/docs/Web/API/Blob) | Represents a blob 表示一个 blob                   |
| [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) | Represents URL search parameters 表示网址搜索参数 |

## [Encoding APIs 编码接口](https://nextjs.org/docs/app/api-reference/edge#encoding-apis)

| API                                                          | Description 描述                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`TextEncoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder) | Encodes a string into a Uint8Array 将字符串编码为 Uint8Array |
| [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) | Decodes a Uint8Array into a string 将 Uint8Array 解码为字符串 |
| [`atob`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/atob) | Decodes a base-64 encoded string 解码 base-64 编码的字符串   |
| [`btoa`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/btoa) | Encodes a string in base-64 以 base-64 编码字符串            |

## [Stream APIs 流接口](https://nextjs.org/docs/app/api-reference/edge#stream-apis)

| API                                                          | Description 描述                                           |
| ------------------------------------------------------------ | ---------------------------------------------------------- |
| [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream) | Represents a readable stream 表示可读流                    |
| [`WritableStream`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream) | Represents a writable stream 表示可写流                    |
| [`WritableStreamDefaultWriter`](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultWriter) | Represents a writer of a WritableStream 表示可写流的编写器 |
| [`TransformStream`](https://developer.mozilla.org/en-US/docs/Web/API/TransformStream) | Represents a transform stream 表示转换流                   |
| [`ReadableStreamDefaultReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultReader) | Represents a reader of a ReadableStream 表示可读流的读取器 |
| [`ReadableStreamBYOBReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamBYOBReader) | Represents a reader of a ReadableStream 表示可读流的读取器 |

## [Crypto APIs 加密接口](https://nextjs.org/docs/app/api-reference/edge#crypto-apis)

| API                                                          | Description 描述                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`crypto`](https://developer.mozilla.org/en-US/docs/Web/API/Window/crypto) | Provides access to the cryptographic functionality of the platform 提供对平台加密功能的访问 |
| [`SubtleCrypto`](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto) | Provides access to common cryptographic primitives, like hashing, signing, encryption or decryption 提供对常见加密原语（如哈希、签名、加密或解密）的访问 |
| [`CryptoKey`](https://developer.mozilla.org/en-US/docs/Web/API/CryptoKey) | Represents a cryptographic key 表示加密密钥                  |

## [Web Standard APIs](https://nextjs.org/docs/app/api-reference/edge#web-standard-apis)

| API                                                          | Description 描述                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) | Allows you to abort one or more DOM requests as and when desired 允许您在需要时中止一个或多个 DOM 请求 |
| [`DOMException`](https://developer.mozilla.org/en-US/docs/Web/API/DOMException) | Represents an error that occurs in the DOM 表示 DOM 中发生的错误 |
| [`structuredClone`](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm) | Creates a deep copy of a value 创建值的深层副本              |
| [`URLPattern`](https://developer.mozilla.org/en-US/docs/Web/API/URLPattern) | Represents a URL pattern 表示 URL 模式                       |
| [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) | Represents an array of values 表示值数组                     |
| [`ArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) | Represents a generic, fixed-length raw binary data buffer 表示通用的、固定长度的原始二进制数据缓冲区 |
| [`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics) | Provides atomic operations as static methods 提供原子操作作为静态方法 |
| [`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) | Represents a whole number with arbitrary precision 以任意精度表示整数 |
| [`BigInt64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt64Array) | Represents a typed array of 64-bit signed integers 表示 64 位有符号整数的类型化数组 |
| [`BigUint64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigUint64Array) | Represents a typed array of 64-bit unsigned integers 表示 64 位无符号整数的类型化数组 |
| [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) | Represents a logical entity and can have two values: `true` and `false` 表示逻辑实体，可以有两个值： `true` 和 `false` |
| [`clearInterval`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearInterval) | Cancels a timed, repeating action which was previously established by a call to `setInterval()` 取消先前通过调用 `setInterval()` 建立的定时重复操作 |
| [`clearTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearTimeout) | Cancels a timed, repeating action which was previously established by a call to `setTimeout()` 取消先前通过调用 `setTimeout()` 建立的定时重复操作 |
| [`console`](https://developer.mozilla.org/en-US/docs/Web/API/Console) | Provides access to the browser's debugging console 提供对浏览器调试控制台的访问 |
| [`DataView`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView) | Represents a generic view of an `ArrayBuffer` 表示 `ArrayBuffer` 的通用视图 |
| [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Represents a single moment in time in a platform-independent format 以独立于平台的格式表示单个时刻 |
| [`decodeURI`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURI) | Decodes a Uniform Resource Identifier (URI) previously created by `encodeURI` or by a similar routine 解码以前由 `encodeURI` 或类似例程创建的统一资源标识符 （URI） |
| [`decodeURIComponent`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) | Decodes a Uniform Resource Identifier (URI) component previously created by `encodeURIComponent` or by a similar routine 解码以前由 `encodeURIComponent` 或类似例程创建的统一资源标识符 （URI） 组件 |
| [`encodeURI`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) | Encodes a Uniform Resource Identifier (URI) by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character 通过将某些字符的每个实例替换为表示字符的 UTF-8 编码的一个、两个、三个或四个转义序列来对统一资源标识符 （URI） 进行编码 |
| [`encodeURIComponent`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) | Encodes a Uniform Resource Identifier (URI) component by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character 通过将某些字符的每个实例替换为表示字符的 UTF-8 编码的一个、两个、三个或四个转义序列，对统一资源标识符 （URI） 组件进行编码 |
| [`Error`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) | Represents an error when trying to execute a statement or accessing a property 表示尝试执行语句或访问属性时出现的错误 |
| [`EvalError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/EvalError) | Represents an error that occurs regarding the global function `eval()` 表示有关全局函数 `eval()` 发生的错误 |
| [`Float32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float32Array) | Represents a typed array of 32-bit floating point numbers 表示 32 位浮点数的类型化数组 |
| [`Float64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array) | Represents a typed array of 64-bit floating point numbers 表示 64 位浮点数的类型化数组 |
| [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) | Represents a function 表示函数                               |
| [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity) | Represents the mathematical Infinity value 表示数学无穷大值  |
| [`Int8Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int8Array) | Represents a typed array of 8-bit signed integers 表示 8 位有符号整数的类型化数组 |
| [`Int16Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int16Array) | Represents a typed array of 16-bit signed integers 表示 16 位有符号整数的类型化数组 |
| [`Int32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int32Array) | Represents a typed array of 32-bit signed integers 表示 32 位有符号整数的类型化数组 |
| [`Intl`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) | Provides access to internationalization and localization functionality 提供对国际化和本地化功能的访问 |
| [`isFinite`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isFinite) | Determines whether a value is a finite number 确定值是否为有限数 |
| [`isNaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN) | Determines whether a value is `NaN` or not 确定值是否为 `NaN` |
| [`JSON`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) | Provides functionality to convert JavaScript values to and from the JSON format 提供将 JavaScript 值与 JSON 格式相互转换的功能 |
| [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) | Represents a collection of values, where each value may occur only once 表示值的集合，其中每个值只能出现一次 |
| [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) | Provides access to mathematical functions and constants 提供对数学函数和常量的访问 |
| [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) | Represents a numeric value 表示数值                          |
| [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) | Represents the object that is the base of all JavaScript objects 表示作为所有 JavaScript 对象的基础的对象 |
| [`parseFloat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) | Parses a string argument and returns a floating point number 分析字符串参数并返回浮点数 |
| [`parseInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) | Parses a string argument and returns an integer of the specified radix 分析字符串参数并返回指定基数的整数 |
| [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) | Represents the eventual completion (or failure) of an asynchronous operation, and its resulting value 表示异步操作的最终完成（或失败）及其结果值 |
| [`Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) | Represents an object that is used to define custom behavior for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc) 表示用于定义基本操作（例如属性查找、赋值、枚举、函数调用等）的自定义行为的对象 |
| [`RangeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RangeError) | Represents an error when a value is not in the set or range of allowed values 表示当值不在允许值的集合或范围内时的错误 |
| [`ReferenceError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError) | Represents an error when a non-existent variable is referenced 表示引用不存在的变量时的错误 |
| [`Reflect`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) | Provides methods for interceptable JavaScript operations 提供用于可拦截的 JavaScript 操作的方法 |
| [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) | Represents a regular expression, allowing you to match combinations of characters 表示正则表达式，允许您匹配字符组合 |
| [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) | Represents a collection of values, where each value may occur only once 表示值的集合，其中每个值只能出现一次 |
| [`setInterval`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) | Repeatedly calls a function, with a fixed time delay between each call 重复调用函数，每次调用之间具有固定的时间延迟 |
| [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) | Calls a function or evaluates an expression after a specified number of milliseconds 在指定的毫秒数后调用函数或计算表达式 |
| [`SharedArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) | Represents a generic, fixed-length raw binary data buffer 表示通用的、固定长度的原始二进制数据缓冲区 |
| [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) | Represents a sequence of characters 表示一系列字符           |
| [`Symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) | Represents a unique and immutable data type that is used as the key of an object property 表示用作对象属性键的唯一且不可变的数据类型 |
| [`SyntaxError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) | Represents an error when trying to interpret syntactically invalid code 表示尝试解释语法无效代码时出现的错误 |
| [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) | Represents an error when a value is not of the expected type 表示值不是预期类型时的错误 |
| [`Uint8Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | Represents a typed array of 8-bit unsigned integers 表示 8 位无符号整数的类型化数组 |
| [`Uint8ClampedArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray) | Represents a typed array of 8-bit unsigned integers clamped to 0-255 表示固定为 0-255 的 8 位无符号整数的类型化数组 |
| [`Uint32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint32Array) | Represents a typed array of 32-bit unsigned integers 表示 32 位无符号整数的类型化数组 |
| [`URIError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/URIError) | Represents an error when a global URI handling function was used in a wrong way 表示以错误方式使用全局 URI 处理函数时出现的错误 |
| [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL) | Represents an object providing static methods used for creating object URLs 表示提供用于创建对象 URL 的静态方法的对象 |
| [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) | Represents a collection of key/value pairs 表示键/值对的集合 |
| [`WeakMap`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap) | Represents a collection of key/value pairs in which the keys are weakly referenced 表示键/值对的集合，其中键被弱引用 |
| [`WeakSet`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet) | Represents a collection of objects in which each object may occur only once 表示对象的集合，其中每个对象只能出现一次 |
| [`WebAssembly`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly) | Provides access to WebAssembly 提供对 WebAssembly 的访问     |

## [Next.js Specific Polyfills 下一页.js特定填充物](https://nextjs.org/docs/app/api-reference/edge#nextjs-specific-polyfills)

- [`AsyncLocalStorage`](https://nodejs.org/api/async_context.html#class-asynclocalstorage)

## [Environment Variables 环境变量](https://nextjs.org/docs/app/api-reference/edge#environment-variables)

You can use `process.env` to access [Environment Variables](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables) for both `next dev` and `next build`.
您可以使用 `process.env` 访问 `next dev` 和 `next build` 的环境变量。

## [Unsupported APIs 不受支持的接口](https://nextjs.org/docs/app/api-reference/edge#unsupported-apis)

The Edge Runtime has some restrictions including:
边缘运行时有一些限制，包括：

- Native Node.js APIs **are not supported**. For example, you can't read or write to the filesystem.
  不支持本机节点.js API。例如，您无法读取或写入文件系统。
- `node_modules` *can* be used, as long as they implement ES Modules and do not use native Node.js APIs.
  可以使用 `node_modules` ，只要它们实现 ES 模块并且不使用本机 Node.js API。
- Calling `require` directly is **not allowed**. Use ES Modules instead.
  不允许直接调用 `require` 。请改用 ES 模块。

The following JavaScript language features are disabled, and **will not work:**
以下 JavaScript 语言功能已禁用，并且不起作用：

| API                                                          | Description 描述                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`eval`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval) | Evaluates JavaScript code represented as a string 计算表示为字符串的 JavaScript 代码 |
| [`new Function(evalString)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) | Creates a new function with the code provided as an argument 使用作为参数提供的代码创建新函数 |
| [`WebAssembly.compile`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/compile) | Compiles a WebAssembly module from a buffer source 从缓冲源编译 WebAssembly 模块 |
| [`WebAssembly.instantiate`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/instantiate) | Compiles and instantiates a WebAssembly module from a buffer source 从缓冲源编译和实例化 WebAssembly 模块 |

In rare cases, your code could contain (or import) some dynamic code evaluation statements which *can not be reached at runtime* and which can not be removed by treeshaking. You can relax the check to allow specific files with your Middleware or Edge API Route exported configuration:
在极少数情况下，您的代码可能包含（或导入）一些动态代码评估语句，这些语句在运行时无法访问，并且无法通过树摇删除。您可以放宽检查，以允许具有中间件或边缘 API 路由导出配置的特定文件：

```
export const config = {  runtime: 'edge', // for Edge API Routes only  unstable_allowDynamic: [    // allows a single file    '/lib/utilities.js',    // use a glob to allow anything in the function-bind 3rd party module    '/node_modules/function-bind/**',  ],};
```

`unstable_allowDynamic` is a [glob](https://github.com/micromatch/micromatch#matching-features), or an array of globs, ignoring dynamic code evaluation for specific files. The globs are relative to your application root folder.
`unstable_allowDynamic` 是一个 glob 或 glob 数组，忽略特定文件的动态代码计算。glob 相对于应用程序根文件夹。

Be warned that if these statements are executed on the Edge, *they will throw and cause a runtime error*.
请注意，如果在 Edge 上执行这些语句，它们将引发并导致运行时错误。