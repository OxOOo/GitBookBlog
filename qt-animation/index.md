# Qt中的属性动画

**代码下载<a href="Animation.zip" target="_blank">Animation.zip</a>**

### 什么是属性动画

1. 首先来说，什么是属性？

    举例来说，一个窗口的大小，位置等。

1. 属性动画是什么？

    属性动画就是“属性的动画”。

    比如：窗口在1s之内从A点滑动到B点。

### HelloAnimation

我们先来看一个属性动画的例子：

```c++
#include <QApplication>
#include <QLabel>
#include <QPropertyAnimation>

int main(int argc, char* argv[])
{
    QApplication app(argc, argv);

    // 开始设置窗口
    QLabel w;
    w.setText("Hello Animation");
    w.resize(400, 300);
    w.setAlignment(Qt::AlignCenter);
    w.show();
    // 设置窗口结束

    // 开始设置动画
    QPropertyAnimation animation(&w, "pos"); // 设置动画的对象和属性
    animation.setStartValue(QPoint(200, 200)); // 设置动画开始时的属性值
    animation.setEndValue(QPoint(800, 500)); // 设置动画结束时的属性值
    animation.setDuration(2000); // 设置动画的持续时间
    // 设置动画结束

    // 开始动画
    animation.start();

    return app.exec();
}

```

*此处应有演示*

### KeyStep

属性动画除了可以设置开始时和结束时的属性值，还可以指定某个时间点的属性值。

通过`QPropertyAnimation::setKeyValueAt(qreal step, const QVariant &value)`实现。

```c++
#include <QApplication>
#include <QLabel>
#include <QPropertyAnimation>

int main(int argc, char* argv[])
{
    QApplication app(argc, argv);

    // 开始设置窗口
    QLabel w;
    w.setText("Hello Animation");
    w.resize(400, 300);
    w.setAlignment(Qt::AlignCenter);
    w.show();
    // 设置窗口结束

    // 开始设置动画
    QPropertyAnimation animation(&w, "pos"); // 设置动画的对象和属性
    animation.setStartValue(QPoint(200, 200)); // 设置动画开始时的属性值
    animation.setKeyValueAt(0.5, QPoint(0, 0)); // !!! 修改了此处，设置关键点的属性值
    animation.setEndValue(QPoint(800, 500)); // 设置动画结束时的属性值
    animation.setDuration(2000); // 设置动画的持续时间
    // 设置动画结束

    // 开始动画
    animation.start();

    return app.exec();
}

```

*此处应有演示*

### 缓和曲线

```c++
#include <QApplication>
#include <QLabel>
#include <QPropertyAnimation>

int main(int argc, char* argv[])
{
    QApplication app(argc, argv);

    // 开始设置窗口
    QLabel w;
    w.setText("Hello Animation");
    w.resize(400, 300);
    w.setAlignment(Qt::AlignCenter);
    w.show();
    // 设置窗口结束

    // 开始设置动画
    QPropertyAnimation animation(&w, "pos"); // 设置动画的对象和属性
    animation.setStartValue(QPoint(200, 200)); // 设置动画开始时的属性值
    animation.setEndValue(QPoint(800, 500)); // 设置动画结束时的属性值
    animation.setDuration(2000); // 设置动画的持续时间
    animation.setEasingCurve(QEasingCurve::OutBounce); // !!! 修改了这里，使用缓和曲线
    // 设置动画结束

    // 开始动画
    animation.start();

    return app.exec();
}

```

*此处应有演示*

### 动画组

有时候，我们有很多动画需要以此播放，或者是同时播放，这种情况下就可以使用动画组。

```c++
#include <QApplication>
#include <QLabel>
#include <QPropertyAnimation>
#include <QSequentialAnimationGroup>

int main(int argc, char* argv[])
{
    QApplication app(argc, argv);

    // 开始设置窗口
    QLabel w;
    w.setText("Hello Animation");
    w.resize(400, 300);
    w.setAlignment(Qt::AlignCenter);
    w.show();
    // 设置窗口结束

    // 开始设置动画1
    QPropertyAnimation *animation1 = new QPropertyAnimation(&w, "pos"); // 设置动画的对象和属性
    animation1->setStartValue(QPoint(200, 200)); // 设置动画开始时的属性值
    animation1->setEndValue(QPoint(800, 500)); // 设置动画结束时的属性值
    animation1->setDuration(2000); // 设置动画的持续时间
    // 设置动画结束1

    // 开始设置动画2
    QPropertyAnimation *animation2 = new QPropertyAnimation(&w, "size"); // 设置动画的对象和属性
    animation2->setStartValue(QSize(400, 300)); // 设置动画开始时的属性值
    animation2->setEndValue(QSize(160, 120)); // 设置动画结束时的属性值
    animation2->setDuration(2000); // 设置动画的持续时间
    // 设置动画结束2

    // 将两个动画添加到串行动画组中
    QSequentialAnimationGroup group; // 可以将QSequentialAnimationGroup替换成QParallelAnimationGroup试试
    group.addAnimation(animation1);
    group.addAnimation(animation2);

    // 开始动画
    group.start(); // !!! 注意此处

    return app.exec();
}

```

*此处应有演示*

### 更复杂的情况

在更复杂的情况时，可以使用“状态机”来做，比如样例:animated titles