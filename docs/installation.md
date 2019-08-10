## 开始

### 安装
```bash
npm install vue-analytics
```

Start using it your Vue application
```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X'
})
```

**Important**

For all the ES5 users out there, this package uses a default export so if you want to use `require` instead of `import` you should import the plugin like this

```js
const VueAnalytics = require('vue-analytics').default

Vue.use(VueAnalytics, { ... })
```

### 使用
可以通过两种不同的方式使用api：
 - 在组件范围内
 - 单独导入方法

#### 组件作用域

```js
export default {
  name: 'MyComponent',

  methods: {
    track () {
      this.$ga.page('/')
    }
  }
}
```

#### 导入方法

为了能够使用方法导入，请确保在要使用它们之前安装vue-analytics；

```js
import { page } from 'vue-analytics'

export default {
  name: 'MyComponent',

  methods: {
    track () {
      page('/')
    }
  }
}
```

#### API
- event
- ecommerce
- set
- page
- query
- screenview
- time
- require
- exception
- social

## 跟踪（Track）多个帐户

为多个跟踪系统传递一串字符串数组。
每次点击都会被触发两次：每次都有不同的跟踪器名称

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: ['UA-XXX-A', 'UA-XXX-B']
})
```

## Use functions or/and Promises

也可以传递一个函数，一个Promise或一个返回Promise的函数：只要它总是返回一个字符串或一个字符串数组

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'
import axios from 'axios'

// a function
Vue.use(VueAnalytics, {
  id () {
    return 'UA-XXX-A'
  }
})

// a Promise
Vue.use(VueAnalytics, {
  id: axios.get('/api/foo').then(response => {
    return response.data
  })
})

// a function that returns a Promise
Vue.use(VueAnalytics, {
  id: () => axios.get('/api/foo').then(response => {
    return response.data
  })
})
```
