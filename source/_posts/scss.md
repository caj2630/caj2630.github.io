---
uuid: 6
title: 'SASS sass'
date: 2021-11-08
categories:
- 样式
- sass
tags:
- sass
---

>在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能

命名规范：BEM（模块名_元素名--修饰器）block-name__element-name--modifier-name
定义：$width: 20;
     $height: 40;
使用：width: #{$width}px
     border: #{$width / $height}px;

```bash
# 根节点
$name: "v";
$root-separator: "-";
$element-separator: "__";


@mixin block($block) {
  $NAME: $name + $root-separator + #{$block} !global;
  .#{$NAME}{
    @content
  }

}
# 嵌套子节点
@mixin element($element){
  $currentElement: '';

  @each $unit in $element{
    $currentElement: #{$currentElement + '.' + $NAME + $element-separator + $unit + ','}
  }

  @at-root {
    #{$currentElement}{
      @content
    }
  }

}

# 伪元素
@mixin pseudo($pseudo){
  @at-root #{&}#{':'#{$pseudo}} {
    @content
  }
}

```
