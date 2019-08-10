## 如何加载Google Analytics脚本

**默认情况下，不需要任何东西：插件为您完成所有操作!**

但是，在某些情况下，您可能希望避免自动加载analytics.js脚本，因为：
 - 也许你正在使用的框架已经为你做了
 - 你真的无法从你的项目中删除它
 - 我无法想出的其他问题

因此，对于所有这些情况，可以让插件检测是否已在html中添加了分析脚本

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  checkDuplicatedScript: true
})
```

或者只是禁用脚本加载器

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: 'UA-XXX-X',
  disableScriptLoader: true
})
```

### Important
可能插件无法删除初始跟踪器，因为它需要它们为多跟踪功能创建所有方法

**如果您无法删除初始跟踪器，请不要使用此插件：结果可能无法预测.**
