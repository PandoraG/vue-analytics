## 网页跟踪

网页跟踪是Google Analytics最重要的功能，您可以通过不同方式实现这一目标。

### 启用页面自动跟踪

跟踪应用程序最简单的方法是将VueRouter实例传递给插件，让它为您处理所有事情。

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import VueAnalytics from 'vue-analytics'

const router = new VueRouter({
  router: // your routes
})

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router
})
```

### 手动页面跟踪

标准方法是传递当前页面路径。

```js
this.$ga.page('/')
```

传递一个对象字面量。

```js
this.$ga.page({
  page: '/',
  title: 'Home page',
  location: window.location.href
})
```

或者您甚至可以传递组件中的VueRouter实例，插件将自动检测当前路由名称，路径和位置：只需确保在路由对象中添加`name`属性。

```js
this.$ga.page(this.$router)
```

Google Analytics docs: [page tracking](https://developers.google.com/analytics/devguides/collection/analyticsjs/pages)


### 使用screenview进行自动跟踪

通过将true传递给 `autoTracking` 对象中的 `screenview` 属性，也可以使用自动跟踪和屏幕跟踪。

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  autoTracking: {
    screenview: true
  }
})
```

### Disable pageview hit on page load

页面自动跟踪在页面加载时发送网页浏览事件，但可以禁用它。

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import VueAnalytics from 'vue-analytics'

const router = new VueRouter({
  router: // your routes
})

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    pageviewOnLoad: false
  }
})
```

### 禁用页面自动跟踪

要禁用自动跟踪，我们只需删除VueRouter实例，但如果您只需要在特定环境或情况下进行跟踪，也可以禁用页面自动跟踪。

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    page: false
  }
})
```

### Ignore routes on page auto tracking

要禁用特定路由的自动跟踪，您需要将一个字符串数组传递给插件选项。
字符串需要是路由`name`或路由`path`。

```js
Vue.use(VueAnalytics, {
  router,
  ignoreRoutes: ['home', '/contacts']
})
```

### Auto track with custom data

当自动跟踪可以传递具有自定义对象形状的功能以用作跟踪器时。
`pageViewTemplate`将当前路由作为参数传递

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    pageviewTemplate (route) {
      return {
        page: route.path,
        title: document.title,
        location: window.location.href
      }
    }
  }
})
```

还可以使用元对象为每个路径添加自定义数据结构。

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'
import VueRouter from 'vue-router'

const router = new VueRouter({
  routes: [
    {
      name: 'home',
      path: '/',
      component: {...},
      meta: {
        analytics: {
          pageviewTemplate (route) {
            return {
              title: 'This is my custom title',
              page: route.path,
              location: 'www.mydomain.com'
            }
          }
        }
      }
    }
  ]
})

```
important: the route pageviewTemplate has always priority over the global one.

`pageviewTemplate` can return a falsy value to skip tracking, which can be useful for specific needs:

- `shouldRouterUpdate` documented below is more appropriate for tracking control based on routing, but is not enough when you need to disable initial tracking on some pages, since it only applies to navigation after initial page load.
- `pageviewOnLoad: false` is global and can’t depend on current route.

## Avoid transforming route query object into querystring
It is possible to avoid route query to be sent as querystring using the `transformQueryString` property

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    transformQueryString: false
  }
})
```

## Remove vue-router base option
When a base path is added to the VueRouter instance, the path is merged to the actual router path during the automatic tracking: however it is still possible to remove this behaviour modifying the `prependBase` property in the configuration object

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    prependBase: false
  }
})
```

## Customize router updates
On every route change, the plugin will track the new route: when we change hashes, query strings or other parameters.

To avoid router to update and start tracking when a route has the same path of the previous one, it is possible to use the `skipSamePath` property in the `autoTracking` object

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    skipSamePath: true
  }
})
```

For other use cases it is also possible to use the `shouldRouterUpdate`, accessible in the plugin configuration object, inside the `autoTracking` property.
The methods has the previous and current route as parameters and it needs to return a truthy or falsy value.

```js
Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  router,
  autoTracking: {
    shouldRouterUpdate (to, from) {
      // Here I'm allowing tracking only when
      // next route path is not the same as the previous
      return to.path !== from.path
    }
  }
})
```
