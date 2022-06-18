# 初始化CSS

### 原因

不同浏览器对各个元素的 `margin`、`padding`、`border`、`font` 等属性的定义各有不同，不同浏览器的显示效果可能不一样，为杜绝这种情况，通过 CSS 强制让所有元素的属性值都一样，这样浏览器显示就能一致，减少了不兼容情况的发生。

### 初始化

每次新开发网站或新网页时候通过初始化 CSS 样式的属性，为我们将用到的 CSS 或 HTML 标签更加方便准确，使得我们开发网页内容时更加方便简洁，同时减少 CSS 代码量，节约网页下载时间。
最简单的初始化方法就是： `* {padding: 0; margin: 0;}`。通配符 `*` 在编写代码的时候很快，但如果网站很大，CSS 样式表文件很大，通配符选择器会把所有的标签都初始化一遍，从而加大了网站运行的负载，会使网站加载的时候需要很长时间。

### 常用初始化CSS

```css
/* 去除外边距和内边距 */
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,legend,button，form,fieldset,input,textarea,p,blockquote,th,td {   
　　padding: 0;   
　　margin: 0;   
}

/* 根据要求进行修改 */
body {
    font-size: 12px;
    font-family: "SimSun", "宋体", "Arial Narrow";
}

/* 缩写，图片等无边框 */
fieldset, img, abbr, acronym {border: 0 none;}
abbr, acronym {font-variant: normal;}
legend {color: #000;}

/* 清除特殊标记的字体和字号 */
address,caption,cite,code,dfn,em,strong,th,var {   
　　font-weight: normal;   
　　font-style: normal;   
}

/* 上下标进行垂直对齐 */
sup {vertical-align: text-top;}
sub {vertical-align: text-bottom;}

/* 设置表格的边框被合并为一个单一的边框, 指定分隔边框模型中单元格边界之间的距离为0*/
table {   
　　border-collapse: collapse;   
　　border-spacing: 0;   
}   

/* 表格标题及内容居左显示 */
caption, th {text-align: left;}
input, img, select {vertical-align: middle;}

/* 清除列表样式 */
ol,ul {list-style: none;}  

/* 输入控件字体 */
input,button,textarea,select,optgroup,option {
    font-family: inherit;
    font-size: inherit;
    font-style: inherit;
    font-weight: inherit;
}

/* 标题元素样式清除 */ 
h1,h2,h3,h4,h5,h6 {   
　 font-weight: normal;   
　 font-size: 100%;   
}   

/* 链接样式，颜色可根据要求修改 */
del,ins,a {text-decoration: none;}
a:link {color: blue;}
a:visited {color: gray;}
a:hover, a:active, a:focus {
    color: coral; 
    text-decoration: underline;
} 

/* 鼠标样式 */
input[type="submit"] {cursor: pointer;}
button {cursor: pointer;}
input::-moz-focus-inner { border: 0; padding: 0;}

/* 清除浮动 */
.clear {clear: both;}
.clearfix{ 
    *zoom:1; /* IE/7/6 */ 
} 
```

除了对 HTML 原有的样式清除以外，我们还可以设置一些固定类名，在需要的时候进行调用，比如：

```css
.fl {
    float:left
}

.fr {
    float:right
}

.ac {
    text-align:center
}

.hide {
    display:none
}
```

