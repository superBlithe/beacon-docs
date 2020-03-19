# 自定义事件
  > **个人理解**: 可以把一些业务逻辑单独拆出来，特别是需要复用的事件，降低耦合
  - 举个栗子：`还是集卡`：
    - 抽卡事件：抽卡不仅仅是在点击抽卡按钮会触发，有些弹窗的再来一次也要触发，所以封装成了自定义事件。
    - 登录or关注引导拦截： 因为有助力、抽卡、送卡等大量地方需要拦截，但是拦截后的处理逻辑都是一样得，所以这里封装成了自定义事件  


  > 你可以把这玩意当成一个代码块，需要的地方执行以下，也可以灵活组合嵌套使用
  - `添加自定义事件`  
  ![](//yun.duiba.com.cn/h5/activity/SUPER/自定义事件)
  - `触发自定义事件`  比如一个抽卡按钮的点击事件，只放了以下代码  
 ```
  if (window['isNotLogin']) {
      engine.globalEvent.dispatchEvent('loginGuide');
  } else {
      engine.globalEvent.dispatchEvent('cardDoJoin');
  }
```