---
uuid: 4
title: 页面性能优化
---
## 浏览器部分 代码部分

  一、影响因素：
    1、网络层面：过多的http请求、资源访问带宽小、网页元素视频图片样式等太大
    2、浏览器渲染层面：渲染阻塞、重复渲染、dns解析
    3、服务端层面（构建层面、编码层面、机制规范等）：服务器软件（防火墙）、未对nginx等服务器进行配置优化、cpu占满、数据库未优化、代码问题、性能效率、过多的分析类工具
  二、浏览器组成：
    1、用户界面（地址栏、书签栏）
    2、浏览器引擎（用户界面和渲染引擎之间的传送指令）
    3、渲染引擎（负责显示请求内容）
    4、网络（网络调用）
    5、用户界面后端（绘制基本的窗口小部件）
    6、js解析器（v8引擎）
    7、数据存（cookie）
  三、重排和重绘：重排比重绘成本高
    重排：重新计算页面布局
    重绘：样式属性变化
  四、优化原则：
    1、减少http请求
    2、cdn链接
    3、避免src或href空值
    4、gizp组件
    5、css放顶部，js放底部
    6、减少nds查找
    7、压缩资源
    8、避免3xx或4xx
    9、ajax优化（能用get尽量get，post有2次请求，对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据））
    10、cookie优化
    11、利用缓存
    12、缩短服务器响应时间
  五、vue性能优化：
    1、活用v-show，减少v-if使用
    2、使用keep-alive
    3、活用延迟装载（defer） mixins/defer  eg：const Login = () => import("./login")
    4、分批处理（分批请求接口）
    5、非响应模式
    6、仅渲染可视化部分
    7、函数型组件
    8、子组件拆分
    9、局部变量
  六、vue首屏优化：
    渲染过程：
    == html  ```<div id="app"></div>``` FP:html页面加载下来（first paint）
      == 静态资源、css、js
        == 解析js => 生成html FCP: css、js资源加载下来（first content paint）
          == ajax FMP: ajax数据请求回来（first meaning paint）
    处理方案：
      1.路由动态加载
      2.ssr、预渲染、同构
        ssr: 服务端渲染 （请求=>node（执行的vue项目）=> 将实例转出html => 吐给客户端）最大问题扛不住并发
        预渲染: webpack vue打包出来的项目 => 以某些方式到无头浏览器执行html => 拿到html（获取到预渲染的页面的html内容）=> 放到index.html文件 => cnd
        同构(nuxt): 一套代码多端使用 vue打包 => json => vue-server-render => html
      3. loading、骨架屏、预渲染
      4. webpack entry打包 单页打包为多页
      5. 资源请求时间片的处理（webpack可配置）
      6. cdn
      7. quicklink：prefetch、preload、preconnect
