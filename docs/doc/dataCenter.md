# 数据中心
  > 单向数据流，数据驱动事件&视图
  - 举个栗子：`集卡`：卡片的数量、赠卡索卡等信息展示，会有`多个触发更新`的点（页面首次加载、送卡、索卡、抽卡、收卡等等）都需要重新去请求更新卡片数据。 如果在~~*每个动作都去执行一系列的逻辑处理*~~，明显是不合理的。我们可以利用数据中心拆分成以下模块  
  
    - 触发点只需要`触发更新数据`（不关心数据变更新后的操作）
    - 数据中心去统一`订阅`某个数据的变更，然后`单个出口`来处理后续逻辑  
  - 直接上图
  - 数据中心 注：请务必把`数据中心筛选事件`加上，不然其他数据的变化也会触发
  ![](//yun.duiba.com.cn/h5/activity/SUPER/数据中心)

  - 在需要更新的地方直接触发，主要数据名和数据中心一致  
  ![](//yun.duiba.com.cn/h5/activity/SUPER/数据中心触发点)

  > 数据也可以通过`global.dataCenter.store.xxx`来处理