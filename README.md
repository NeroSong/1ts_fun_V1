## 对主题的自定义修改
修改cover features的颜色，它的高亮也一样

1. 在/themes/volantis/source/css/_layout/cover.styl中更改们menu中的所有相关的color为color-site-features

2. 在/themes/volantis/source/css/_defines/color.styl中添加common中features选项


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

底部footer间距