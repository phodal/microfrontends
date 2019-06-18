# Thinking in Microfrontend

> Micro-frontends is a microservice-like architecture that applies the concept of microservices to the browser side. Transforming to a mono-like applications from a single, single application to an application that combines multiple small front-end applications. Each frontend application can also be **standalone run**, **independent development**, **standalone deployment**.

At the same time, they can also be developed in parallel with **sharing components** - these components can be managed via NPM or Git Tag, Git Submodule etc.

**Note**: The front-end application here refers to a single-page application separated from the front and back. It is meaningful to talk about the micro-frontends on this basis.


Why micro-frontends be popular – aggregation of web applications
===

> Adopting new technology, more is not because of advanced, but because it can solve the pain.

In the past, I have always had a doubt as to whether people really need microservices and whether they really need micro-frontends. After all, there is no silver bullet. When people consider whether to adopt a new architecture, in addition to considering its benefits, it still considers the large number of risks and technical challenges.

Migration frontend legacy system 
---

For past years, I have received some consultations on how to implement micro-frontends architecture. In the meantime, I found a very interesting thing: **Resolving the legacy system is the most important reason people use the micro-frontends solution**.

In these consultations, the situation encountered by the developers is similar to the situation I encountered before. My scenario is: design a new front-end architecture. They began to consider frontend with micro-services because of the legacy system.

In the past, single-page applications written using Backbone.js, Angular.js, Vue.js 1 and other frameworks have been running steadily online and have no new features. For such applications, there is no reason to waste time and paty effort to rewriting old applications. Applications in here, using old, no longer used technology stacks can be called legacy systems. However, these applications need to be combined into new applications. The more I've encountered is that the old app was written in Angular.js, and the new app started with Angular 2+. This is a very common technology stack for a business-stable IT team.

Under the premise of not rewriting the original system, it is possible to extract manpower to develop new business. It is not only a very attractive feature for business people; it is also quite challenging for technicians not to rewrite old business and to make some technical challenges.

Backend decoupling, frontend aggregation
---

A selling point for front-end microservices is also here, to be compatible with different types of front-end frameworks. This reminds me of the benefits of microservices and the reasons why many projects fall into microservices:

In the early, a big selling point for back-end microservices was that different technology stacks could be used to develop background applications. However, in fact, organizations and institutions that adopt a micro-service architecture are generally medium to large-scale. Compared with small and medium-sized, the selection of frameworks and languages ​​is more stringent, such as limiting the framework internally and limiting the language. Therefore, it is almost rare to fully utilize the different technology stacks to take advantage of microservices. In these large organizations, the main reason for adopting microservices is to **use the microservices architecture to decouple inter-service dependencies**.

In the frontend micro-services, it is exactly the opposite. The result people want more is **aggregation**, especially those applications of To B(to Bussiness).

In the past two or three years, mobile applications have shown a trend, users do not want to install so many applications. And often a large commercial company offers a range of applications. These applications also reflect, to some extent, the organizational structure of the company. However, in the eyes of users they are a company, they should only have one product. Similarly, this trend is also emerging on the desktop web. **Aggregation** has become a technology trend, and the aggregation in the front end is the micro-service architecture.

Compatible with legacy systems
---

Then, at this time, we need to use new technologies and new architectures to accommodate and be compatible with these old applications. The front-end micro-services just fits the selling point that people want.


Six ways to implement a micro-frontends architecture
===

Combined with my practice and research in the [micro-frontends](https://github.com/phodal/microfrontends) in the past six months, the micro-frontends architecture can generally be implemented in the following ways:

1. Use HTTP server routing to redirect multiple apps
2. Design communication and loading mechanisms on different frameworks, such as [Mooa](https://github.com/phodal/mooa) and [Single-SPA](https://github.com/CanopyTax/single-spa)
3. Build a single application by combining multiple independent applications and components
4. iFrame. Use iFrame and custom messaging mechanisms
5. Build an app with pure Web Components
6. Building with Web Components
7. Seperate application in build-time (TBC)
8. Seperate appliation in local build time (TBC)

Basic: Application Distribution Routing -> Route Distribution Application
---

In a monoli front-end, mono back-end application, there is a typical feature that routing is distributed by the **framework**, which assigns routes to corresponding components or internal services. What the microservice does in the process is to call the **function call** into a **remote call**, such as a remote HTTP call. The micro-frontends is similar, it is to change the component call** in the application into a more fine-grained **inter-application component call**, that is, we just distribute the route to the application component execution. Now you need to find the corresponding application based on the route, and then distribute it to the corresponding component by the application.

### Backend: Function Call -> Remote Call

In most CRUD-type web applications, there are some very similar patterns, namely: Home -> List -> Details:

 - Home page for displaying specific data or pages to users. These data are usually a finite number and are of multiple models.
 - List, the aggregation of the data model, which is typically a collection of data of a certain type, can see as many ** data summaries** (such as Google only returns 100 pages), typically see Google, Taobao / Ebay, Amazon search results page.
 - Details, showing as much content as possible for a single piece of data.

Here's an example of a Spring framework for returning to the home page:

```java
@RequestMapping(value="/")
Public ModelAndView homePage(){
   Return new ModelAndView("/WEB-INF/jsp/index.jsp");
}
```

For a detail page, it might look like this:

```java
@RequestMapping(value="/detail/{detailId}")
Public ModelAndView detail(HttpServletRequest request, ModelMap model){
   ....
   Return new ModelAndView("/WEB-INF/jsp/detail.jsp", "detail", detail);
}
```

So, in the case of microservices, it will look like this:

```java
@RequestMapping("/name")
Public String name(){
    String name = restTemplate.getForObject("http://account/name", String.class);
    Return Name" + name;
}
```

In the process, the backend has a service discovery service to manage the relationship between different microservices.

### Front End: Component Call -> Application Call

Formally speaking, the routing of the single front-end framework and the single-end back-end application are not much different: **Return the templates of different pages according to different routes**. An Angular examples:

```javascript
const appRoutes: Routes = [
  { path: 'index', component: IndexComponent },
  { path: 'detail/:id', component: DetailComponent },
];
```

And when we micro-service it, it may become the route of application A:

```javascript
const appRoutes: Routes = [
  { path: 'index', component: IndexComponent },
];
```

Plus the route of application B:

```javascript
const appRoutes: Routes = [
  { path: 'detail/:id', component: DetailComponent },
];
```

The key to the problem is: **How to dispatch routes to these different applications**. At the same time, it is also responsible for managing different front-end applications.

Route-dispatch micro-frontends
---

**route-distributed micro-frontends**, which distributes different services** to different, independent front-end applications by routing. It can usually be implemented by a reverse proxy of the HTTP server, or by the routing that comes with the application framework.

For the moment, the micro-frontends architecture through route distribution should be the most popular and easy-to-use "micro-frontends" solution. But this approach looks more like an aggregation of multiple front-end applications, that is, we just put together these different front-end applications to make them look like a complete whole. But they are not, every time a user applies from A to B, they often need to refresh the page.

In a project a few years ago, we were working on a **legacy system rewrite**. We have a migration plan:

1. First, use **static website generation** to dynamically generate the home page
2. Second, refactor the details page using the React stack
3. Finally, replace the search results page

The whole system is not a one-time migration, but a step by step. So when we need to complete the different steps, we need to go online, so we need to use Nginx for route distribution.

The following is an example of a Nginx configuration based on route distribution:

```nginx
http {
  server {
    listen       80;
    server_name  www.phodal.com;
    location /api/ {
      proxy_pass http://http://172.31.25.15:8000/api;
    }
    location /web/admin {
      proxy_pass http://172.31.25.29/web/admin;
    }
    location /web/notifications {
      proxy_pass http://172.31.25.27/web/notifications;
    }
    location / {
      proxy_pass /;
    }
  }
}}
}
```

In this example, requests for different pages are distributed to different servers.

Later, we used a similar approach on other projects, the main reason is: **cross-team collaboration**. When the team reaches a certain size, we have to face this problem. In addition, there is the problem of Angluar cliff-style upgrade. So, in this case, the user foreground uses Angular rewrite, and the background continues to use Angular.js and so on to keep the technology stack. In different scenarios, there are some similar technical decisions.

So in this case it works for the following scenarios:

 - The difference between different technology stacks is relatively large, and it is difficult to be compatible, migrated, and modified.
 - The project does not want to spend a lot of time on the transformation of this system
 - Existing systems will be replaced in the future
 - System functions are perfect, there are no new requirements

In the case of satisfying the above scenario, if it is for a better user experience, it can also be solved by using an iframe.
 
Create a container with iFrame
---

iFrame is a very old technology that everyone feels ordinary, but it has always worked.

> **HTML Inline Framework Elements**  ``<iframe>`` represents a nested context being browsed that effectively embeds another HTML page into the current page.

Iframes can create a completely new, stand-alone hosting environment, which means our front-end applications can run independently of each other. There are several important prerequisites for using an iframe:

 - Website does not require SEO support
 - Have the appropriate **application management mechanism**.

If we are working on an application platform, we will integrate a third-party system in our system, or a system under a number of different department teams. Obviously this is a good solution. Some typical scenarios, such as traditional desktop applications, are migrated to web applications:

![Angular Tabs example](imgs/angular-tabs-example.png)

If this type of application is too complex, then it must be a split for microservices. Therefore, when using an iframe, we need to do two things:

 - Design **Management Application Mechanism**
 - Design **Application Communication Mechanism**

**Load mechanism**. Under what circumstances, we will load and unload these applications; in the process, what kind of animation transition is used to make the user look more natural.

**Communication mechanism**. Creating a ``postMessage`` event directly in each app and listening is not a friendly thing. It's inherently intrusive to the application, so getting the Window object of the iFrame element through ``iframeEl.contentWindow`` is a much simpler approach. Then, you need to **define a set of communication specifications**: what format the event name uses, when to start listening for events, and so on.

Interested readers can look at the micro front-end framework that I wrote before: [Mooa](https://github.com/phodal/mooa).

Either way, iframe is afraid that we will not bring benefit to our KPI this year, so let's build a wheel. :)

Homemade micro-frontends framework 
---

Whether it's Web Components-based Angular or VirtualDOM's React, existing front-end frameworks are inseparable from the basic HTML element DOM.

Well, we only need to:

1. Introduce or create a DOM where appropriate on the page
2. When the user operates, load the corresponding application (trigger the launch of the application) and uninstall the application.

The first problem, creating a DOM is an easy problem to solve. The second problem is not easy at all, especially to remove the monitoring of the DOM and the corresponding application. When we have a different technology stack, we need to design a set of such logic.

Although [Single-SPA](https://github.com/CanopyTax/single-spa) already has startup and uninstallation processing for most frameworks (such as React, Angular, Vue, etc.), it is still not suitable for production. When I designed a micro front-end architecture application for the Angular framework based on Single-SPA, I finally chose to rewrite my own framework, [Mooa](https://github.com/phodal/mooa).

Although the difficulty of getting started in this way is relatively high, it is convenient to order and maintainability later. Regardless of the user experience issues caused by each application being loaded, the only possible risk may be: **third-party libraries are not compatible**.

However, no matter what, compared with iFrame, it is technically more **savvy**, and more interesting. Similarly, similar to iframes, we still face a series of minor problems:

 - Need to design a mechanism to manage the application.
 - For toC applications with high traffic, there will be a lot of requests when loading for the first time.

And we have to split the application again, and want to blabla..., what else can we do?

Combined integration: Widging applications
---

**Combined integration**, which is a step-by-step splitting and recombination of the application in the steps of pre-build, build-time, post-build, etc. by means of **software engineering**.

From this definition point of view, it may not be a micro-frontends - it can satisfy the three elements of the micro-frontends, namely: **independent run**, **independent development**, **independent deploy**. However, with the Lazyload function of the component of the front-end framework - that is, when the required business component or application is loaded, it looks like a micro front-end application.

At the same time, CSS styles don't need to be reloaded because all the dependencies and pollyfill have been loaded as much as possible for the first time.

Common ways are:

 - Build components and applications independently, generate chunk files, build and then categorize the generated chunk files. (This approach is more similar to microservices, but at a higher cost)
 - Develop components or applications independently at development time, merge components and applications when integrating, and finally generate single-body applications.
 - At runtime, load the application's Runtime and then load the corresponding application code and template.

The relationship between the applications is shown in the following figure

![Combined integration comparison](imgs/angular-split-code-compare-en.jpg)

This approach seems quite ideal, that is, to meet the parallel development of multiple teams, but also to build a suitable deliverable.

But first, it has a serious limitation: **must use the same framework**. For most teams, this is not a problem. Teams that use microservices will not use different languages ​​and technologies to develop them because of the front end of microservices. Of course, if you want to use another framework, it is not a problem, we only need to combine the ** homemade framework compatible application in the previous step to meet our needs.

Second, there is a limit to this approach, which is: **specification!** **specification!** **specification!**. In adopting this approach, we need to:

 - Unified dependencies. Keep these dependent versions and add new ones.
 - Specification of the components and routes of the application. Avoid conflicts between different applications because these component names conflict.
 - Build complex. In some scenarios, we need to modify the build system, and in some scenarios we need complex schema scripts.
 - Share common code. This is obviously a problem that we must face frequently.
 - Develop code specifications.

Therefore, this approach looks more like a software engineering problem.

Now, we have four options, each with its own pros and cons. Obviously, combining them would be a more ideal approach.

Taking into account the limitations of existing and commonly used technologies, let us look again in the long run.

Pure Web Components technology build
---

In the process of learning Web Components to develop a micro-frontends architecture, I tried to write my own Web Components framework: [oan](https://github.com/phodal/oan). After adding some basic web front-end framework features, I found this technology to be particularly suitable for **as the cornerstone of the micro-frontends**.

> Web Components is a different set of technologies that allow you to create reusable custom elements (their functionality is packaged outside of your code) and use them in your web applications.

It consists mainly of four technical components:

 - Custom elements, allowing developers to create custom elements such as <today-news></today-news>.
 - Shadow DOM, the shadow DOM, usually attaches the Shadow DOM to the main document DOM and controls its associated functionality. This Shadow DOM cannot be directly controlled by other main document DOMs.
 - HTML templates, the ``<template>`` and ``<slot>`` elements, are used to write markup templates that are not displayed on the page.
 - HTML Imports for introducing custom components.

Each component is introduced by the ``link`` tag:

```
<link rel="import" href="components/di-li.html">
<link rel="import" href="components/d-header.html">
```

Then, in the respective HTML file, create the corresponding component elements and write the corresponding component logic. A typical Web Components application architecture is shown below:

![Web Components Architecture](imgs/web-components-architecture.png)

You can see that this is similar to the way we use iframes above. The components have their own separate ``scripts`` and ``styles``, and the corresponding domain names for the individual deployment components. However, it is not as good as it is supposed to be. It is difficult to build front-end applications directly using **Web Components**:

 - Rewrite existing front-end applications. Yes, now we need to complete the use of Web Components to complete the functionality of the entire system.
 - The upstream and downstream ecosystems are not perfect. There is a lack of support for some third-party controls, which is why jQuery is quite popular.
 - The system architecture is complex. When an application is split into one component after another, communication between components becomes a particularly big problem.

ShadowDOM in Web Components is more like a new generation of front-end DOM containers. Unfortunately, not all browsers can fully support Web Components.

Build with Web Components
---

Web Components are too far away from us, but combining Web Components to build front-end applications is an architecture for future evolution. Or in the future, we can start to build our application in this way. Fortunately, there are already frameworks to create this possibility.

For now, there are two ways to build a micro front-end application with Web Components:

 - Build framework-independent components using Web Components and then introduce them in the corresponding framework
 - Introducing an existing framework in Web Components, similar to the form of an iframe

The former is a component-based approach, or it is like migrating future “legacy systems” to future architectures.

### Integrating existing frameworks in Web Components

Existing Web frameworks already have forms that support Web Components, such as createCustomElement supported by Angular, to implement a component in the form of a Web Component:

```javascript
platformBrowser()
  .bootstrapModuleFactory(MyPopupModuleNgFactory)
    .then(({injector}) => {
      const MyPopupElement = createCustomElement(MyPopup, {injector});
      customElements.define(‘my-popup’, MyPopupElement);
});
```

In the future, there will be more frameworks that can be integrated into the Web Components application using a form like this.

### Web Components integrated into existing frameworks

Alternatively, it is similar to the form of [Stencil](https://github.com/ionic-team/stencil), which builds the component directly into a component in the form of Web Components, and then in the corresponding such as React or Direct reference in Angular.

Here's an example of a Web Component generated by reference to Stencil in React:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

import 'test-components/testcomponents';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

In this case, we can build a framework-independent component.

The same Stencil still only supports recent browsers such as Chrome, Safari, Firefox, Edge and IE11.

Compound type micro-frontends
---

**Composite type**, right in the above categories, just pick several combinations together.

I am not nonsense :)




