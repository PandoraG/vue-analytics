## Event tracking

Event tracking can be achieved in different ways, following Google specifications
可以按照Google规范以不同方式实现事件跟踪

passing parameters in this exact order
以这个确切的顺序传递参数

```js
this.$ga.event('category', 'action', 'label', 123)
```

an object literal is also possible
对象文字也是可能的

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



