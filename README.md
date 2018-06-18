# 微前端的那些事儿

**目录**


*   [为什么微前端开始在流行——Web 应用的聚合](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%BE%AE%E5%89%8D%E7%AB%AF%E5%BC%80%E5%A7%8B%E5%9C%A8%E6%B5%81%E8%A1%8C%E2%80%94%E2%80%94web-%E5%BA%94%E7%94%A8%E7%9A%84%E8%81%9A%E5%90%88)
    *   [前端遗留系统迁移](#%E5%89%8D%E7%AB%AF%E9%81%97%E7%95%99%E7%B3%BB%E7%BB%9F%E8%BF%81%E7%A7%BB)
    *   [后端解耦，前端聚合](#%E5%90%8E%E7%AB%AF%E8%A7%A3%E8%80%A6%EF%BC%8C%E5%89%8D%E7%AB%AF%E8%81%9A%E5%90%88)
    *   [兼容遗留系统](#%E5%85%BC%E5%AE%B9%E9%81%97%E7%95%99%E7%B3%BB%E7%BB%9F)
*   [如何解构单体前端应用——前端应用的微服务式拆分](#%E5%A6%82%E4%BD%95%E8%A7%A3%E6%9E%84%E5%8D%95%E4%BD%93%E5%89%8D%E7%AB%AF%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E5%89%8D%E7%AB%AF%E5%BA%94%E7%94%A8%E7%9A%84%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%BC%8F%E6%8B%86%E5%88%86)
    *   [前端微服化](#%E5%89%8D%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8C%96)
        *   [独立开发](#%E7%8B%AC%E7%AB%8B%E5%BC%80%E5%8F%91)
        *   [独立部署](#%E7%8B%AC%E7%AB%8B%E9%83%A8%E7%BD%B2)
        *   [我们真的需要技术无关吗？](#%E6%88%91%E4%BB%AC%E7%9C%9F%E7%9A%84%E9%9C%80%E8%A6%81%E6%8A%80%E6%9C%AF%E6%97%A0%E5%85%B3%E5%90%97%EF%BC%9F)
        *   [不影响用户体验](#%E4%B8%8D%E5%BD%B1%E5%93%8D%E7%94%A8%E6%88%B7%E4%BD%93%E9%AA%8C)
    *   [微前端的设计理念](#%E5%BE%AE%E5%89%8D%E7%AB%AF%E7%9A%84%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5)
        *   [设计理念一：中心化路由](#%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5%E4%B8%80%EF%BC%9A%E4%B8%AD%E5%BF%83%E5%8C%96%E8%B7%AF%E7%94%B1)
        *   [设计理念二：标识化应用](#%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5%E4%BA%8C%EF%BC%9A%E6%A0%87%E8%AF%86%E5%8C%96%E5%BA%94%E7%94%A8)
        *   [设计理念三：生命周期](#%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5%E4%B8%89%EF%BC%9A%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
        *   [设计理念四：独立部署与配置自动化](#%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5%E5%9B%9B%EF%BC%9A%E7%8B%AC%E7%AB%8B%E9%83%A8%E7%BD%B2%E4%B8%8E%E9%85%8D%E7%BD%AE%E8%87%AA%E5%8A%A8%E5%8C%96)
    *   [实战微前端架构设计](#%E5%AE%9E%E6%88%98%E5%BE%AE%E5%89%8D%E7%AB%AF%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1)
        *   [独立部署与配置自动化](#%E7%8B%AC%E7%AB%8B%E9%83%A8%E7%BD%B2%E4%B8%8E%E9%85%8D%E7%BD%AE%E8%87%AA%E5%8A%A8%E5%8C%96)
        *   [应用间路由——事件](#%E5%BA%94%E7%94%A8%E9%97%B4%E8%B7%AF%E7%94%B1%E2%80%94%E2%80%94%E4%BA%8B%E4%BB%B6)
*   [大型 Angular 应用微前端的四种拆分策略](#%E5%A4%A7%E5%9E%8B-angular-%E5%BA%94%E7%94%A8%E5%BE%AE%E5%89%8D%E7%AB%AF%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%8B%86%E5%88%86%E7%AD%96%E7%95%A5)
    *   [前端微服务化：路由懒加载及其变体](#%E5%89%8D%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96%EF%BC%9A%E8%B7%AF%E7%94%B1%E6%87%92%E5%8A%A0%E8%BD%BD%E5%8F%8A%E5%85%B6%E5%8F%98%E4%BD%93)
    *   [微服务化方案：子应用模式](#%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96%E6%96%B9%E6%A1%88%EF%BC%9A%E5%AD%90%E5%BA%94%E7%94%A8%E6%A8%A1%E5%BC%8F)
    *   [方案对比](#%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94)
        *   [标准 LazyLoad](#%E6%A0%87%E5%87%86-lazyload)
        *   [LazyLoad 变体 1：构建时集成](#lazyload-%E5%8F%98%E4%BD%93-1%EF%BC%9A%E6%9E%84%E5%BB%BA%E6%97%B6%E9%9B%86%E6%88%90)
        *   [LazyLoad 变体 2：构建后集成](#lazyload-%E5%8F%98%E4%BD%93-2%EF%BC%9A%E6%9E%84%E5%BB%BA%E5%90%8E%E9%9B%86%E6%88%90)
        *   [前端微服务化](#%E5%89%8D%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96)
    *   [总对比](#%E6%80%BB%E5%AF%B9%E6%AF%94)
*   [前端微服务化：使用微前端框架 Mooa 开发微前端应用](#%E5%89%8D%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96%EF%BC%9A%E4%BD%BF%E7%94%A8%E5%BE%AE%E5%89%8D%E7%AB%AF%E6%A1%86%E6%9E%B6-mooa-%E5%BC%80%E5%8F%91%E5%BE%AE%E5%89%8D%E7%AB%AF%E5%BA%94%E7%94%A8)
    *   [Mooa 概念](#mooa-%E6%A6%82%E5%BF%B5)
    *   [微前端主工程创建](#%E5%BE%AE%E5%89%8D%E7%AB%AF%E4%B8%BB%E5%B7%A5%E7%A8%8B%E5%88%9B%E5%BB%BA)
    *   [Mooa 子应用创建](#mooa-%E5%AD%90%E5%BA%94%E7%94%A8%E5%88%9B%E5%BB%BA)
    *   [导航到特定的子应用](#%E5%AF%BC%E8%88%AA%E5%88%B0%E7%89%B9%E5%AE%9A%E7%9A%84%E5%AD%90%E5%BA%94%E7%94%A8)
*   [前端微服务化：使用特制的 iframe 微服务化 Angular 应用](#%E5%89%8D%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96%EF%BC%9A%E4%BD%BF%E7%94%A8%E7%89%B9%E5%88%B6%E7%9A%84-iframe-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%8C%96-angular-%E5%BA%94%E7%94%A8)
    *   [iframe 微服务架构设计](#iframe-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1)
    *   [微前端框架 Mooa 的特制 iframe 模式](#%E5%BE%AE%E5%89%8D%E7%AB%AF%E6%A1%86%E6%9E%B6-mooa-%E7%9A%84%E7%89%B9%E5%88%B6-iframe-%E6%A8%A1%E5%BC%8F)
    *   [微前端框架 Mooa iframe 通讯机制](#%E5%BE%AE%E5%89%8D%E7%AB%AF%E6%A1%86%E6%9E%B6-mooa-iframe-%E9%80%9A%E8%AE%AF%E6%9C%BA%E5%88%B6)
        *   [发布主应用事件](#%E5%8F%91%E5%B8%83%E4%B8%BB%E5%BA%94%E7%94%A8%E4%BA%8B%E4%BB%B6)
        *   [监听子应用事件](#%E7%9B%91%E5%90%AC%E5%AD%90%E5%BA%94%E7%94%A8%E4%BA%8B%E4%BB%B6)
    *   [示例](#%E7%A4%BA%E4%BE%8B)

为什么微前端开始在流行——Web 应用的聚合
===

> 采用新技术，更多不是因为先进，而是因为它能解决痛点。

过去，我一直有一个疑惑，人们是否真的需要微服务，是否真的需要微前端。毕竟，没有银弹。当人们考虑是否采用一种新的架构，除了考虑它带来好处之外，仍然也考量着存在的大量的风险和技术挑战。

前端遗留系统迁移
---

自微前端框架 [Mooa](https://github.com/phodal/mooa) 及对应的《[微前端的那些事儿](https://github.com/phodal/microfrontend)》发布的两个多月以来，我陆陆续续地接收到一些微前端架构的一些咨询。过程中，我发现了一件很有趣的事：**解决遗留系统，才是人们采用微前端方案最重要的原因**。

这些咨询里，开发人员所遇到的情况，与我之前遇到的情形并相似，我的场景是：设计一个新的前端架构。他们开始考虑前端微服务化，是因为遗留系统的存在。

过去那些使用 Backbone.js、Angular.js、Vue.js 1 等等框架所编写的单页面应用，已经在线上稳定地运行着，也没有新的功能。对于这样的应用来说，我们也没有理由浪费时间和精力重写旧的应用。这里的那些使用旧的、不再使用的技术栈编写的应用，可以称为遗留系统。而，这些应用又需要结合到新应用中使用。我遇到的较多的情况是：旧的应用使用的是 Angular.js 编写，而新的应用开始采用 Angular 2+。这对于业务稳定的团队来说，是极为常见的技术栈。

在即不重写原有系统的基础之下，又可以抽出人力来开发新的业务。其不仅仅对于业务人员来说， 是一个相当吸引力的特性；对于技术人员来说，不重写旧的业务，同时还能做一些技术上的挑战，也是一件相当有挑战的事情。

后端解耦，前端聚合
---

而前端微服务的一个卖点也在这里，去兼容不同类型的前端框架。这让我又联想到微服务的好处，及许多项目落地微服务的原因：

在初期，后台微服务的一个很大的卖点在于，可以使用不同的技术栈来开发后台应用。但是，事实上，采用微服务架构的组织和机构，一般都是中大型规模的。相较于中小型，对于框架和语言的选型要求比较严格，如在内部限定了框架，限制了语言。因此，在充分使用不同的技术栈来发挥微服务的优势这一点上，几乎是很少出现的。在这些大型组织机构里，采用微服务的原因主要还是在于，**使用微服务架构来解耦服务间依赖**。

而在前端微服务化上，则是恰恰与之相反的，人们更想要的结果是**聚合**，尤其是那些 To B（to Bussiness）的应用。

在这两三年里，移动应用出现了一种趋势，用户不想装那么多应用了。而往往一家大的商业公司，会提供一系列的应用。这些应用也从某种程度上，反应了这家公司的组织架构。然而，在用户的眼里他们就是一家公司，他们就只应该有一个产品。相似的，这种趋势也在桌面 Web 出现。**聚合**成为了一个技术趋势，体现在前端的聚合就是微服务化架构。

兼容遗留系统
---

那么，在这个时候，我们就需要使用新的技术、新的架构，来容纳、兼容这些旧的应用。而前端微服务化，正好是契合人们想要的这个卖点呗了。

如何解构单体前端应用——前端应用的微服务式拆分
===

> 刷新页面？路由拆分？No，动态加载组件。

本文分为以下四部分：

 - 前端微服务化思想介绍
 - 微前端的设计理念
 - 实战微前端架构设计
 - 基于 Mooa 进行前端微服务化

前端微服化
---

对于前端微服化来说，有这么一些方案：

 - Web Component 显然可以一个很优秀的基础架构。然而，我们并不可能去大量地复写已有的应用。
 - iFrame。你是说真的吗？
 - 另外一个微前端框架 Single-SPA，显然是一个更好的方式。然而，它并非 Production Ready。
 - 通过路由来切分应用，而这个跳转会影响用户体验。
 - 等等。

因此，当我们考虑前端微服务化的时候，我们希望：

 - 独立部署
 - 独立开发
 - 技术无关
 - 不影响用户体验

### 独立开发

在过去的几星期里，我花费了大量的时间在学习 Single-SPA 的代码。但是，我发现它在开发和部署上真的太麻烦了，完全达不到独立部署地标准。按 Single-SPA 的设计，我需要在入口文件中声名我的应用，然后才能去构建：

```
declareChildApplication('inferno', () => import('src/inferno/inferno.app.js'), pathPrefix('/inferno'));
```

同时，在我的应用里，我还需要去指定我的生命周期。这就意味着，当我开发了一个新的应用时，必须更新两份代码：主工程和应用。这时我们还极可能在同一个源码里工作。 

当出现多个团队的时候，在同一份源码里工作，显然变得相当的不可靠——比如说，对方团队使用的是 Tab，而我们使用的是 2 个空格，隔壁的老王用的是 4 个空格。

### 独立部署

一个单体的前端应用最大的问题是，构建出来的 js、css 文件相当的巨大。而微前端则意味着，这个文件被独立地拆分成多个文件，它们便可以独立去部署应用。

### 我们真的需要技术无关吗？

等等，我们是否真的需要**技术无关**？如果我们不需要技术无关的话，微前端问题就很容易解决了。

事实上，对于大部分的公司和团队来说，技术无关只是一个无关痛痒的话术。当一家公司的几个创始人使用了 Java，那么极有可能在未来的选型上继续使用 Java。除非，一些额外的服务来使用 Python 来实现人工智能。因此，在大部分的情况下，仍然是技术栈唯一。

对于前端项目来说，更是如此：一个部门里基本上只会选用一个框架。

于是，我们选择了 Angular。

### 不影响用户体验

使用路由跳转来进行前端微服务化，是一种很简单、高效的切分方式。然而，路由跳转地过程中，会有一个白屏的过程。在这个过程中，跳转前的应用和将要跳转的应用，都失去了对页面的控制权。如果这个应用出了问题，那么用户就会一脸懵逼。

理想的情况下，它应该可以被控制。

微前端的设计理念
---

### 设计理念一：中心化路由

互联网本质是去中心化的吗？不，DNS 决定了它不是。TAB，决定了它不是。

微服务从本质上来说，它应该是去中心化的。但是，它又不能是完全的去中心化。对于一个微服务来说，它需要一个**服务注册中心**：

> 服务提供方要注册通告服务地址，服务的调用方要能发现目标服务。

对于一个前端应用来说，这个东西就是路由。

从页面上来说，只有我们在网页上添加一个菜单链接，用户才能知道某个页面是可以使用的。

而从代码上来说，那就是我们需要有一个地方来管理我们的应用：**发现存在哪些应用，哪个应用使用哪个路由。

**管理好我们的路由，实际上就是管理好我们的应用**。

### 设计理念二：标识化应用

在设计一个微前端框架的时候，为**每个项目取一个名字的**问题纠结了我很久——怎么去规范化这个东西。直到，我再一次想到了康威定律：

> 系统设计(产品结构等同组织形式，每个设计系统的组织，其产生的设计等同于组织之间的沟通结构。


换句人话说，就是同一个组织下，不可能有两个项目的名称是一样的。

所以，这个问题很简单就解决了。

### 设计理念三：生命周期

Single-SPA 设计了一个基本的生命周期（虽然它没有统一管理），它包含了五种状态：

 - load，决定加载哪个应用，并绑定生命周期
 - bootstrap，获取静态资源
 - mount，安装应用，如创建 DOM 节点
 - unload，删除应用的生命周期
 - unmount，卸载应用，如删除 DOM 节点

于是，我在设计上基本上沿用了这个生命周期。显然，诸如 load 之类对于我的设计是多余的。

### 设计理念四：独立部署与配置自动化

从某种意义上来说，整个每系统是围绕着应用配置进行的。如果应用的配置能自动化，那么整个系统就自动化。

当我们只开发一个新的组件，那么我们只需要更新我们的组件，并更新配置即可。而这个配置本身也应该是能自动生成的。

实战微前端架构设计
---

基于以上的前提，系统的工作流程如下所示：

![系统工作流](./imgs/mooa-graph.jpg)

整体的工程流程如下所示：

1. 主工程在运行的时候，会去服务器获取最新的应用配置。
2. 主工程在获取到配置后，将一一创建应用，并为应用绑定生命周期。
3. 当主工程监测到路由变化的时候，将寻找是否有对应的路由匹配到应用。
4. 当匹配对对应应用时，则加载相应的应用。

故而，其对应的结构下图所示：

![Architecture](./imgs/mooa-app.jpg)

整体的流程如下图所示：

![Workflow](./imgs/workflow.png)

### 独立部署与配置自动化

我们做的部署策略如下：我们的应用使用的配置文件叫 ``apps.json``，由主工程去获取这个配置。每次部署的时候，我们只需要将 ``apps.json`` 指向最新的配置文件即可。配置的文件类如下所示：

1. 96a7907e5488b6bb.json
2. 6ff3bfaaa2cd39ea.json
3. dcd074685c97ab9b.json

一个应用的配置如下所示：

```javascript
{
  "name": "help",
  "selector": "help-root",
  "baseScriptUrl": "/assets/help",
  "styles": [
    "styles.bundle.css"
  ],
  "prefix": "help",
  "scripts": [
    "inline.bundle.js",
    "polyfills.bundle.js",
    "main.bundle.js"
  ]
}
```

这里的 ``selector`` 对应于应用所需要的 DOM 节点，prefix 则是用于 URL 路由上。这些都是自动从 ``index.html`` 文件和 ``package.json`` 中获取生成的。

### 应用间路由——事件

由于现在的应用变成了两部分：主工程和应用部分。就会出现一个问题：**只有一个工程能捕获路由变化**。当由主工程去改变应用的二级路由时，就无法有效地传达到子应用。在这时，只能通过事件的方式去通知子应用，子应用也需要监测是否是当前应用的路由。

```javascript
if (event.detail.app.name === appName) {
  let urlPrefix = 'app'
  if (urlPrefix) {
    urlPrefix = `/${window.mooa.option.urlPrefix}/`
  }
  router.navigate([event.detail.url.replace(urlPrefix + appName, '')])
}
```

相似的，当我们需要从应用 A 跳转到应用 B 时，我们也需要这样的一个机制：

```
window.addEventListener('mooa.routing.navigate', function(event: CustomEvent) {
  const opts = event.detail
  if (opts) {
    navigateAppByName(opts)
  }
})
```

剩下的诸如 Loading 动画也是类似的。


大型 Angular 应用微前端的四种拆分策略
===

上一个月，我们花了大量的时间不熂设计方案来拆分一个大型的 Angular 应用。从使用 Angular 的 Lazyload 到前端微服务化，进行了一系列的讨论。最后，我们终于有了结果，采用的是 Lazyload 变体：**构建时集成代码** 的方式。

过去的几周里，作为一个 “专业” 的咨询师，一直忙于在为客户设计一个 Angular 拆分的服务化方案。主要是为了达成以下的设计目标：

 - 构建插件化的 Web 开发平台，满足业务快速变化及分布式多团队并行开发的需求
 - 构建服务化的中间件，搭建高可用及高复用的前端微服务平台
 - 支持前端的独立交付及部署

简单地来说，就是要支持**应用插件化开发**，以及**多团队并行开发**。

**应用插件化开发**，其所要解决的主要问题是：臃肿的大型应用的拆分问题。大型前端应用，在开发的时候要面临大量的**遗留代码**、不同业务的代码耦合在一起，在线上的时候还要面临加载速度慢，运行效率低的问题。

最后就落在了两个方案上：路由懒加载及其变体与前端微服务化

前端微服务化：路由懒加载及其变体
---

路由懒加载，即通过不同的路由来将应用切成不同的代码快，当路由被访问的时候，才加载对应组件。在诸如 Angular、Vue 框架里都可以通过路由 +  Webpack 打包的方式来实现。而，不可避免地就会需要一些问题：

**难以多团队并行开发**，路由拆分就意味着我们仍然是在一个源码库里工作的。也可以尝试拆分成不同的项目，再编译到一起。

**每次发布需要重新编译**，是的，当我们只是更新一个子模块的代码，我们要重新编译整个应用，再重新发布这个应用。而不能独立地去构建它，再发布它。

**统一的 Vendor 版本**，统一第三方依赖是一件好事。可问题的关键在于：每当我们添加一个新的依赖，我们可能就需要开会讨论一下。

然而，标准 Route Lazyload 最大的问题就是**难以多团队并行开发**，这里之所以说的是 “难以” 是因为，还是有办法解决这个问题。在日常的开发中，一个小的团队会一直在一个代码库里开发，而一个大的团队则应该是在不同的代码库里开发。

于是，我们在标准的路由懒加载之上做了一些尝试。

对于一个二三十人规模的团队来说，他们可能在业务上归属于不同的部门，技术上也有一些不一致的规范，如 4 个空格、2 个空格还是使用 Tab 的问题。特别是当它是不同的公司和团队时，他们可能要放弃测试、代码静态检测、代码风格统一等等的一系列问题。

微服务化方案：子应用模式
---

除了路由懒加载，我们还可以采用子应用模式，即每个应用都是相互独立地。即我们有一个基座工程，当用户点击相应的路由时，我们去加载这个**独立** 的 Angular 应用；如果是同一个应用下的路由，就不需要重复加载了。而且，这些都可以依赖于浏览器缓存来做。

除了路由懒加载，还可以采用的是类似于 Mooa 的应用嵌入方案。如下是基于 Mooa 框架 + Angular 开发而生成的 HTML 示例：

```
<app-root _nghost-c0="" ng-version="4.2.0">
  ...
  <app-home _nghost-c2="">
    <app-app1 _nghost-c0="" ng-version="5.2.8" style="display: none;"><nav _ngcontent-c0="" class="navbar"></app-app1>
    <iframe frameborder="" width="100%" height="100%" src="http://localhost:4200/app/help/homeassets/iframe.html" id="help_206547"></iframe>
  </app-home>
</app-root>
```

Mooa 提供了两种模式，一种是基于 Single-SPA 的实验做的，在同一页面加载、渲染两个 Angular 应用；一种是基于 iFrame 来提供独立的应用容器。

解决了以下的问题：

 - **首页加载速度更快**，因为只需要加载首页所需要的功能，而不是所有的依赖。
 - **多个团队并行开发**，每个团队里可以独立地在自己的项目里开发。
 - **独立地进行模块化更新**，现在我们只需要去单独更新我们的应用，而不需要更新整个完整的应用。

但是，它仍然包含有以下的问题：

 - 重复加载依赖项，即我们在 A 应用中使用到的模块，在 B 应用中也会重新使用到。有一部分可以通过浏览器的缓存来自动解决。
 - 第一次打开对应的应用需要时间，当然**预加载**可以解决一部分问题。
 - 在非 iframe 模式下运行，会遇到难以预料的第三方依赖冲突。

于是在总结了一系列的讨论之后，我们形成了一系列的对比方案：

方案对比
---

在这个过程中，我们做了大量的方案设计与对比，便想写一篇文章对比一下之前的结果。先看一下图：

![Angular 代码拆分对比](./imgs/angular-split-code-compare.jpg)

表格对比：

x       | 标准 Lazyload |   构建时集成  | 构建后集成   | 应用独立 
--------|--------------|------------|-------------|-------------
开发流程 |  多个团队在同一个代码库里开发 | 多个团队在同不同的代码库里开发 | 多个团队在同不同的代码库里开发 | 多个团队在同不同的代码库里开发 
构建与发布 | 构建时只需要拿这一份代码去构建、部署 | 将不同代码库的代码整合到一起，再构建应用 | 将直接编译成各个项目模块，运行时通过懒加载合并 |  将直接编译成不同的几个应用，运行时通过主工程加载 
适用场景 |  单一团队，依赖库少、业务单一 | 多团队，依赖库少、业务单一 | 多团队，依赖库少、业务单一 |  多团队，依赖库多、业务复杂 
表现方式 | 开发、构建、运行一体  | 开发分离，构建时集成，运行一体| 开发分离，构建分离，运行一体 |  开发、构建、运行分离

详细的介绍如下：

### 标准 LazyLoad

开发流程：多个团队在同一个代码库里开发，构建时只需要拿这一份代码去部署。

行为：开发、构建、运行一体

适用场景：单一团队，依赖库少、业务单一

### LazyLoad 变体 1：构建时集成

开发流程：多个团队在同不同的代码库里开发，在构建时将不同代码库的代码整合到一起，再去构建这个应用。

适用场景：多团队，依赖库少、业务单一

变体-构建时集成：开发分离，构建时集成，运行一体

### LazyLoad 变体 2：构建后集成

开发流程：多个团队在同不同的代码库里开发，在构建时将编译成不同的几份代码，运行时会通过懒加载合并到一起。

适用场景：多团队，依赖库少、业务单一

变体-构建后集成：开发分离，构建分离，运行一体

### 前端微服务化

开发流程：多个团队在同不同的代码库里开发，在构建时将编译成不同的几个应用，运行时通过主工程加载。

适用场景：多团队，依赖库多、业务复杂

前端微服务化：开发、构建、运行分离

总对比
---

总体的对比如下表所示：

x       | 标准 Lazyload |   构建时集成  | 构建后集成   | 应用独立 
--------|--------------|------------|-------------|-------------
依赖管理 |  统一管理     | 统一管理   | 统一管理  | 各应用独立管理
部署方式 |  统一部署     | 统一部署  | 可单独部署。更新依赖时，需要全量部署 | 可完全独立部署
首屏加载 |  依赖在同一个文件，加载速度慢 | 依赖在同一个文件，加载速度慢 | 依赖在同一个文件，加载速度慢  | 依赖各自管理，首页加载快
首次加载应用、模块 | 只加载模块，速度快  | 只加载模块，速度快 | 只加载模块，速度快  | 单独加载，加载略慢 
前期构建成本 |  低 | 设计构建流程|   设计构建流程 | 设计通讯机制与加载方式
维护成本    | 一个代码库不好管理 | 多个代码库不好统一 | 后期需要维护组件依赖 | 后期维护成本低
打包优化  | 可进行摇树优化、AoT 编译、删除无用代码| 可进行摇树优化、AoT 编译、删除无用代码 | 应用依赖的组件无法确定，不能删除无用代码 | 可进行摇树优化、AoT 编译、删除无用代码

前端微服务化：使用微前端框架 Mooa 开发微前端应用
===

Mooa 是一个为 Angular 服务的微前端框架，它是一个基于 [single-spa](https://github.com/CanopyTax/single-spa)，针对 IE 10 及 IFRAME 优化的微前端解决方案。

Mooa 概念
---

Mooa 框架与 Single-SPA 不一样的是，Mooa 采用的是 Master-Slave 架构，即主-从式设计。

对于 Web 页面来说，它可以同时存在两个到多个的 Angular 应用：其中的一个 Angular 应用作为主工程存在，剩下的则是子应用模块。

 - 主工程，负责加载其它应用，及用户权限管理等核心控制功能。
 - 子应用，负责不同模块的具体业务代码。

在这种模式下，则由主工程来控制整个系统的行为，子应用则做出一些对应的响应。

微前端主工程创建
---

要创建微前端框架 Mooa 的主工程，并不需要多少修改，只需要使用 ``angular-cli`` 来生成相应的应用：

```
ng new hello-world
```

然后添加 ``mooa`` 依赖

```
yarn add mooa
```

接着创建一个简单的配置文件 ``apps.json``，放在 ``assets`` 目录下：

```
[{
    "name": "help",
    "selector": "app-help",
    "baseScriptUrl": "/assets/help",
    "styles": [
      "styles.bundle.css"
    ],
    "prefix": "help",
    "scripts": [
      "inline.bundle.js",
      "polyfills.bundle.js",
      "main.bundle.js"
    ]
  }
]]
```

接着，在我们的 ``app.component.ts`` 中编写相应的创建应用逻辑：

```
mooa = new Mooa({
  mode: 'iframe',
  debug: false,
  parentElement: 'app-home',
  urlPrefix: 'app',
  switchMode: 'coexist',
  preload: true,
  includeZone: true
});

constructor(private renderer: Renderer2, http: HttpClient, private router: Router) {
  http.get<IAppOption[]>('/assets/apps.json')
    .subscribe(
      data => {
        this.createApps(data);
      },
      err => console.log(err)
    );
}

private createApps(data: IAppOption[]) {
  data.map((config) => {
    this.mooa.registerApplication(config.name, config, mooaRouter.hashPrefix(config.prefix));
  });

  const that = this;
  this.router.events.subscribe((event) => {
    if (event instanceof NavigationEnd) {
      that.mooa.reRouter(event);
    }
  });

  return mooa.start();
}
```

再为应用创建一个对应的路由即可：

```
{
  path: 'app/:appName/:route',
  component: HomeComponent
}
```

接着，我们就可以创建 Mooa 子应用。


Mooa 子应用创建
---

Mooa 官方提供了一个子应用的模块，直接使用该模块即可：

```
git clone https://github.com/phodal/mooa-boilerplate
```

然后执行：

```
npm install
```

在安装完依赖后，会进行项目的初始化设置，如更改包名等操作。在这里，将我们的应用取名为 help。

然后，我们就可以完成子应用的构建。

接着，执行：``yarn build`` 就可以构建出我们的应用。

将 ``dist`` 目录一下的文件拷贝到主工程的 src/assets/help 目录下，再启动主工程即可。

导航到特定的子应用
---

在 Mooa 中，有一个路由接口 ``mooaPlatform.navigateTo``，具体使用情况如下：

```
mooaPlatform.navigateTo({
  appName: 'help',
  router: 'home'
});
```

它将触发一个 ``MOOA_EVENT.ROUTING_NAVIGATE`` 事件。而在我们调用 ``mooa.start()`` 方法时，则会开发监听对应的事件：

```
window.addEventListener(MOOA_EVENT.ROUTING_NAVIGATE, function(event: CustomEvent) {
  if (event.detail) {
    navigateAppByName(event.detail)
  }
})
```

它将负责将应用导向新的应用。

嗯，就是这么简单。DEMO 视频如下：

Demo 地址见：[http://mooa.phodal.com/](http://mooa.phodal.com/)

GitHub 示例：[https://github.com/phodal/mooa](https://github.com/phodal/mooa)

前端微服务化：使用特制的 iframe 微服务化 Angular 应用
===

Angular 基于 Component 的思想，可以让其在一个页面上同时运行多个 Angular 应用；可以在一个 DOM 节点下，存在多个 Angular 应用，即类似于下面的形式：

```html
<app-home _nghost-c3="" ng-version="5.2.8">
  <app-help _nghost-c0="" ng-version="5.2.2" style="display:block;"><div _ngcontent-c0=""></div></app-help>
  <app-app1 _nghost-c0="" ng-version="5.2.3" style="display:none;"><nav _ngcontent-c0="" class="navbar"></div></app-app1>
  <app-app2 _nghost-c0="" ng-version="5.2.2" style="display:none;"><nav _ngcontent-c0="" class="navbar"></div></app-app2>
</app-home>
```

可这一样一来，难免需要做以下的一些额外的工作：

 - 创建子应用项目模板，以统一 Angular 版本
 - 构建时，删除子应用的依赖
 - 修改第三方模块

而在这其中最麻烦的就是**第三方模块**冲突问题。思来想去，在三月中旬，我在 Mooa 中添加了一个 iframe 模式。

iframe 微服务架构设计
---

在这里，总的设计思想和之前的《[如何解构单体前端应用——前端应用的微服务式拆分](https://www.phodal.com/blog/how-to-build-a-microfrontend-framework-mooa/)》中介绍是一致的：

![Mooa 架构](./imgs/mooa-graph.jpg)

主要过程如下：

 - 主工程在运行的时候，会去服务器获取最新的应用配置。
 - 主工程在获取到配置后，将一一创建应用，并为应用绑定生命周期。
 - 当主工程监测到路由变化的时候，将寻找是否有对应的路由匹配到应用。
 - 当匹配对对应应用时，则**创建或显示相应应用的 iframe**，并隐藏其它子应用的 iframe。

其加载形式与之前的 Component 模式并没有太大的区别：

![Mooa Component 加载](./imgs/mooa-app.jpg)

而为了控制不同的 iframe 需要做到这么几件事：

 1. 为不同的子应用分配 ID
 2. 在子应用中进行 hook，以通知主应用：子应用已加载
 3. 在子应用中创建对应的事件监听，来响应主应用的 URL 变化事件
 4. 在主应用中监听子程序的路由跳转等需求

因为大部分的代码可以与之前的 [Mooa](https://github.com/phodal/mooa) 复用，于是我便在 Mooa 中实现了相应的功能。

微前端框架 Mooa 的特制 iframe 模式
---

iframe 可以创建一个**全新的独立的宿主环境**，这意味着我们的 Angular 应用之间可以相互独立运行，我们唯一要做的是：**建立一个通讯机制**。

它可以不修改子应用代码的情况下，可以直接使用。与此同时，它在一般的 iframe 模式进行了优化。使用普通的 iframe 模式，意味着：**我们需要加载大量的重复组件**，即使经过 Tree-Shaking 优化，它也将带来大量的重复内容。如果子应用过多，那么它在初始化应用的时候，体验可能就没有那么友好。但是与此相比，在初始化应用的时候，加载所有的依赖在主程序上，也不是一种很友好的体验。

于是，我就在想能不能创建一个更友好地 IFrame 模式，在里面对应用及依赖进行处理。如下，就是最后生成的页面的 iframe 代码：


```html
<app-home _nghost-c2="" ng-version="5.2.8">
  <iframe frameborder="" width="100%" height="100%" src="http://localhost:4200/assets/iframe.html" id="help_206547" style="display:block;"></iframe>
  <iframe frameborder="" width="100%" height="100%" src="http://localhost:4200/assets/iframe.html" id="app_235458 style="display:none;"></iframe>
</app-home>
```

对，两个 iframe 的 src 是一样的，但是它表现出来的确实是两个不同的 iframe 应用。那个 iframe.html 里面其实是没有内容的：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>App1</title>
  <base href="/">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>

</body>
</html>
```

（PS：详细的代码可以见 [https://github.com/phodal/mooa](https://github.com/phodal/mooa)）

只是为了创建 iframe 的需要而存在的，对于一个 Angular 应用来说，是不是一个 iframe 的区别并不大。但是，对于我们而言，区别就大了。我们可以使用自己的方式来控制这个 IFrame，以及我们所要加载的内容。如：

 - 共同 Style Guide 中的 CSS 样式。如，在使用 iframe 集成时，移除不需要的 <link>
 - 去除不需要重复加载的 JavaScript。如，打包时不需要的 zone.min.js、polyfill.js 等等

``注意``：对于一些共用 UI 组件而言，仍然需要重复加载。这也就是 iframe 模式下的问题。

微前端框架 Mooa iframe 通讯机制
---

为了在主工程与子工程通讯，我们需要做到这么一些事件策略：

### 发布主应用事件

由于，我们使用 Mooa 来控制 iframe 加载。这就意味着我们可以通过 ``document.getElementById`` 来获取到 iframe，随后通过 ``iframeEl.contentWindow`` 来发布事件，如下：

```typescript
let iframeEl: any = document.getElementById(iframeId)
if (iframeEl && iframeEl.contentWindow) {
  iframeEl.contentWindow.mooa.option = window.mooa.option
  iframeEl.contentWindow.dispatchEvent(
    new CustomEvent(MOOA_EVENT.ROUTING_CHANGE, { detail: eventArgs })
  )
}
```

这样，子应用就不需要修改代码，就可以直接接收对应的事件响应。


### 监听子应用事件

由于，我们也希望能直接在主工程中处理子程序的事件，并且不修改原有的代码。因此，我们也使用同样的方式来在子应用中监听主应用的事件：

```
iframeEl.contentWindow.addEventListener(MOOA_EVENT.ROUTING_NAVIGATE, function(event: CustomEvent) {
  if (event.detail) {
    navigateAppByName(event.detail)
  }
})
```

示例
---

同样的我们仍以 Mooa 框架作为示例，我们只需要在创建 mooa 实例时，配置使用 iframe 模式即可：

```typescript
this.mooa = new Mooa({
  mode: 'iframe',
  debug: false,
  parentElement: 'app-home',
  urlPrefix: 'app',
  switchMode: 'coexist',
  preload: true,
  includeZone: true
});

...

that.mooa.registerApplicationByLink('help', '/assets/help', mooaRouter.matchRoute('help'));
that.mooa.registerApplicationByLink('app1', '/assets/app1', mooaRouter.matchRoute('app1'));
this.mooa.start();

...
this.router.events.subscribe((event: any) => {
  if (event instanceof NavigationEnd) {
    that.mooa.reRouter(event);
  }
});
```

子程序则直接使用：[https://github.com/phodal/mooa-boilerplate](https://github.com/phodal/mooa-boilerplate) 就可以了。
