## 渲染引擎

渲染引擎的职责就是渲染，即在浏览器窗口中显示所请求的内容。默认情况下，渲染引擎可以显示 HTML、XML 文档及图片，它也可以借助插件（一种浏览器扩展）显示其他类型数据，例如使用 PDF 阅读器插件，可以显示 PDF 格式。渲染引擎最主要的用途——显示应用了 CSS 之后的 HTML 及图片。

### 渲染引擎种类

不同的浏览器有不同的渲染引擎，对于渲染引擎的使用总结如下：

- Webkit内核：Safari/Chrome等
- Gecko内核：Netscape6及以上版本/Firefox/MozillaSuite/SeaMonkey等
- Trident（MSHTML）内核：IE/MaxThon/TT/The World/360/搜狗浏览器等
- Presto内核：Opera7及以上
- Edge内核：Win10以上IE浏览器
- Blink内核：Chromium

JavaScript 引擎：

- JScript引擎：IE 系列浏览器
- spiderMonkey引擎：Mozilla Firefox
- V8引擎：Google Chrome
- linear b/futhark引擎：Opera

### 渲染引擎及依赖模块分析

![渲染引擎及依赖模块分析](../../images/5/824d82fb-f521-4adc-9892-539db9547ca4.png)

上图中虚线部分表示渲染引擎所提供的功能。 这里渲染引擎包含了JavaScript引擎，许多时候两者都不太区分。

### 渲染引擎基本流程

渲染引擎首先通过网络获得所请求文档的内容，通常以8k分块的方式完成。

下面是渲染引擎在获取文档内容之后的基本流程：

![渲染引擎基本流程](../../images/5/273fa38f-7637-46c9-a5fc-54a28a8fff9e.png)

1. 解析 HTML：渲染引擎开始解析 HTML，并将标签转化为 DOM 节点树，即内容树
2. 解析 CSS：同时，它也会解析外部 CSS 文件及 `<style>` 标签中的样式数据，生成 CSS 规则树
3. 构建渲染树：这样样式信息以及 HTML 中的可见性指令将被用来构建另一个棵树——渲染树
   - 渲染树由一些包含有颜色和大小属性的矩形组成，它们将按照正确的顺序显示到屏幕上
4. 布局：渲染树构建好之后，将会执行布局过程，它将确定每个节点在屏幕上的确切坐标
5. 绘制：再下一步就是绘制，即遍历渲染树，并使用 UI 后端层绘制每个节点

值得注意的是，这个过程是逐步完成的，为了更好的用户体验，渲染引擎将会尽可能早地将内容呈现到屏幕上，并不会等到所有 HTML 都解析完成之后再去构建和布局渲染树，它是解析完一部分内容就显示一部分内容，同时，可能还在通过网络下载其余内容。

### 主要浏览器渲染主流程

![Webkit主流程](../../images/5/01908eec-19c7-4676-8c6f-fe574a7364b4.png)



![Gecko主流程](../../images/5/205dae2d-835d-4e31-9592-c6ee9abe039a.png)

- Gecko 将视觉格式化元素组成的树称为”框架树”（frame）。每个元素都是一个框架。Webkit 使用的术语是”渲染树”（render），它由”渲染对象”组成。
- 对于元素的放置，Webkit 使用的术语是”布局”（layout），而 Gecko 称之为”重排”（reflow）。
- Webkit 称利用 DOM 节点及样式信息去构建渲染树的过程为 Attachment，Gecko 在 HTML 和 DOM 树之间附加了一层，这层称为内容接收器，相当制造dom元素的工厂。

---

参考资料：

- [浏览器渲染引擎到底做了什么](https://www.jianshu.com/p/281b574ee3f8)
- [浅析渲染引擎与前端优化](https://blog.csdn.net/john1337/article/details/53579506)

 

 

 

 