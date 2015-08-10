title: Qt Style Sheets--简介
date: 2015-08-09 23:45:42
categories: [开发, Qt, 笔记]
tags: [qt, qss, Linux, 前端]
---
(本文翻译自Qt SDK帮助手册，水平有限，多多包含，不是完全的直译)
Qt样式表是一个功能强大的机制，它允许您自定义部件的外观，这是除了子类化QStyle来自定义部件外观的另一种机制（其实Qt样式表跟QStyle有很大的关系，以后慢慢了解）。
Qt样式表的概念、术语、语法都深受HTML启发Cascading Style Sheets (CSS)但是却适合于widgets的世界。
- - -
##专题
- 概述
- Qt设计师集成开发工具（以后不对这个模块做描述，大家有兴趣的自己看看帮助文档）
- 使用样式表自定义Qt Widgets
- Qt样式表标准
- Qt样式表例子
- - -
##概述
&emsp;&emsp;样式表是一种能使用 **QApplication::setStyleSheet()**设置到整个应用程序或者使用**QWidget::setStyleSheet()**设置到某个特定的部件（包括其子部件）的文本规范。如果在几个不同的级别上设置了不同的样式表，Qt会同时应用这几个级别上设置的所有样式表，这就是所谓的级联。
&emsp;&emsp;举个例子，下面的样式表语句规定所有的**QLineEdit**都应该将黄色作为它们的背景样色，同样的，所有的**QCheckBox**都应该将红色作为该部件上字体的颜色:

```css
QLineEdit { background: yellow }
QCheckBox { color: red }
```

&emsp;&emsp;对于这种定制，样式表比**QPalette**要强大得多。举个例子，通过设置**QPalette::Button**的**role**成红色以获得一个红色的按钮看似非常有诱惑力，然而,这并不能保证适用于所有风格,因为样式作者会受限于不同平台的指导方针和(在 Windows XP 和 Mac OS X)本地的主题引擎。
&emsp;&emsp;此外，样式表可以在你不子类化**QStyle**的情况下也能为你的应用程序提供独特的观感。举个例子，你可以为单选框和复选框指定任意的图片以使它们更加的突出。使用这种技术，还可以实现更小的定制，但这通常需要子类化一下style类，比如style hint。下面展示的两个独特的样式表，你可以尝试和修改。

![](http://ww2.sinaimg.cn/large/ec07ab8bgw1euwtmsduhij20or0dj427.jpg)
![](http://ww2.sinaimg.cn/large/ec07ab8bgw1euwtrjhm8aj20oo0dgwhc.jpg)

&emsp;&emsp;样式表让你可以执行任何**QPalette**难以做到甚至不能做到的各种自定义操作。 如果你想让必填字段显示黄色的背景色、让退出按钮等破坏性的按钮显示红色的文字有或者想弄个花里胡哨的复选框，那么样式表都能满足你。

&emsp;&emsp;样式表应用在所有部件的顶层，这意味着你的应用程序看起来会像本地的样式，但是任何样式表定义都会被考虑。与palette那种不靠谱相反，样式表能保证：如果你把**QPushButton**的背景色设置成了红色，你可以确定在所有平台和主题下它的背景颜色都是红色的。此外，Qt 设计师工具提供了样式表的集成，可以更直观的看到样式表的效果。

&emsp;&emsp;当样式表激活时，由**QWidget::style()**返回的QStyle其实是“样式表”的包装而不是平台指定的样式。“样式表”的包装保证样式表会优先使用，否则在进行绘制操作时，平台指定的样式将会生效。（例子：Windows XP上使用QWindowsXPStyle）
- - -
版权声明：本文为博主原创文章，欢迎转载，转载请注明出处