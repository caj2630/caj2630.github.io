---
uuid: 6
title: '全屏展示'
date: 2020-11-06
categories:
- 项目扩展
tags:
- 全屏展示大数据
---
### 进入全屏

``` bash
  if (this.$refs.fullStaticPage) {
    if (this.$refs.fullStaticPage.requestFullscreen) {
      this.$refs.fullStaticPage.requestFullscreen()
    } else if (this.$refs.fullStaticPage.webkitRequestFullScreen) {
      this.$refs.fullStaticPage.webkitRequestFullScreen()
    } else if (this.$refs.fullStaticPage.mozRequestFullScreen) {
      this.$refs.fullStaticPage.mozRequestFullScreen()
    } else if (this.$refs.fullStaticPage.msRequestFullscreen) {
      this.$refs.fullStaticPage.msRequestFullscreen()
    }
  }
```

### 退出全屏

``` bash
  if (document.exitFullscreen) {
    document.exitFullscreen()
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen()
  } else if (document.webkitExitFullscreen) {
    document.webkitExitFullscreen()
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen()
  }
```

### 判断是否全屏

``` bash
  checkFull() {
    // var isFull = document.webkitIsFullScreen document.fullscreenEnabled
    var isFull = window.fullScreen || document.webkitIsFullScreen || document.msFullscreenEnabled
    if (isFull === undefined) isFull = false
    return isFull
  }
```
