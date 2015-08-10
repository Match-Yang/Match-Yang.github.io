title: ' Qt Style Sheets--语法 '
date: 2015-08-10 21:12:06
categories: [开发, Qt, 笔记]
tags: [qt, qss, Linux, 前端]
---
[Qt样式表语法英文原文](http://doc.qt.io/qt-4.8/stylesheet-syntax.html)
Qss的术语和语法几乎和HTML CSS相同，如果你已经熟悉CSS，你可以快速的浏览本文。
##样式规则
&emsp;&emsp;样式表由一系列的样式规则组成。一条样式规则由一个选择器和一个声明语句组成，选择器指明了哪个（或者说是哪种）部件将会受***规则***影响，而声明语句则指明了哪些属性会设置到这个（这些）部件.

举个例子：
```css
QPushButton {color: red}
```
&emsp;&emsp;在上面的样式规则中，**QPushBbutton**就是***选择器***，而**{ color： red}**则是***声明语句***。该规则指明了QPushButton和它的子类（如：MyPushButton）应该使用红色作为它们的前景色。Qt样式表通常是不区分大小写的（比如：color, Color, COLOR, 和 cOloR 都是表示同一个属性）。当然也有区分大小写的，类名、对象名、Qt属性名（与color这些属性不是一回事，后面详细解释）这几个都是需要区分大小写的。

&emsp;&emsp;几个选择器可以指定同样的声明语句，使用(,)来隔开不同的选择器，举个例子，如下规则：
```css
QPushButton, QLineEdit, QComboBox { color: red }
```
&emsp;&emsp;与下面三个规则是等价的：
```css
QPushButton { color: red }
QLineEdit { color: red }
QComboBox { color: red }
```
&emsp;&emsp;样式规则的声明语句部分是一个***属性:值***对列表，由(**{}**)包闭，由分号作为分割，举个例子：
```css
QPushButton { color: red; background-color: white }
```
&emsp;&emsp;查看更多由Qt widgets提供的[属性列表](http://doc.qt.io/qt-4.8/stylesheet-reference.html#list-of-properties)（属性名大概看一下就知道什么意思了，有时间再详细说吧）

##选择器类型
&emsp;&emsp;到目前为止，所有例子所使用的都是简单的选择器，即类型选择器。Qt样式表支持所有的[CSS2定义的选择器](http://www.w3.org/TR/REC-CSS2/selector.html#q1)。下表总结了几种最常用的选择器类型。

| **选择器** | **实例** |**描述**|
|--------|--------|------------|
|通用选择器|   *   |匹配所有的widget|
|类型选择器|QPushButton|匹配所有的QPushButton实例和继承于它的子类|
|属性选择器|QPushButton[flat="false"]|匹配所有非flat的QPushButton(通常情况下，使用Q_PROPERTY宏来声明你的属性，比如此例中的**flat**),并且要注意，你的属性类型要受 [QVariant::toString()](http://doc.qt.io/qt-4.8/qvariant.html#toString)支持(查看[toString()](http://doc.qt.io/qt-4.8/qvariant.html#toString)方法的帮助文档以获取更详细的解释).<br>这个选择器类型也可以用来判断动态属性，要了解更多使用自定义动态属性的细节，请参考使用自定义动态属性 。<br>除了使用=，你还可以使用~=来判断一个QStringList中是否包含给定的QString。<br>**警告**：如果在设置了样式表后，相应的属性值发生了改变(如：flat变成了"true")，则有必要重新加载样式表，一个有效的方法是，取消样式表，再重新设置一次,下面的代码是其中一种方式：<br><br>style()->unpolish(this);<br>style()->polish(this);// force a stylesheet recomputation<br>
|类选择器|.QPushButton|匹配所有的QPushButton实例，但不包括它的子类，与*[class~="QPushButton"]是等价的。|
|ID选择器|QPushButton#okButton|匹配所有[object name](http://doc.qt.io/qt-4.8/qobject.html#objectName-prop)为"okButton"的QPushButton实例。|
|后裔选择器|QDialog QPushButton|匹配所有继承于QDialog(包括其所有子孙)的QPushButton实例。|
|子选择器|QDialog > QPushButton|匹配所有直接继承与QDialog的QPushButton实例。|

##Sub-Controls
&emsp;&emsp;为了样式化你的复杂widget，很有必要使用widget的subcontrol，比如QComboBox的**drop-down** 部分或者是QSpinBox的上和下箭头。选择器也许会包含subcontrols用于限制widget的subcontrols，举个例子：
```css
QComboBox::drop-down { image: url(dropdown.png) }
```
&emsp;&emsp;上述规则样式化所有QComboBox的drop-down部分，虽然双冒号(::)让人联想到CSS3的伪元素语法，但是Qt的 Sub-Controls 跟它是不一样的。
- - -
版权声明：本文为博主原创文章，欢迎转载，转载请注明出处