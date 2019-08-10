## Event tracking



可以按照Google规范以不同方式实现事件跟踪


以这个确切的顺序传递参数

```js
this.$ga.event('category', 'action', 'label', 123)
```

也可能是一个对象字面量。

```js
this.$ga.event({
  eventCategory: 'category',
  eventAction: 'action',
  eventLabel: 'label',
  eventValue: 123
})
```

Google Analytics docs: [event tracking](https://developers.google.com/analytics/devguides/collection/analyticsjs/events)

## 



