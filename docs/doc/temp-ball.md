# 打星球
[cf打星球前端文档](http://cf.dui88.com/pages/viewpage.action?pageId=48342605)
## 需求相关
  - [需求文档](http://cf.dui88.com/pages/viewpage.action?pageId=41231557)
  - [技术方案](http://cf.dui88.com/pages/viewpage.action?pageId=41249751)
  - [接口文档](http://ams.dui88.com/#/home/project/inside/api/list?groupID=971&childGroupID=972&projectName=ProjectX&projectID=130)

## 首页
  - **添加登录代码** 涉及接口 `/aaw/projectx/getDevAppInfo`
  - **更新游戏次数** 文案涉及接口 `coring/index.do`
  - **获取是否有排行榜奖励** `scoring/getPrize.do`
  - [弹出规则](http://cf.dui88.com/pages/viewpage.action?pageId=46351125)
  - **开始游戏**
    1. 校验是否登录
    2. 校验活动是否结束
    3. 开始一次游戏接口 `scoring/start.do`
  - 自动弹出`签到弹窗` 控制在 首页-自动弹出签到
---
> 签到弹窗
  - 获取签到信息 `sign/getSignInfo.do`
  - 如果没有签到过会`自动弹出`，直接执行【签到点击】封装，转到【首页 - 签到】

> 首页 - 分享得道具
  - 1、判断是否有分享次数 `scoring/index.do`
  - 2、分享获得奖励玩法 `share/join.do`

> 首页 - 签到
  - 拼接聚合查询参数 这里需要两个接口，时间可能会长。`merge.query post {"querys" : "{}”}`

> 弹窗-签到
  - 1、签到信息展示
  - 2、签到之后要重新发`sign/getSignInfo.do`，为了更新签到信息。点击入口的时候没有发。
  - 3、签到接口 签到只要一个接口 `sign/join.do`  不需要prize.query get {"ids":"12”}

> 弹窗-排行榜
  - 1、本期/上期
  - 2、获取我的本期和上期的信息scoring/ranking.do get {}
  - 3、奖品滚动列表
  - 4、选项卡功能
  - 5、渲染排行信息（我的+列表）