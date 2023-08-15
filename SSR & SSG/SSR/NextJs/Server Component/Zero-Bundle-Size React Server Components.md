## React 服务器组件简介

React 服务器组件 （RSC） 是一种基本的组件类型，它从服务器检索数据并在服务器端呈现其内容。这意味着它将能够获取数据以进行快速渲染。这些服务器组件中没有客户端交互。只有呈现的内容才会发送到客户端进行显示。因此，开发人员可以创建跨服务器和客户端的应用。

换句话说，React 服务器组件是客户端应用程序的强大交互性与传统服务器渲染的增强性能的组合。

通过服务器驱动的心智模型，React 服务器组件功能旨在增强 React 应用程序的用户体验和性能。此外，客户端 JavaScript 捆绑包可能会明显更小，因为服务器组件将不包含在客户端捆绑包中。

## React 服务器组件与服务器端渲染

**传统的服务器端渲染 （SSR） 不能被零捆绑包大小的 React Server 组件所取代**。SSR 是一种在**服务器端渲染应用的方法**，它涉及将 HTML 传送到客户端，以便浏览器渲染它。

另一方面，**React Server 组件通过使用中间结构来启用渲染**，而**无需向客户端交付任何捆绑包**，从而**与 SSR 一起工作**。它们**以唯一的格式（而不是 HTML）在服务器端呈现**，并**流式传输到客户端**。当与 Next.js 等静态站点生成器 （SSG） 结合使用时，服务器组件通过添加更多组件来实现可伸缩性，从而允许将服务器树和客户端树组合在一起而不会丢失状态。

SSR 的工作原理是将预构建的 HTML 标记发送到客户端，然后使用 JS 进行水化。在维护客户端状态时，无法重新呈现这些页面。这可以防止用户在加载 JS 之前与您的网站进行交互。相比之下，React Server 组件可以多次调用和呈现，而不会丢失客户端状态，并具有自己的网络数据传输协议。

**服务器组件**可以从**组件树中的任何位置访问后端资源**。当使用**SSR**（例如Next.js）时，你**必须使用getServerProps（）**来**到达后端**。但是，此功能**只能在顶级页面**上使用，**不能使用随机 npm 组件**完成。

## 如何实现服务器组件

在引入 React Server 组件之前，只有客户端组件。但是现在，由于有两种不同类型的组件，您可以使用 server.js 和 client.js 文件后缀（或者 server.jsx 和 client.jsx）来区分它们。缺少任一组件都标识可同时用作服务器和客户端组件的组件。

React Server 组件的基本示例如下所示。

```tsx
import db from "database";

const Note= (props) => {
  const { id } = props;
  const note = db.notes.get(id);
  return (
    <div><h1>{note.title}</h1><p>{note.text}</p></div>
  );
};
```

如您所见，React Server 组件具有与传统 React 组件相同的视觉外观，后者接受 props 并具有渲染方法。但是你需要知道一些 React Server 组件的独特功能：

- 它们**直接访问服务器数据源**，如数据库、微服务和函数。凭借这种灵活性，我们可以构建使用不同数据源运行的内部 API，并**从组件访问服务器**，而**无需将其显式公开给 API**。

```tsx
import db from "database";

const Note= (props) => {
  const { id } = props;
  const note = db.notes.get(id);

  return (
   <NoteContent note={note} />
  );

};
```

- React 服务器组件**不会影响客户端捆绑包的大小**。更重要的是，如果第三方库仅由服务器组件使用，而不由客户端组件使用，则不会将其添加到客户端捆绑包中，因为它不是必需的。

考虑一个名为 NoteContent 的客户端组件，该组件使用标记为 sanitize-html 的第三方库。依赖项将对增加客户端捆绑包大小产生直接影响。

```tsx
// NoteContent.js
// Client Component

import marked from 'marked'; // 35.9K (11.2K gzipped)
import sanitizeHtml from 'sanitize-html'; // 206K (63.3K gzipped)

function NoteContent({content}) {
  const html = sanitizeHtml(marked(content));
  return (/* render */);
}
```

但服务器组件对捆绑包大小没有影响。

```tsx
// NoteContent.server.js 
// Server Component

import marked from 'marked'; // zero bundle size
import sanitizeHtml from 'sanitize-html'; // zero bundle size

function NoteContent({content}) {
  // same as before
}
```

在服务器端渲染时，React 服务器组件的行为完全不同。一旦提取的 UI 状态准备就绪，框架就会对其进行流式处理。这样，可以轻松识别和呈现最重要的 UI 组件。同时可以利用悬念来有效地管理交互式客户端组件。

React 服务器组件还可以呈现客户端组件、本机 HTML 元素和服务器组件。

例如，如果我们有一个名为 EditNoteButton 的客户端组件，我们可以将其导入到我们的服务器组件中并使用它，如下所示。

```tsx
import db from "database";
import EditNoteButton from 'EditNoteButton.client';

const Note= (props) => {
  const { id } = props;
  const note = db.notes.get(id);
  return (
    <div><h1>{note.title}</h1><section>{note.body}</section>
      <EditNoteButton/></div>
  );
};
```

另一方面，客户端组件无法导入服务器组件。如果我们尝试这样做，它会将许多依赖项导入到浏览器捆绑包中。

## 优点和缺点

React Server Components 有很多优点，但也有一些限制。

### Advantages

- 分离客户端和服务器逻辑之间的关注点。
- 仅服务器代码会缩小捆绑包大小，并且对客户端捆绑包没有影响。
- 直接访问私有和自定义的服务器端数据源。
- 与客户端组件无缝集成。
- 渐进式水合作用和流式渲染。
- 子树和组件级别的更新将保留客户端状态。因此，当重新获取服务器组件树时，客户端状态、焦点甚至活动动画都不会受到影响或重置。
- 适当时共享服务器和客户端代码。

### Disadvantages

- 服务器组件无法访问仅客户端功能（如状态）。因此，不支持用于处理状态数据的 React Hook，例如 useState 和 useReducer。
- 用于渲染生命周期方法（如 useEffect 和 useLayoutEffect）的 React Hooks 不能被 React Server 组件使用。
- 不能使用仅限浏览器的 API。
- 由于该功能仍在开发中，因此没有文档。相反，您必须检查演示应用程序的源代码。

## 总结

- 一个component默认使用server来执行组件，除非使用use client
- RSC可以极大的减少包的size，并且如果可以在此处理包完全不会导入到客户端中
- RSC设立的初衷是为了和client端业务分离，也就是说非必要（与客户实时交互）尽量使用server端，比如Data Fetch
- 与用户交互的功能如使用hook，server端是无法完成的，这时候就需要使用use client来使用客户端组件完成
- 或者直接访问数据库或者第三方（如果需要与之交互），而并不需要API给到client端，否则会降低其性能
- **规则：**server组件永远不可以把props传入client组件中（虽然理论上可以），因为这样会把server组件中使用的包带进client组件中造成client端性能降低
- 它的工作原理和流程是：RSC通过中间结构来启动渲染，而无需向客户端交付任何捆绑包，从而与SSR一起工作。她们以唯一的格式（非HTML，具体是什么还不清楚）在服务端渲染，并流式传输到客户端。这样既不会去把包流入客户端造成客户端性能下降，也不会影响客户端的状态，因为服务端根本无所谓客户端是什么状态，因为状态是动态的，服务端只负责做静态的部分，如静态页面，data fetch等，而动态的部分由客户端独立完成。也就是说和传统流程不一样，客户端的功能被分割，比如data fetch被分割到服务端去做


