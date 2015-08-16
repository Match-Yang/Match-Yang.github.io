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

&emsp;&emsp;Sub-Controls始终相对于另一个元素来定位--一个参考元素。这个参考元素可以是一个Widget又或者是另一个Sub-Control。举个例子，QComboBox的[::drop-down](http://doc.qt.io/qt-4.8/stylesheet-reference.html#drop-down-sub)默认被放置于QComboBox的Padding rectangle([盒子模型](http://doc.qt.io/qt-4.8/stylesheet-customizing.html#the-box-model))的右上角。::drop-down默认会被放置于另一个::drop-down Sub-Control的中心。查看[可样式化的Widget列表](http://doc.qt.io/qt-4.8/stylesheet-reference.html#list-of-stylable-widgets)以了解更多使用Sub-Control来样式化Widget和初始化其位置的内容。

&emsp;&emsp;源rectangle可以使用[subcontrol-origin](http://doc.qt.io/qt-4.8/stylesheet-reference.html#subcontrol-origin-prop)来改变。举个例子，如果我们想要把drop-down放置于QComboBox的margin rectangle而不是默认的Padding rectangle，我们可以像下面这样指定：
```css
QComboBox {
    margin-right: 20px;
}
QComboBox::drop-down {
    subcontrol-origin: margin;
}
```
&emsp;&emsp;drop-down在Margin rectangle内的排列方式可以由[subcontrol-position](http://doc.qt.io/qt-4.8/stylesheet-reference.html#subcontrol-position-prop)来改变.
&emsp;&emsp;[width](http://doc.qt.io/qt-4.8/stylesheet-reference.html#width-prop)和[height](http://doc.qt.io/qt-4.8/stylesheet-reference.html#height-prop)属性可以用来控制Sub-control的size.需要注意的是，设置了[image](http://doc.qt.io/qt-4.8/stylesheet-reference.html#image-prop)就隐式的设置了Sub-control的size了。

&emsp;&emsp;相对定位方案（position:relative）,允许Sub-Control的位置从它的初始化位置作出偏移。举个例子，当QComboBox的drop-down按钮被pressed时，我们也许想要那个箭头作出位移以显示一种“pressed”的效果，为了达到目标，我们可以像下面那样指定：
```css
QComboBox::down-arrow {
    image: url(down_arrow.png);
}
QComboBox::down-arrow:pressed {
    position: relative;
    top: 1px; left: 1px;
}
```
&emsp;&emsp;绝对定位方案(position : absolute)，使得Sub-control的position和size基于其参考元素而改变。

&emsp;&emsp;一旦定位，它们将会与widget同等对待并且可以使用[盒子模型](http://doc.qt.io/qt-4.8/stylesheet-customizing.html#box-model)来样式化。

&emsp;&emsp;查看[Sub-Control列表](http://doc.qt.io/qt-4.8/stylesheet-reference.html#list-of-sub-controls)以了解那些sub-control是被支持的，并且可以查看[自定义QPushButton的菜单指示器Sub-Control](http://doc.qt.io/qt-4.8/stylesheet-examples.html#customizing-the-qpushbutton-s-menu-indicator-sub-control)来了解一个实际的使用例子。

&emsp;&emsp;**注意**：像QComboBox和QScrollBar这样的复杂部件，如果sub-control的一项属性是自定义的，那么其他所有的属性跟sub-control也都应该自定义。

##伪状态
&emsp;&emsp;选择器也许会包含基于widget的state的程序限制规则的伪状态。伪状态以冒号(:)作为分隔紧跟着选择器。举个例子，下面的规则在鼠标悬浮在QPushButton的上方时生效：
```css
QPushButton:hover { color: white }
```
&emsp;&emsp;伪状态可以使用感叹号进行取反，下面一条规则在鼠标没有悬浮在QRadioButton上方时生效：
```css
QRadioButton:!hover { color: red }
```
&emsp;&emsp;伪状态可以链接，在这样的情况下，隐式地包含了**逻辑与**。举个例子，下面一条规则在鼠标悬浮到一个已check的QCheckBox上时生效：
```css
QCheckBox:hover:checked { color: white }
```
&emsp;&emsp;伪状态的**取反**也可以出现在伪状态链中，举个例子，下面的规则在鼠标悬浮到一个没有被press的QPushButton上时生效：
```css
QPushButton:hover:!pressed { color: blue; }
```
&emsp;&emsp;如果有需要，可以使用逗号来表示**逻辑或**:
```css
QCheckBox:hover, QCheckBox:checked { color: white }
```
&emsp;&emsp;伪状态可以与subcontrol组合使用，举个例子：
```css
QComboBox::drop-down:hover { image: url(dropdown_bright.png) }
```
查看由Qt widgets提供的[伪状态列表](http://doc.qt.io/qt-4.8/stylesheet-reference.html#list-of-pseudo-states)章节

##解决冲突
&emsp;&emsp;当几个样式规则为同一个属性指定不同的值时，就产生了冲突。请考虑下面的样式表：
```css
QPushButton#okButton { color: gray }
QPushButton { color: red }
```
&emsp;&emsp;两条规则都匹配名为okButton的QPushButton实例并且冲突于颜色属性。为了解决冲突，我们必须考虑到选择器的特殊性。在上面的例子中，QPushButton#okButton被视为比QPushButton更特殊，因为它（通常）指向一个单一的对象而不是QPushButton的所有实例。

&emsp;&emsp;相似的，指定了伪状态的选择器比没有指定伪状态的更特殊。从而，下面的样式表指明了当鼠标悬浮到QPushButton上方时其字体颜色应该为白色，而其余情况则为红色：
```css
QPushButton:hover { color: white }
QPushButton { color: red }
```
&emsp;&emsp;接下来看一个很有意思的：
```css
QPushButton:hover { color: white }
QPushButton:enabled { color: red }
```
&emsp;&emsp;两个选择器都有相同的特殊性，所以当鼠标悬浮在一个enabled的按钮上时，第二条规则优先。如果在这种情况下我们想要文字变成白色，我们可以像下面那样重新排布一下样式规则：
```css
QPushButton:enabled { color: red }
QPushButton:hover { color: white }
```
&emsp;&emsp;另外，我们可以使第一条规则更特殊一些：
```css
QPushButton:hover:enabled { color: white }
QPushButton:enabled { color: red }
```
&emsp;&emsp;相似的问题出现在相互配合的类型选择器上。考虑以下情况：
```css
QPushButton { color: red }
QAbstractButton { color: gray }
```
&emsp;&emsp;两条规则都应用于QPushButton的实例（因为QPushButton继承于QAbstractButton）并且冲突于color属性。因为QPushButton继承于QAbstractButton，这让人不禁想到QPushButton比QAbstractButton更特殊。然而，对于样式表的运算，所有的类型选择器都具有同等的特殊性，并且出现在更后面的规则优先级更高。换句话说，QAbstractButton的color会被设置成灰色，包括QPushButton。如果我们确实想要QPushButton字体颜色设置为红色，我们总是可以使用重新排列样式表规则顺序的方式实现。

为确定规则的特殊性，Qt样式表跟随[CSS2规范](http://www.w3.org/TR/REC-CSS2/cascade.html#specificity)

一个选择器的特殊性由下面的方式计算：
- 计算选择器中ID属性的数量[=a]
- 计算选择器中其他属性和伪类的数量[=b]
- 计算选择器中元素名字的数量[=c]
- 忽略伪原素[如:subcontrol]

串联这三个数字a-b-c（在一个大基数的数字系统）就得到了特殊性等级。

举个例子：
```css
*             {}  /* a=0 b=0 c=0 -> specificity =   0 */
LI            {}  /* a=0 b=0 c=1 -> specificity =   1 */
UL LI         {}  /* a=0 b=0 c=2 -> specificity =   2 */
UL OL+LI      {}  /* a=0 b=0 c=3 -> specificity =   3 */
H1 + *[REL=up]{}  /* a=0 b=1 c=1 -> specificity =  11 */
UL OL LI.red  {}  /* a=0 b=1 c=3 -> specificity =  13 */
LI.red.level  {}  /* a=0 b=2 c=1 -> specificity =  21 */
#x34y         {}  /* a=1 b=0 c=0 -> specificity = 100 */
```
##级联
&emsp;&emsp;样式表可以设置到QApplication、父部件、子部件。任意部件应用的样式表通过合并设置到祖先部件（父亲、祖父等）的样式表获得，以及设置到QApplication上的任何样式表。

&emsp;&emsp;当冲突产生时，部件本身的样式表总是优先于继承来的样式表，不论冲突规则计算的特殊性是多少。同样，父部件的样式表优先级要高于祖父部件的等等。

&emsp;&emsp;这样做的一个后果是，部件上的一个样式规则自动获得高于其他由祖先部件或QApplication样式表指定的样式规则。考虑下面这个例子。首先，我们给QApplication设置样式表：
```css
qApp->setStyleSheet("QPushButton { color: white }");
```
&emsp;&emsp;然后，我们给一个QPushButton对象设置样式表：
```css
myPushButton->setStyleSheet("* { color: blue }");
```
&emsp;&emsp;设置到QPushButton上的样式表强制要求QPushButton（包括其任何子部件）的字体颜色为蓝色，尽管程序范围内的样式表给出了更详尽的样式规则。

&emsp;&emsp;如果我们写成下面这样，效果还是一样的：
```css
myPushButton->setStyleSheet("color: blue");
```
&emsp;&emsp;如果该QPushButton有孩子（这是不太可能的），这段样式表对它们无效。

&emsp;&emsp;样式表级联是一个很复杂的话题。请参考[CSS2规范](http://www.w3.org/TR/CSS2/cascade.html#cascade)获取更多细节。请注意Qt目前没有实现的！很重要。

##继承
&emsp;&emsp;在典型的CSS中，如果一个项的字体和颜色没有显式设置，它会自动从其父亲获得。当使用Qt样式表时，一个部件**不会**从其父亲继承字体和颜色的设置（请注意，父亲和父类、孩子和子类都是不同的概念，不要搞混）。

举个例子，考虑一个QGroupBox内有一个QPushButton：
```css
qApp->setStyleSheet("QGroupBox { color: red; } ");
```
&emsp;&emsp;QPushButton没有任何显式的color设置。因此，它会获得系统的颜色而不是从父亲继承color的值。如果我们要设置QGroupBox及其所有孩子的color，我们可以这样写：
```css
qApp->setStyleSheet("QGroupBox, QGroupBox * { color: red; }");
```
&emsp;&emsp;与此相反，使用QWidget::setFont()可以设置字体包括孩子的字体，使用 QWidget::setPalette()可以设置调色板包括孩子的调色板。

##C++命名空间内的部件
类型选择器可以用于样式化特定的类型，举个例子：
```css
class MyPushButton : public QPushButton {
    // ...
}

// ...
qApp->setStyleSheet("MyPushButton { background: yellow; }");

```
&emsp;&emsp;Qt样式表使用widget的QObject::className()来决定何时应用类型选择器。当自定义部件在命名空间之中，QObject::className()会返回<namespace>::<classname>。这与subcontrol产生了冲突。为了解决这个问题，当为命名空间中widget使用类型选择器时，我们必须将"::"替换成"--"。举个例子：
```css
namespace ns {
    class MyPushButton : public QPushButton {
        // ...
    }
}

// ...
qApp->setStyleSheet("ns--MyPushButton { background: yellow; }");
```

##设置对象的属性
&emsp;&emsp;从4.3以后，任何使用[Q_PROPERTY](http://doc.qt.io/qt-4.8/qobject.html#Q_PROPERTY)宏声明的属性都可以使用**qproperty-&lt;property name&gt;**语法。举个例子：
```css
MyLabel { qproperty-pixmap: url(pixmap.png); }
MyGroupBox { qproperty-titleColor: rgb(100, 200, 100); }
QPushButton { qproperty-iconSize: 20px 20px; }
```

##相关链接
- ###[所有Qt C++类](http://doc.qt.io/qt-4.8/classes.html)
- ###[函数索引](http://doc.qt.io/qt-4.8/functions.html)
- ###[所有Qt模块](http://doc.qt.io/qt-4.8/modules.html)
- ###[所有QML元素](http://doc.qt.io/qt-4.8/qdeclarativeelements.html)
- ###[Qt Quick](http://doc.qt.io/qt-4.8/qtquick.html)
- ###[Qt Creator手册](http://doc.qt.io/qtcreator/index.html)

- - - - -
版权声明：[本文](http://wanqingyang.me/2015/08/10/Qt-Style-Sheets-%E8%AF%AD%E6%B3%95/)为博主原创文章，欢迎转载，转载请注明出处
