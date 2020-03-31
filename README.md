# 个人博客

## 说明

使用了 Hexo 框架，主题为 [Volantis](https://volantis.js.org/)。

CDN 加速使用 GitHub + jsdelivr 实现。

部分优化参见[博客重构迁移记录](https://1ts.fun/2020/03/19/%E5%8D%9A%E5%AE%A2%E9%87%8D%E6%9E%84%E8%BF%81%E7%A7%BB%E8%AE%B0%E5%BD%95/)。


## 对主题的自定义修改

修改 cover features 的颜色，它的高亮也一样

1. 在`/themes/volantis/source/css/_layout/cover.styl`中更改 menu 中的所有相关的 color 为 color-site-features

2. 在`/themes/volantis/source/css/_defines/color.styl`中添加 common 中 features 选项


设置选中后字符颜色和背景色

```
::selection { 
    background: #00c4b6;
    color: #f7f7f7; 
}
/* firefox */
::-moz-selection { 
    background: #00c4b6;
    color: #f7f7f7;    
}
```

底部footer间距减小
