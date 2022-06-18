# text-decoration

设置文本的修饰线外观。它是 `text-decoration-line`、`text-decoration-color`、`text-decoration-style` 和新出现的 `text-decoration-thickness` 属性的缩写。

## text-decoration 的属性及取值

| 属性                      | 作用                                                         |
| :------------------------ | :----------------------------------------------------------- |
| text-decoration-line      | 文本修饰的位置, 如下划线 `underline`，删除线 `line-through`  |
| text-decoration-color     | 文本修饰的颜色                                               |
| text-decoration-style     | 文本修饰的样式, 如波浪线 `wavy`，实线 `solid`，虚线 `dashed` |
| text-decoration-thickness | 文本修饰线的粗细                                             |

**text-decoration-line**

| 取值         | 作用                       |
| :----------- | :------------------------- |
| none         | 表示没有文本修饰效果       |
| underline    | 在文本的下方有一条修饰线   |
| overline     | 在文本的上方有一条修饰线   |
| line-through | 有一条贯穿文本中间的修饰线 |

**text-decoration-color**

| 取值  | 作用             |
| :---- | :--------------- |
| color | 修饰文本线的颜色 |

**text-decoration-style**

| 取值   | 作用   |
| :----- | :----- |
| solid  | 实线   |
| double | 双实线 |
| dotted | 点划线 |
| dashed | 虚线   |
| wavy   | 波浪线 |

**text-decoration-thickness**

| 取值      | 作用                                                         |
| :-------- | :----------------------------------------------------------- |
| auto      | 由浏览器为文本装饰线选择合适的厚度                           |
| from-font | 如果字体文件中包含了首选的厚度值，则使用字体文件的厚度值。如果字体文件中没有包含首选的厚度值，则效果和设置为 `auto` 一样，由浏览器选择合适的厚度值 |
| 数值长度  | 将文本装饰线的厚度设置为一个 length 类型的值，覆盖掉字体文件建议的值或浏览器默认的值 |
| 百分比    | 指定文本装饰线的厚度为当前字体中 `1em` 的百分比。百分比作为一个相对值来继承，因此会随着字体的变化而变化。浏览器必须使用最小的1设备像素。对于这个属性的一个给定的应用，厚度在它所应用的整个盒子里是恒定的，即使有不同字体大小的子元素 |

