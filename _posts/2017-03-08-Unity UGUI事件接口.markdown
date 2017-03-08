---
layout: post
title: "Unity EventSystem中的事件接口"
excerpt: "介绍了Unity中的事件接口以及使用方法，这些事件接口在声明类的后面可以被继承，继承之后便可以使用相对应的事件了。"
categories: [Unity]
comments: true

---
### 目前Unity 的事件系统中包含的事件接口有如下
***

1. IPointerEnterHandler：指针进入事件
2. IPointerExitHandler：指针退出事件
3. IPointerDownHandler：指针按下事件
4. IPointerUpHandler：指针抬起事件
5. IPointerClickHandler：指针点击事件
6. IInitializePotentialDragHandler：初始化潜在的拖动事件
7. IBeginDragHandler：拖动开始事件
8. IDragHandler：拖动中事件
9. IEndDragHandler：拖动结束事件
10. IDropHandler：接收拖动事件
11. IScrollHandler：滚动事件
12. ISelectHandler：选择事件
13. IDeselectHandler：取消选择事件
14. IUpdateSelectedHandler：选中物体每帧触发事件
15. IMoveHandler：移动事件(上下左右)
16. ISubmitHandler：提交事件
17. ICancelHandler：取消事件

***

这些事件接口在声明类的后面可以被继承，继承之后便可以使用相对应的事件了。
事件系统只针对UI有效。
