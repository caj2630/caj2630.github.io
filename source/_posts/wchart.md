---
uuid: 6
title: WeChat浅聊
categories:
- js
tags:
- 前端
- 微信小程序
---

### 第三方授权操作
![Image text](images/第三方授权1.png)
1.登录微信后台管理系统
2.设置 =》 第三方授权 =》 插件管理添加插件
![Image text](images/第三方授权2.jpg)
3.app.json中添加插件配置
```js
// "provider": "wx6885acbedba59c14" // 插件appid
```
```json
{
  "plugins": {
    "kdPlugin": {
      "version": "1.1.2",
      "provider": "wx6885acbedba59c14" 
    }
  }
}
```
4. 组件中使用
```js
//使用 plugin:// 协议指明插件的引用名和自定义组件名
wx.navigateTo({
  url: "plugin://kdPlugin/index?num=xxx&appName=xxx",
})
```

