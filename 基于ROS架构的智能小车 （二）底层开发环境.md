# 基于ROS架构的智能小车 （二）底层开发环境

- stm32单片机

 首先选取底层的驱动的单片机，市场上的单片机五花八门，以降低开发环境的复杂度为原则，我们选择的是**STM32F103C8T6**，（其实我当时在想为什么不用arduion，不过后来感觉还是stm32更好用一点）结合Mbed的在线开发环境开发，用的电路板是梁老师的C8T6的最小系统开发板。

>关于最小系统开发板的介绍，参见**梁老师的执行器电路板讲义.pdf**。

- Mbed在线开发环境
Mbed的开发环境的优点在于开发过程简洁，其编译器是C++编译器，而且Mbed.h的库已经封装了绝大多数我们需要的stm32功能，所以只要懂C++编程就可以上手了。将stm32单片机通过st-link连接到电脑上，(注意是有一种紫色的st-link,好像是v2版,主机是可以把它当作USB来识别的)，在线编译后将生成的.bin，直接下载到单片机上，看到st-link的指示灯闪三次(在firefox上下载)或者两次(在chrome上下载),就表明已经烧写到了单片机中了，这比keil方便很多。但有时侯连续使用时间过长就可能烧写失败，这种情况的话重新拔插一下st-link就好了。

> 初次接触Mbed的话，可以参考张宇翔学长的**mbed培训171013.pdf**,参考做几个学长给的几个例程就会熟悉了，用起来还是非常方便的。

最后对照C8T6最小系统电路板的电路图，查看对应的引脚，将stm32插入到电路板上(注意方向)就可以进行控制了。

- 电机驱动
电机驱动板使用的是L298N直流电机驱动板。

![](https://i.imgur.com/jNydTpr.png)

可以通过输出A和输出B同时驱动两个电机，以一个电机的控制为例子，有两种可以选择的控制方式，一种是两输出的，即两个输出口都是Pwm输出，通过脉宽相对大小控制转向和转速，一般是如果正转的话，一个为0，另一个调节脉宽来调节转速，反转相反。见下图

![](https://i.imgur.com/4jbahqX.png)

另一种则是需要拔出通道使能的短路片，将中间的逻辑输入的两个引脚作为DigitalOut的GPIO口，通过高低电平控制转向，然后让边上的口作为pwm输出调节转速，边上pwm输出前面的引脚是板载5V使能，可以不用管。

> 对应的单片机引脚参照**梁老师的执行器电路板讲义.pdf**中的**Mbed 环境下 STM32F103C8T6 管脚资源表**。

例程的话我封装了两输出[Motor_2](https://os.mbed.com/users/himarsmty/code/motor_2/ "Motor_2的库")的库，可以参考自己写出三输出的库。

> 关于L298N驱动板的介绍，也可以参考[https://www.ncnynl.com/archives/201612/1204.html](https://www.ncnynl.com/archives/201612/1204.html "搭建ROS小车底盘-第一篇硬件")

---------------------------------

到此为止，我们的底层的开发环境就基本搭建完成了，底层开发调试的话通过串口打印Serial类调试是很有帮助的，也可以学习一下。下一步，我们讲底盘的选择和控制。