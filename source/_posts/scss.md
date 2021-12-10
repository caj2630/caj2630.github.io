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
