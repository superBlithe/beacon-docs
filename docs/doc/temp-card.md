# 集卡模板
模板主要包含以下六个模块，最好配合需求文档看
  - [mvp模块](#mvp模块)
  - [次数校验组件](#次数校验组件)
  - [消耗卡片发奖](#消耗卡片发奖组件)
  - [邀请助力组件](#邀请助力组件)
  - [赠卡组件](#赠卡组件)
  - [索卡组件](#索卡组件)

## 需求相关
  - [需求文档](http://cf.dui88.com/pages/viewpage.action?pageId=48340358)
  - [技术方案](http://cf.dui88.com/pages/viewpage.action?pageId=48349829)
  - [接口文档](http://ams.dui88.com/#/home/project/inside/api/list?groupID=607&childGroupID=1110&projectName=%E5%85%91%E5%90%A7&projectID=3)
  - [前端接口梳理](//yun.duiba.com.cn/h5/activity_custom/skins/yd-cq_191231/集卡模板.jpg)


## 注意事项
  - `coop_components`接口会返回所有开启的组件
  - 投放链接url记得加参数`channelType`， 1：app    2：微信
  - 进入页面有可能同时出现多种弹窗，具体查看[弹窗梯队管理](#弹窗队列管理（自定义事件）)
  - 页面高度会根据`是否开启邀请助力组件`和`邀请人奖励是随机出卡`来变化
  - 登录/关注 抽卡、消耗卡片发奖、助力、赠卡组件-领卡、索卡组件-送卡5个校验点，来触发自定义事件[loginGuide](#引导登录or关注loginGuide（自定义事件）)
   1. 请求前：拦截会根据cookie `isNotLoginUser = true`, 为未登录/未关注 (微信不生效，所以接口层也会拦截)
   2. 接口后：根据异常码  `errorCode === 400001` 为未登录/未关注
  - 助力、送卡、索卡 url传参都是`sharecode`，前端额外增加的参数`sharetype`来区分 send送卡；ask索卡; help 助力
  - 抽卡和消耗卡片兑换的大奖这俩是用的同一个弹窗，`弹窗-获得奖品：激活`的事件会进行区分
  - 中间页只做跳转无逻辑
---
## 弹窗队列管理（自定义事件）
> 用来控制进页面自动出现弹窗的梯队管理，每个tx可以push多个任务。
  * `t0`：助力弹窗、领卡弹窗、送卡弹窗
  * `t1`：分享者：期间多少好友给自己助力弹窗
  * `t2`：期间新增领取成功的卡片数量提示

> 在首次渲染卡事件以及其他需要的地方（多为t0,t1弹窗按钮）进行：
  - 触发管理中心 `engine.globalEvent.dispatchEvent('modalLevelController');`
  - 管理中心派发对应handle `engine.globalEvent.dispatchEvent(`modalCtr_${item}_handle`);`
---
## 引导登录or关注loginGuide（自定义事件）
 - 所有的登录拦截都会指向这个事件
 - 会根据`channelType` 来触发相应的弹窗或提示
---
## mvp模块
  - mvp模块无次数限制，可无限抽奖
  - 抽卡相关的弹窗：`再来一次按钮`  是会直接进行下一次抽奖，其他皆为关闭弹窗
  - 卡片是根据查询接口来动态获取的，如果是未登录会调用公用接口`clcard/coop_getCards.do`来获取
  - 卡片布局在`数据中心：渲染卡片` let posArr = [[0, 0], [239, 0], [479, 0], [0, 320], [239, 320], [479, 320]];
---
## 次数校验组件
  - 对应字段为JoinTimesCheckComponent
  - 会在`首页init` 组件判断是否渲染抽卡按钮是否显示次数
  - 自定义事件 `抽卡事件` 会进行相关的拦截
---
## 消耗卡片发奖组件
  - 对应字段为SendPrizeCostSpComponent
  - 模板需求`抽卡按钮`和`兑换大奖`按钮是互斥的，可在数据中心`是否开大奖`里修改
  - `clcard/getPrizeCondition.do`会返回目前满足的开奖规则，默认去兑换`第一`条规则
---
## 邀请助力组件
  - 对应字段为AssistComponent
  - 邀请助力按钮是显示控制在`首页init`事件，组件过滤处
  - `被助力者`：在进入页面时会触发t1弹窗控制，`期间多少人给自己助力`，如果prizeIds.length > 0 则是弹窗(弹窗图片默认取第一个)，否则是toast
  - `助力者`：点击助力按钮会进行登录拦截，助力接口会直接发奖， `rewardType`: 奖励类型： 0-无 1-次数 2-奖品
---
## 赠卡组件
  - 对应字段GiveCardComponent
  - `开启了组件`，且此卡`卡片数量大于零`，会展示赠卡按钮。在数据中心-渲染卡来绑定自定义事件`send-card`
  - `赠送者`：在进入页面时会触发t2弹窗控制，`期间被领了多少卡`，是toast
  - `接收者`：进入页面触发t0弹窗，领取卡片会校验登录
---
## 索卡组件
  - 对应字段RequestCardComponent
  - `开启了组件`，且此卡`卡片数量<=0`，会展示索卡按钮。在数据中心-渲染卡来绑定自定义事件`require-card`
  - `索卡接收者`：进入页面触发t0弹窗，赠送卡片
