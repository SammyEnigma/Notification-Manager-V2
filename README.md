Qt通知控件
===

仿Windows通知，多个通知显示，定时消失，支持出现/消失动画

和之前版本相比，使用了另一套控件：InteractiveButton，提高交互操作

允许两种使用方式：

- 局部通知，通过信号槽和 Lambda 直接获取通知的操作方式
- 全部通知，触发信号给其他控件使用

另外支持卡片本身、附加三个按钮的点击事件

![picture](picture.gif)



## 部署

开发环境：Qt13.1（理论上相近版本亦可）

## 用法

### 头文件

需要在工程pro文件中包含头文件

```C++
#include "tipbox.h"
```



### 初始化

```C++
tip_box = new TipBox(this);
connect(tip_box, &TipBox::signalCardClicked, [=](NotificationEntry* n){
    qDebug() << n->toString();
});
connect(tip_box, &TipBox::signalBtnClicked, [=](NotificationEntry* n){
    qDebug() << n->toString();
});
```



### 添加通知

在窗口右下角添加一个通知卡片，5秒钟后定时消失

```C++
NotificationEntry* noti = new NotificationEntry("key", "title", "content1");
noti->setBtn(1, "btn1", "cmd1"); // 全局信号可接收
noti->setBtn(2, "btn2", "cmd2");
tip_box->createTipCard(noti);

connect(noti, &NotificationEntry::signalCardClicked, [=]{
    qDebug() << "通知被点击";
});
connect(noti, &NotificationEntry::signalBtnClicked, [=](int x){
    qDebug() << "按钮被点击" << x;
});
```



### 自动调整大小

在修改窗口大小的时候，固定通知Box在右下角

```C++
void MainWindow::resizeEvent(QResizeEvent*)
{
    tip_box->adjustPosition();
}
```

